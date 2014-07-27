---
title       : Visualizing international migration flows
subtitle    : Coursera - Developing Data Products course
author      : asheshwor
job         : ninja
framework   : html5slides
theme       : uulm # {uulm}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : monokai      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained
knit        : slidify::knit2slides
---

## 1 | Shiny app pitch - introduction

* A shiny app for visualizing international migration flows to and from a region using UN migrants stock data.

* Colors to visualize direction of movement, and number of arcs to visualize size of movement.

* It's very easy to use, just select the region, theme, year to update the output. That's it!

<center>**Try it at** https://asheshwor.shinyapps.io/migrationviz/</center>

<center>![App screenshot](pictures/screenshot.jpg)
<small>Screenshot of options</small></center>
<pre align='right'><small>@asheshwor | 2014-07-27 </small></pre>

--- .class #id bg:#F0F0F0

## 2 | Getting the data

* The migration data was obtained from [United Nations, Department of Economic and Social Affairs, Population Division] (http://esa.un.org/unmigration/TIMSA2013/data/UN_MigrantStock_2013.xls)

* The world map shape file was obtained from [NaturalEarthData.com] (naturalearthdata.com). The location and population of cities were obtained from the cities database at [geonames.org] (geonames.org)

* Code for reading migration data, world map shape file and cities database:


```r
data2013 <- read.xlsx2("data/UN_MigrantStockByOriginAndDestination_2013.xls",
                       sheetName = "Table 10",
                       startRow = 16, colIndex = c(2, 4 , 10:241),
                       colClasses = c("character", rep("numeric", 232)))
wmap <- readShapeSpatial("data/110m_cultural/ne_110m_admin_0_countries.shp")
places <- read.csv("data/cities1000.csv", header=FALSE, stringsAsFactors=FALSE)
```

--- .class #id bg:#F0F0F0

## 3 | Data processing

* With some processing, a data-frame with the required arc connections is created. Following is an example a section of the dataframe for Australia


```
##   source destination stock lat.d lon.d lat.s lon.s stocklog id
## 1     AD          AU    22   -27   133  42.5   1.5        3  1
## 2     AD          AU    22   -27   133  42.5   1.5        3  1
## 3     AD          AU    22   -27   133  42.5   1.5        3  1
## 4     AE          AU  5890   -27   133  24.0  54.0        9  2
## 5     AE          AU  5890   -27   133  24.0  54.0        9  2
## 6     AE          AU  5890   -27   133  24.0  54.0        9  2
## 7     AE          AU  5890   -27   133  24.0  54.0        9  2
## 8     AE          AU  5890   -27   133  24.0  54.0        9  2
```

* In the next step, the source and destination coordinates are replaced with locations of cities in the from the country or region.

--- .class #id bg:#F0F0F0

## 4 | Migrant origin and destination points

* The coordinates for each origin or destination in a region are sampled from 15 most populated cities in that region with probability based on the population.

* Since the sampling is done at random, the map generated each time is different.

<center>![More populated cities are more likely to get selected](pictures/australia.jpg)</center>
<center><small>Cities with higher population are more likely to get selected</small></center>

--- .class #id bg:#F0F0F0

## 5 | Generating the final map

* Finally the great circle arcs obtained using ```gcIntermediate``` function are and plotted using ```ggplot2``` over the world map.

<img src="assets/fig/final-plot.png" title="plot of chunk final-plot" alt="plot of chunk final-plot" style="display: block; margin: auto;" />

<center><small>An example plot for Australia with 'light' map theme</small></center>

<center>Try the app at https://asheshwor.shinyapps.io/migrationviz/</center>
