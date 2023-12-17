# Mars Weather Analysis

## Overview
This repository uses splinter, beautiful soup, pandas, and matplotlib to scrape the following websites for information about Mars, then answer research questions with the scraped data:
<ul>
    <li><a href='https://static.bc-edx.com/data/web/mars_news/index.html'>Mars Planet Science</a></li>
    <li><a href='https://static.bc-edx.com/data/web/mars_facts/temperature.html'>Mars Temperature Data</a></li>
</ul>

## Part 1: Mars News Website Scraping

Section one utilizes splinter and beautiful soup to scrape the [Mars Planet Science](https://static.bc-edx.com/data/web/mars_news/index.html) website. This website contains several news stories regarding NASA's Mars program and lists a title and short blurb for each article. 

The [part_1_mars_news](part_1_mars_news.ipynb) notebook first creates a browser object, using splinter, then reads the HTML into a beautiful soup object for parsing. Beautiful Soup is then used to extract the code from all divs in the "list_text" class. From this object, we then isolate the titles (within class: "content_title") and previews (within class 'article_teaser_body'). 

Once Isolated, these two elements are placed within a dictionary which is then printed in the notebook, and dumped to the json file: [articles.json](Data_Output/articles.json).

## Part 2: Mars Weather Data Scraping

This section uses the [part_2_mars_weather](part_2_mars_weather.ipynb) notebook. 

Section two utilizes splinter, beautiful soup, pandas, and matplotlib to scrape [Mars Temperature Data](https://static.bc-edx.com/data/web/mars_facts/temperature.html) for analysis. This website contains a table which was used to answer the following questions:

<ol>
    <li>1.How many months exist on Mars?</li>
    <li>2.How many Martian days(sols) worth of data exist in the scraped dataset?</li>
    <li>3.What are the coldest and warmest months on Mars?</li>
    <li>4.Which months have the highest/lowest average atmospheric pressures?</li>
    <li>5.How many terrestrial days are in a martian year?</li>

In order to utilize this information, first, a browser object was created using splinter to open the website and a Beautiful Soup object parsed the HTML. From this object, the table rows were separated into headers('th' elements) and data-rows (using the class descriptor: 'data-row'). Each of these were cast into lists and then a pandas dataframe was created for the data, using the header list as column names.

Once in a dataframe, the datatypes were changed to the appropriate type for each column as follows:

<table>
    <tr>
        <th>id</th>
        <td>object</td>                          
    </tr>
    <tr>
        <th>terrestrial_date</th>    
        <td>datetime64[ns]</td>
    </tr>
    <tr>
        <th>sol</th>
        <td>int64</td>
    </tr>
    <tr>
        <th>ls</th>
        <td>int64</td>
    </tr>
    <tr>
        <th>month</th>
        <td>int64</td>
    </tr>
    <tr>
        <th>min_temp</th>
        <td>float64</td>
    </tr>
    <tr>
        <th>pressure</th>
        <td>float64</td>
    </tr>
</table>

Finally, the data is analyzed utilizing matplotlib plots and answers the questions as follows:

### 1.How many months exist on Mars?

Using the <code>df.month.nunique()</code> we gathered that there were 12 months on Mars, with the following number of observations for each month:

<table>
     <tr>
        <th>Month</th>
        <th>Number of Observations</th>
    </tr>
    <tr>
        <th>1</th>
        <td>174</td>
    </tr>
     <tr>
        <th>2</th>
        <td>178</td>
    </tr>
     <tr>
        <th>3</th>
        <td>192</td>
    </tr>
     <tr>
        <th>4</th>
        <td>194</td>
    </tr>
     <tr>
        <th>5</th>
        <td>149</td>
    </tr>
     <tr>
        <th>6</th>
        <td>147</td>
    </tr>
     <tr>
        <th>7</th>
        <td>142</td>
    </tr>
     <tr>
        <th>8</th>
        <td>141</td>
    </tr>
     <tr>
        <th>9</th>
        <td>134</td>
    </tr>
     <tr>
        <th>10</th>
        <td>112</td>
    </tr>
     <tr>
        <th>11</th>
        <td>138</td>
    </tr>
     <tr>
        <th>12</th>
        <td>166</td>
    </tr>    
</table>


### 2.How many Martian days(sols) worth of data exist in the scraped dataset?

This was answered with the <code>df['sol'].count()</code> function to return <b>1867 Martian Days ('sols')</b>

### 3.What are the coldest and warmest months on Mars?

In this section, a groupby object was made from the dataframe with the following code:

<code>avg_temps = df.groupby('month')['min_temp'].mean()</code>

The groupby object was then plotted into a bar chart using matplotlib. This visualization is within the: [temp_by_month_unsorted.png](Visualizations/temp_by_month_unsorted.png) file.

The values were then sorted into the [temp_by_month_sorted.png](Visualizations/temp_by_month_sorted.png) file, leading the the following conclusions:

<b>Coldest Month: 3</b>
<b>Warmest Month: 8</b>

### 4.Which months have the highest/lowest average atmospheric pressures?

A similar group by object was created in this section (substiting pressure for min_temp). This was sorted and then plotted, resulting in the plot that can be found in the [pressure_by_month_sorted.png](Visualizations/pressure_by_month_sorted.png) file.

This chart illustrates the following trends in average pressure by month:

<b>Highest Pressure: 9</b>
<b>Lowest Pressure: 6</b>

### 5.How many terrestrial days are in a martian year?

For the analysis of terrestrial days to Martian years, it was taken into account that each of the measurements in the dataset was sequentially taken on terrestrial days. Once this was known, the min_temps were plotted against the number of measurements, resulting in the [temp_by_terrestrial_day.png](Visualizations/temp_by_terrestrial_day.png) chart.

As can be seen from the chart, there are disctinct peaks and valleys in the minimum temperature, from that, one can assume the relative distance of the sun from Curiosity. Assuming the orbit was geosyncronous (moving with the planet's orbit), the distance from each of these peaks can be seen as the planet's full rotation around the sun (and thus one year). 

Based on this rationale, it was seen that there was a temperature peak at 750 days, and another at 1425, leading to the calculation that a Martian year is approximately <b>675 days</b>.

Once the analysis was completed, the dataframe was saved to a CSV and can be found at [martian_weather.csv](Data_Output/martian_weather.csv)
