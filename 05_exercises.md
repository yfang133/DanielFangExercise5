---
title: 'Weekly Exercises #5'
author: "Daniel Fang"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
library(knitr)
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("mallorca_bike_day7.csv") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
library(babynames)
baby_graph <- babynames %>% 
  group_by(year) %>% 
  summarize(totalbabies= sum(n)) %>% 
  arrange(desc(totalbabies)) %>% 
  ggplot(aes(x=year, y=totalbabies))+
  geom_line()+
  labs(x="year", y="count", title = "The number of total babies born from 1880 to present")

 baby_graph
```

![](05_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
 ggplotly(baby_graph)
```

<!--html_preserve--><div id="htmlwidget-dcad557366d9d971fdec" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-dcad557366d9d971fdec">{"x":{"data":[{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[201484,192696,221533,216946,243462,240854,255317,247394,299473,288946,301401,286672,334375,325223,338690,351022,357485,346957,381458,339233,450283,345814,386736,381204,403489,423931,428461,465395,488652,511228,590715,644279,988064,1137111,1416343,1832461,1934434,2006784,2171150,2110266,2262681,2334511,2289196,2302527,2381674,2333326,2295937,2319274,2260839,2192051,2223159,2104071,2111081,1999141,2076507,2089596,2077372,2130370,2212302,2203276,2302378,2436060,2731457,2822127,2689817,2652638,3195046,3602032,3452217,3484531,3503700,3683862,3799172,3852228,3979884,4014041,4121070,4200007,4131657,4156434,4154377,4140244,4035234,3958791,3887800,3626029,3475674,3395130,3378876,3476215,3607420,3432585,3143627,3017412,3040409,3019910,3034949,3176864,3174268,3327416,3444537,3459182,3507664,3462826,3487820,3568075,3555871,3604403,3693471,3843559,3950992,3894329,3840196,3769282,3716758,3661351,3646362,3624799,3677107,3692537,3778079,3741451,3736042,3799971,3818361,3842004,3952932,3994007,3926358,3815638,3690700,3651914,3650462,3637310,3696311,3688687,3652968,3546301],"text":["year: 1880<br />totalbabies:  201484","year: 1881<br />totalbabies:  192696","year: 1882<br />totalbabies:  221533","year: 1883<br />totalbabies:  216946","year: 1884<br />totalbabies:  243462","year: 1885<br />totalbabies:  240854","year: 1886<br />totalbabies:  255317","year: 1887<br />totalbabies:  247394","year: 1888<br />totalbabies:  299473","year: 1889<br />totalbabies:  288946","year: 1890<br />totalbabies:  301401","year: 1891<br />totalbabies:  286672","year: 1892<br />totalbabies:  334375","year: 1893<br />totalbabies:  325223","year: 1894<br />totalbabies:  338690","year: 1895<br />totalbabies:  351022","year: 1896<br />totalbabies:  357485","year: 1897<br />totalbabies:  346957","year: 1898<br />totalbabies:  381458","year: 1899<br />totalbabies:  339233","year: 1900<br />totalbabies:  450283","year: 1901<br />totalbabies:  345814","year: 1902<br />totalbabies:  386736","year: 1903<br />totalbabies:  381204","year: 1904<br />totalbabies:  403489","year: 1905<br />totalbabies:  423931","year: 1906<br />totalbabies:  428461","year: 1907<br />totalbabies:  465395","year: 1908<br />totalbabies:  488652","year: 1909<br />totalbabies:  511228","year: 1910<br />totalbabies:  590715","year: 1911<br />totalbabies:  644279","year: 1912<br />totalbabies:  988064","year: 1913<br />totalbabies: 1137111","year: 1914<br />totalbabies: 1416343","year: 1915<br />totalbabies: 1832461","year: 1916<br />totalbabies: 1934434","year: 1917<br />totalbabies: 2006784","year: 1918<br />totalbabies: 2171150","year: 1919<br />totalbabies: 2110266","year: 1920<br />totalbabies: 2262681","year: 1921<br />totalbabies: 2334511","year: 1922<br />totalbabies: 2289196","year: 1923<br />totalbabies: 2302527","year: 1924<br />totalbabies: 2381674","year: 1925<br />totalbabies: 2333326","year: 1926<br />totalbabies: 2295937","year: 1927<br />totalbabies: 2319274","year: 1928<br />totalbabies: 2260839","year: 1929<br />totalbabies: 2192051","year: 1930<br />totalbabies: 2223159","year: 1931<br />totalbabies: 2104071","year: 1932<br />totalbabies: 2111081","year: 1933<br />totalbabies: 1999141","year: 1934<br />totalbabies: 2076507","year: 1935<br />totalbabies: 2089596","year: 1936<br />totalbabies: 2077372","year: 1937<br />totalbabies: 2130370","year: 1938<br />totalbabies: 2212302","year: 1939<br />totalbabies: 2203276","year: 1940<br />totalbabies: 2302378","year: 1941<br />totalbabies: 2436060","year: 1942<br />totalbabies: 2731457","year: 1943<br />totalbabies: 2822127","year: 1944<br />totalbabies: 2689817","year: 1945<br />totalbabies: 2652638","year: 1946<br />totalbabies: 3195046","year: 1947<br />totalbabies: 3602032","year: 1948<br />totalbabies: 3452217","year: 1949<br />totalbabies: 3484531","year: 1950<br />totalbabies: 3503700","year: 1951<br />totalbabies: 3683862","year: 1952<br />totalbabies: 3799172","year: 1953<br />totalbabies: 3852228","year: 1954<br />totalbabies: 3979884","year: 1955<br />totalbabies: 4014041","year: 1956<br />totalbabies: 4121070","year: 1957<br />totalbabies: 4200007","year: 1958<br />totalbabies: 4131657","year: 1959<br />totalbabies: 4156434","year: 1960<br />totalbabies: 4154377","year: 1961<br />totalbabies: 4140244","year: 1962<br />totalbabies: 4035234","year: 1963<br />totalbabies: 3958791","year: 1964<br />totalbabies: 3887800","year: 1965<br />totalbabies: 3626029","year: 1966<br />totalbabies: 3475674","year: 1967<br />totalbabies: 3395130","year: 1968<br />totalbabies: 3378876","year: 1969<br />totalbabies: 3476215","year: 1970<br />totalbabies: 3607420","year: 1971<br />totalbabies: 3432585","year: 1972<br />totalbabies: 3143627","year: 1973<br />totalbabies: 3017412","year: 1974<br />totalbabies: 3040409","year: 1975<br />totalbabies: 3019910","year: 1976<br />totalbabies: 3034949","year: 1977<br />totalbabies: 3176864","year: 1978<br />totalbabies: 3174268","year: 1979<br />totalbabies: 3327416","year: 1980<br />totalbabies: 3444537","year: 1981<br />totalbabies: 3459182","year: 1982<br />totalbabies: 3507664","year: 1983<br />totalbabies: 3462826","year: 1984<br />totalbabies: 3487820","year: 1985<br />totalbabies: 3568075","year: 1986<br />totalbabies: 3555871","year: 1987<br />totalbabies: 3604403","year: 1988<br />totalbabies: 3693471","year: 1989<br />totalbabies: 3843559","year: 1990<br />totalbabies: 3950992","year: 1991<br />totalbabies: 3894329","year: 1992<br />totalbabies: 3840196","year: 1993<br />totalbabies: 3769282","year: 1994<br />totalbabies: 3716758","year: 1995<br />totalbabies: 3661351","year: 1996<br />totalbabies: 3646362","year: 1997<br />totalbabies: 3624799","year: 1998<br />totalbabies: 3677107","year: 1999<br />totalbabies: 3692537","year: 2000<br />totalbabies: 3778079","year: 2001<br />totalbabies: 3741451","year: 2002<br />totalbabies: 3736042","year: 2003<br />totalbabies: 3799971","year: 2004<br />totalbabies: 3818361","year: 2005<br />totalbabies: 3842004","year: 2006<br />totalbabies: 3952932","year: 2007<br />totalbabies: 3994007","year: 2008<br />totalbabies: 3926358","year: 2009<br />totalbabies: 3815638","year: 2010<br />totalbabies: 3690700","year: 2011<br />totalbabies: 3651914","year: 2012<br />totalbabies: 3650462","year: 2013<br />totalbabies: 3637310","year: 2014<br />totalbabies: 3696311","year: 2015<br />totalbabies: 3688687","year: 2016<br />totalbabies: 3652968","year: 2017<br />totalbabies: 3546301"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":54.7945205479452},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"The number of total babies born from 1880 to present","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1873.15,2023.85],"tickmode":"array","ticktext":["1880","1920","1960","2000"],"tickvals":[1880,1920,1960,2000],"categoryorder":"array","categoryarray":["1880","1920","1960","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"year","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-7669.55000000002,4400372.55],"tickmode":"array","ticktext":["0e+00","1e+06","2e+06","3e+06","4e+06"],"tickvals":[0,1000000,2000000,3000000,4000000],"categoryorder":"array","categoryarray":["0e+00","1e+06","2e+06","3e+06","4e+06"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"588c441b10f0":{"x":{},"y":{},"type":"scatter"}},"cur_data":"588c441b10f0","visdat":{"588c441b10f0":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
  

```r
topfive<-babynames %>% 
  group_by(year, sex) %>% 
  top_n(5,wt = n) %>% 
  summarise(topprop=sum(prop)) %>% 
  ggplot(aes(y=topprop, x=year)) +
  geom_line() +
  facet_wrap(~sex)+
  labs(x="year", y="Proportion", title = "The proportion of babies using the top five popular names across time")

topfive
```

![](05_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggplotly(topfive)
```

<!--html_preserve--><div id="htmlwidget-2731421750b90720a0d3" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-2731421750b90720a0d3">{"x":{"data":[{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.15733826,0.15336604,0.15371452,0.14971806,0.14867792,0.14537718,0.14302534,0.1406622,0.13698435,0.13512914,0.13142352,0.1313045,0.12849413,0.12903139,0.12508741,0.12526204,0.12706305,0.12641224,0.12531371,0.1272617,0.12346735,0.12470646,0.1253362,0.12604782,0.12645843,0.1260549,0.1285222,0.1274916,0.13008736,0.13211228,0.13368182,0.14110335,0.14469962,0.14856783,0.15182917,0.155428,0.1562639,0.15708047,0.15654148,0.15501069,0.15820886,0.15795597,0.1546151,0.15512911,0.15580078,0.15487124,0.15230637,0.15313366,0.15049065,0.14846556,0.14664577,0.14845169,0.15062075,0.14849569,0.14910392,0.1653253,0.15869459,0.15387533,0.15097616,0.14633748,0.1439578,0.14638839,0.15054653,0.15282031,0.15178253,0.15248012,0.15245235,0.1684526,0.16857285,0.16207023,0.15548231,0.15093026,0.14561663,0.14158578,0.13781677,0.13205107,0.12262228,0.11197175,0.10620866,0.10090156,0.09543854,0.09544018,0.0947038,0.09647524,0.09355687,0.09990811,0.09719181,0.09549955,0.09548775,0.09596011,0.09744915,0.10259564,0.10704177,0.10459675,0.1050265,0.10396556,0.10315652,0.09799833,0.0950487,0.09986868,0.10419573,0.10632166,0.10536197,0.10836369,0.10814581,0.10939796,0.11230832,0.11378722,0.10257414,0.09934284,0.09194181,0.08386043,0.07556316,0.07170849,0.06754764,0.06346912,0.05978772,0.05708431,0.05546932,0.05425125,0.05248133,0.05093735,0.04925589,0.05104653,0.04907801,0.0469792,0.04513969,0.04391392,0.04275158,0.04470194,0.0476606,0.04824275,0.04915199,0.04857068,0.0471749,0.04598787,0.04457753,0.04491068],"text":["year: 1880<br />topprop: 0.15733826","year: 1881<br />topprop: 0.15336604","year: 1882<br />topprop: 0.15371452","year: 1883<br />topprop: 0.14971806","year: 1884<br />topprop: 0.14867792","year: 1885<br />topprop: 0.14537718","year: 1886<br />topprop: 0.14302534","year: 1887<br />topprop: 0.14066220","year: 1888<br />topprop: 0.13698435","year: 1889<br />topprop: 0.13512914","year: 1890<br />topprop: 0.13142352","year: 1891<br />topprop: 0.13130450","year: 1892<br />topprop: 0.12849413","year: 1893<br />topprop: 0.12903139","year: 1894<br />topprop: 0.12508741","year: 1895<br />topprop: 0.12526204","year: 1896<br />topprop: 0.12706305","year: 1897<br />topprop: 0.12641224","year: 1898<br />topprop: 0.12531371","year: 1899<br />topprop: 0.12726170","year: 1900<br />topprop: 0.12346735","year: 1901<br />topprop: 0.12470646","year: 1902<br />topprop: 0.12533620","year: 1903<br />topprop: 0.12604782","year: 1904<br />topprop: 0.12645843","year: 1905<br />topprop: 0.12605490","year: 1906<br />topprop: 0.12852220","year: 1907<br />topprop: 0.12749160","year: 1908<br />topprop: 0.13008736","year: 1909<br />topprop: 0.13211228","year: 1910<br />topprop: 0.13368182","year: 1911<br />topprop: 0.14110335","year: 1912<br />topprop: 0.14469962","year: 1913<br />topprop: 0.14856783","year: 1914<br />topprop: 0.15182917","year: 1915<br />topprop: 0.15542800","year: 1916<br />topprop: 0.15626390","year: 1917<br />topprop: 0.15708047","year: 1918<br />topprop: 0.15654148","year: 1919<br />topprop: 0.15501069","year: 1920<br />topprop: 0.15820886","year: 1921<br />topprop: 0.15795597","year: 1922<br />topprop: 0.15461510","year: 1923<br />topprop: 0.15512911","year: 1924<br />topprop: 0.15580078","year: 1925<br />topprop: 0.15487124","year: 1926<br />topprop: 0.15230637","year: 1927<br />topprop: 0.15313366","year: 1928<br />topprop: 0.15049065","year: 1929<br />topprop: 0.14846556","year: 1930<br />topprop: 0.14664577","year: 1931<br />topprop: 0.14845169","year: 1932<br />topprop: 0.15062075","year: 1933<br />topprop: 0.14849569","year: 1934<br />topprop: 0.14910392","year: 1935<br />topprop: 0.16532530","year: 1936<br />topprop: 0.15869459","year: 1937<br />topprop: 0.15387533","year: 1938<br />topprop: 0.15097616","year: 1939<br />topprop: 0.14633748","year: 1940<br />topprop: 0.14395780","year: 1941<br />topprop: 0.14638839","year: 1942<br />topprop: 0.15054653","year: 1943<br />topprop: 0.15282031","year: 1944<br />topprop: 0.15178253","year: 1945<br />topprop: 0.15248012","year: 1946<br />topprop: 0.15245235","year: 1947<br />topprop: 0.16845260","year: 1948<br />topprop: 0.16857285","year: 1949<br />topprop: 0.16207023","year: 1950<br />topprop: 0.15548231","year: 1951<br />topprop: 0.15093026","year: 1952<br />topprop: 0.14561663","year: 1953<br />topprop: 0.14158578","year: 1954<br />topprop: 0.13781677","year: 1955<br />topprop: 0.13205107","year: 1956<br />topprop: 0.12262228","year: 1957<br />topprop: 0.11197175","year: 1958<br />topprop: 0.10620866","year: 1959<br />topprop: 0.10090156","year: 1960<br />topprop: 0.09543854","year: 1961<br />topprop: 0.09544018","year: 1962<br />topprop: 0.09470380","year: 1963<br />topprop: 0.09647524","year: 1964<br />topprop: 0.09355687","year: 1965<br />topprop: 0.09990811","year: 1966<br />topprop: 0.09719181","year: 1967<br />topprop: 0.09549955","year: 1968<br />topprop: 0.09548775","year: 1969<br />topprop: 0.09596011","year: 1970<br />topprop: 0.09744915","year: 1971<br />topprop: 0.10259564","year: 1972<br />topprop: 0.10704177","year: 1973<br />topprop: 0.10459675","year: 1974<br />topprop: 0.10502650","year: 1975<br />topprop: 0.10396556","year: 1976<br />topprop: 0.10315652","year: 1977<br />topprop: 0.09799833","year: 1978<br />topprop: 0.09504870","year: 1979<br />topprop: 0.09986868","year: 1980<br />topprop: 0.10419573","year: 1981<br />topprop: 0.10632166","year: 1982<br />topprop: 0.10536197","year: 1983<br />topprop: 0.10836369","year: 1984<br />topprop: 0.10814581","year: 1985<br />topprop: 0.10939796","year: 1986<br />topprop: 0.11230832","year: 1987<br />topprop: 0.11378722","year: 1988<br />topprop: 0.10257414","year: 1989<br />topprop: 0.09934284","year: 1990<br />topprop: 0.09194181","year: 1991<br />topprop: 0.08386043","year: 1992<br />topprop: 0.07556316","year: 1993<br />topprop: 0.07170849","year: 1994<br />topprop: 0.06754764","year: 1995<br />topprop: 0.06346912","year: 1996<br />topprop: 0.05978772","year: 1997<br />topprop: 0.05708431","year: 1998<br />topprop: 0.05546932","year: 1999<br />topprop: 0.05425125","year: 2000<br />topprop: 0.05248133","year: 2001<br />topprop: 0.05093735","year: 2002<br />topprop: 0.04925589","year: 2003<br />topprop: 0.05104653","year: 2004<br />topprop: 0.04907801","year: 2005<br />topprop: 0.04697920","year: 2006<br />topprop: 0.04513969","year: 2007<br />topprop: 0.04391392","year: 2008<br />topprop: 0.04275158","year: 2009<br />topprop: 0.04470194","year: 2010<br />topprop: 0.04766060","year: 2011<br />topprop: 0.04824275","year: 2012<br />topprop: 0.04915199","year: 2013<br />topprop: 0.04857068","year: 2014<br />topprop: 0.04717490","year: 2015<br />topprop: 0.04598787","year: 2016<br />topprop: 0.04457753","year: 2017<br />topprop: 0.04491068"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.30057433,0.29583865,0.2870664,0.2850894,0.27490264,0.26950709,0.26744567,0.26221036,0.2541607,0.25177263,0.24738307,0.24280418,0.23809271,0.23417685,0.23039722,0.22783731,0.22481425,0.22371291,0.22171926,0.21562074,0.21682816,0.21006963,0.20865537,0.205473,0.20224102,0.19869727,0.19834665,0.19584833,0.19646207,0.19421374,0.19441208,0.19512417,0.19216891,0.19366448,0.19607216,0.19491338,0.1967567,0.19972064,0.20169375,0.20299762,0.21071168,0.21253757,0.21402412,0.21650321,0.21964029,0.22029538,0.21989846,0.2192253,0.21693506,0.21829951,0.21954593,0.21993085,0.21754481,0.21756594,0.21675027,0.21802583,0.21961999,0.22022199,0.2188485,0.21862718,0.21975057,0.21947803,0.22264304,0.2243406,0.22492094,0.22369821,0.22404892,0.21547425,0.21063197,0.20724374,0.20620735,0.20546305,0.20653744,0.20617153,0.20391865,0.202201,0.1966413,0.19200817,0.18814848,0.18404795,0.18258241,0.18572921,0.18436946,0.18508832,0.18905703,0.18534516,0.18480652,0.18482965,0.18331326,0.17810367,0.17030816,0.15811044,0.15539014,0.15292053,0.15583907,0.15156415,0.14845658,0.14656296,0.14432225,0.13966989,0.13326298,0.13150606,0.13490779,0.13602413,0.13658916,0.13115299,0.12623232,0.12382538,0.12061185,0.11607743,0.11130972,0.10488336,0.09593638,0.09052041,0.08575546,0.08393706,0.08107929,0.08040606,0.07836623,0.07485677,0.0706849,0.06682913,0.06393464,0.06094372,0.05804828,0.05440458,0.0504976,0.04887724,0.04664777,0.04534693,0.04467473,0.04488391,0.04438888,0.04405199,0.04334179,0.04239007,0.04117921,0.04083146],"text":["year: 1880<br />topprop: 0.30057433","year: 1881<br />topprop: 0.29583865","year: 1882<br />topprop: 0.28706640","year: 1883<br />topprop: 0.28508940","year: 1884<br />topprop: 0.27490264","year: 1885<br />topprop: 0.26950709","year: 1886<br />topprop: 0.26744567","year: 1887<br />topprop: 0.26221036","year: 1888<br />topprop: 0.25416070","year: 1889<br />topprop: 0.25177263","year: 1890<br />topprop: 0.24738307","year: 1891<br />topprop: 0.24280418","year: 1892<br />topprop: 0.23809271","year: 1893<br />topprop: 0.23417685","year: 1894<br />topprop: 0.23039722","year: 1895<br />topprop: 0.22783731","year: 1896<br />topprop: 0.22481425","year: 1897<br />topprop: 0.22371291","year: 1898<br />topprop: 0.22171926","year: 1899<br />topprop: 0.21562074","year: 1900<br />topprop: 0.21682816","year: 1901<br />topprop: 0.21006963","year: 1902<br />topprop: 0.20865537","year: 1903<br />topprop: 0.20547300","year: 1904<br />topprop: 0.20224102","year: 1905<br />topprop: 0.19869727","year: 1906<br />topprop: 0.19834665","year: 1907<br />topprop: 0.19584833","year: 1908<br />topprop: 0.19646207","year: 1909<br />topprop: 0.19421374","year: 1910<br />topprop: 0.19441208","year: 1911<br />topprop: 0.19512417","year: 1912<br />topprop: 0.19216891","year: 1913<br />topprop: 0.19366448","year: 1914<br />topprop: 0.19607216","year: 1915<br />topprop: 0.19491338","year: 1916<br />topprop: 0.19675670","year: 1917<br />topprop: 0.19972064","year: 1918<br />topprop: 0.20169375","year: 1919<br />topprop: 0.20299762","year: 1920<br />topprop: 0.21071168","year: 1921<br />topprop: 0.21253757","year: 1922<br />topprop: 0.21402412","year: 1923<br />topprop: 0.21650321","year: 1924<br />topprop: 0.21964029","year: 1925<br />topprop: 0.22029538","year: 1926<br />topprop: 0.21989846","year: 1927<br />topprop: 0.21922530","year: 1928<br />topprop: 0.21693506","year: 1929<br />topprop: 0.21829951","year: 1930<br />topprop: 0.21954593","year: 1931<br />topprop: 0.21993085","year: 1932<br />topprop: 0.21754481","year: 1933<br />topprop: 0.21756594","year: 1934<br />topprop: 0.21675027","year: 1935<br />topprop: 0.21802583","year: 1936<br />topprop: 0.21961999","year: 1937<br />topprop: 0.22022199","year: 1938<br />topprop: 0.21884850","year: 1939<br />topprop: 0.21862718","year: 1940<br />topprop: 0.21975057","year: 1941<br />topprop: 0.21947803","year: 1942<br />topprop: 0.22264304","year: 1943<br />topprop: 0.22434060","year: 1944<br />topprop: 0.22492094","year: 1945<br />topprop: 0.22369821","year: 1946<br />topprop: 0.22404892","year: 1947<br />topprop: 0.21547425","year: 1948<br />topprop: 0.21063197","year: 1949<br />topprop: 0.20724374","year: 1950<br />topprop: 0.20620735","year: 1951<br />topprop: 0.20546305","year: 1952<br />topprop: 0.20653744","year: 1953<br />topprop: 0.20617153","year: 1954<br />topprop: 0.20391865","year: 1955<br />topprop: 0.20220100","year: 1956<br />topprop: 0.19664130","year: 1957<br />topprop: 0.19200817","year: 1958<br />topprop: 0.18814848","year: 1959<br />topprop: 0.18404795","year: 1960<br />topprop: 0.18258241","year: 1961<br />topprop: 0.18572921","year: 1962<br />topprop: 0.18436946","year: 1963<br />topprop: 0.18508832","year: 1964<br />topprop: 0.18905703","year: 1965<br />topprop: 0.18534516","year: 1966<br />topprop: 0.18480652","year: 1967<br />topprop: 0.18482965","year: 1968<br />topprop: 0.18331326","year: 1969<br />topprop: 0.17810367","year: 1970<br />topprop: 0.17030816","year: 1971<br />topprop: 0.15811044","year: 1972<br />topprop: 0.15539014","year: 1973<br />topprop: 0.15292053","year: 1974<br />topprop: 0.15583907","year: 1975<br />topprop: 0.15156415","year: 1976<br />topprop: 0.14845658","year: 1977<br />topprop: 0.14656296","year: 1978<br />topprop: 0.14432225","year: 1979<br />topprop: 0.13966989","year: 1980<br />topprop: 0.13326298","year: 1981<br />topprop: 0.13150606","year: 1982<br />topprop: 0.13490779","year: 1983<br />topprop: 0.13602413","year: 1984<br />topprop: 0.13658916","year: 1985<br />topprop: 0.13115299","year: 1986<br />topprop: 0.12623232","year: 1987<br />topprop: 0.12382538","year: 1988<br />topprop: 0.12061185","year: 1989<br />topprop: 0.11607743","year: 1990<br />topprop: 0.11130972","year: 1991<br />topprop: 0.10488336","year: 1992<br />topprop: 0.09593638","year: 1993<br />topprop: 0.09052041","year: 1994<br />topprop: 0.08575546","year: 1995<br />topprop: 0.08393706","year: 1996<br />topprop: 0.08107929","year: 1997<br />topprop: 0.08040606","year: 1998<br />topprop: 0.07836623","year: 1999<br />topprop: 0.07485677","year: 2000<br />topprop: 0.07068490","year: 2001<br />topprop: 0.06682913","year: 2002<br />topprop: 0.06393464","year: 2003<br />topprop: 0.06094372","year: 2004<br />topprop: 0.05804828","year: 2005<br />topprop: 0.05440458","year: 2006<br />topprop: 0.05049760","year: 2007<br />topprop: 0.04887724","year: 2008<br />topprop: 0.04664777","year: 2009<br />topprop: 0.04534693","year: 2010<br />topprop: 0.04467473","year: 2011<br />topprop: 0.04488391","year: 2012<br />topprop: 0.04438888","year: 2013<br />topprop: 0.04405199","year: 2014<br />topprop: 0.04334179","year: 2015<br />topprop: 0.04239007","year: 2016<br />topprop: 0.04117921","year: 2017<br />topprop: 0.04083146"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":55.4520547945205,"r":7.30593607305936,"b":40.1826484018265,"l":43.1050228310502},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"The proportion of babies using the top five popular names across time","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,0.491846053489889],"automargin":true,"type":"linear","autorange":false,"range":[1873.15,2023.85],"tickmode":"array","ticktext":["1880","1920","1960","2000"],"tickvals":[1880,1920,1960,2000],"categoryorder":"array","categoryarray":["1880","1920","1960","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":"","hoverformat":".2f"},"annotations":[{"text":"year","x":0.5,"y":-0.0353881278538813,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"top","annotationType":"axis"},{"text":"Proportion","x":-0.0318003913894325,"y":0.5,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-90,"xanchor":"right","yanchor":"center","annotationType":"axis"},{"text":"F","x":0.245923026744945,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"M","x":0.754076973255055,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"}],"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.0278443165,0.3135614735],"tickmode":"array","ticktext":["0.1","0.2","0.3"],"tickvals":[0.1,0.2,0.3],"categoryorder":"array","categoryarray":["0.1","0.2","0.3"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.491846053489889,"y0":0,"y1":1},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.491846053489889,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":1,"y0":0,"y1":1},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":1,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"}],"xaxis2":{"type":"linear","autorange":false,"range":[1873.15,2023.85],"tickmode":"array","ticktext":["1880","1920","1960","2000"],"tickvals":[1880,1920,1960,2000],"categoryorder":"array","categoryarray":["1880","1920","1960","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.508153946510111,1],"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":"","hoverformat":".2f"},"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"588c9141923":{"x":{},"y":{},"type":"scatter"}},"cur_data":"588c9141923","visdat":{"588c9141923":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
trainly <- small_trains %>%
  group_by(departure_station) %>%
  summarize(tottrips = sum(total_num_trips)) %>%
  ggplot() +
  geom_col(aes(x = tottrips, y = fct_reorder(departure_station, tottrips, .desc = FALSE), text = departure_station)) +
  labs(title = "The busiest departure stations in Europe", x = "Total Trips", y = "")

ggplotly(trainly, toolkip = c("text", "x"))
```

<!--html_preserve--><div id="htmlwidget-b6cc0c4283a86421c85b" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-b6cc0c4283a86421c85b">{"x":{"data":[{"orientation":"v","width":[116346,121890,87540,51204,91350,155658,4734,65094,55968,208434,66420,58692,121356,54780,69372,34542,61890,63168,24414,58740,34386,62358,60090,123240,291264,542352,56976,1854,44514,351096,81312,186198,85908,80064,167286,58950,97722,429174,1791168,1498554,370950,8952,46662,131244,62100,57468,191304,29736,27486,117396,142854,37788,78282,48954,3462,53376,76782,68274,42072],"base":[40.55,43.55,37.55,14.55,38.55,47.55,2.55,28.55,17.55,51.55,29.55,20.55,42.55,16.55,31.55,8.55,24.55,27.55,4.55,21.55,7.55,26.55,23.55,44.55,52.55,56.55,18.55,0.55,11.55,53.55,35.55,49.55,36.55,34.55,48.55,22.55,39.55,55.55,58.55,57.55,54.55,3.55,12.55,45.55,25.55,19.55,50.55,6.55,5.55,41.55,46.55,9.55,33.55,13.55,1.55,15.55,32.55,30.55,10.55],"x":[58173,60945,43770,25602,45675,77829,2367,32547,27984,104217,33210,29346,60678,27390,34686,17271,30945,31584,12207,29370,17193,31179,30045,61620,145632,271176,28488,927,22257,175548,40656,93099,42954,40032,83643,29475,48861,214587,895584,749277,185475,4476,23331,65622,31050,28734,95652,14868,13743,58698,71427,18894,39141,24477,1731,26688,38391,34137,21036],"y":[0.900000000000006,0.900000000000006,0.900000000000006,0.899999999999999,0.900000000000006,0.900000000000006,0.9,0.899999999999999,0.899999999999999,0.900000000000006,0.899999999999999,0.899999999999999,0.900000000000006,0.899999999999999,0.900000000000002,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.900000000000006,0.900000000000006,0.900000000000006,0.899999999999999,0.9,0.899999999999999,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.899999999999999,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.900000000000006,0.9,0.899999999999999,0.900000000000006,0.899999999999999,0.899999999999999,0.900000000000006,0.9,0.9,0.900000000000006,0.900000000000006,0.899999999999999,0.900000000000006,0.899999999999999,0.9,0.899999999999999,0.900000000000006,0.899999999999999,0.899999999999999],"text":["tottrips:  116346<br />fct_reorder(departure_station, tottrips, .desc = FALSE): AIX EN PROVENCE TGV<br />AIX EN PROVENCE TGV","tottrips:  121890<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ANGERS SAINT LAUD<br />ANGERS SAINT LAUD","tottrips:   87540<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ANGOULEME<br />ANGOULEME","tottrips:   51204<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ANNECY<br />ANNECY","tottrips:   91350<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ARRAS<br />ARRAS","tottrips:  155658<br />fct_reorder(departure_station, tottrips, .desc = FALSE): AVIGNON TGV<br />AVIGNON TGV","tottrips:    4734<br />fct_reorder(departure_station, tottrips, .desc = FALSE): BARCELONA<br />BARCELONA","tottrips:   65094<br />fct_reorder(departure_station, tottrips, .desc = FALSE): BELLEGARDE (AIN)<br />BELLEGARDE (AIN)","tottrips:   55968<br />fct_reorder(departure_station, tottrips, .desc = FALSE): BESANCON FRANCHE COMTE TGV<br />BESANCON FRANCHE COMTE TGV","tottrips:  208434<br />fct_reorder(departure_station, tottrips, .desc = FALSE): BORDEAUX ST JEAN<br />BORDEAUX ST JEAN","tottrips:   66420<br />fct_reorder(departure_station, tottrips, .desc = FALSE): BREST<br />BREST","tottrips:   58692<br />fct_reorder(departure_station, tottrips, .desc = FALSE): CHAMBERY CHALLES LES EAUX<br />CHAMBERY CHALLES LES EAUX","tottrips:  121356<br />fct_reorder(departure_station, tottrips, .desc = FALSE): DIJON VILLE<br />DIJON VILLE","tottrips:   54780<br />fct_reorder(departure_station, tottrips, .desc = FALSE): DOUAI<br />DOUAI","tottrips:   69372<br />fct_reorder(departure_station, tottrips, .desc = FALSE): DUNKERQUE<br />DUNKERQUE","tottrips:   34542<br />fct_reorder(departure_station, tottrips, .desc = FALSE): FRANCFORT<br />FRANCFORT","tottrips:   61890<br />fct_reorder(departure_station, tottrips, .desc = FALSE): GENEVE<br />GENEVE","tottrips:   63168<br />fct_reorder(departure_station, tottrips, .desc = FALSE): GRENOBLE<br />GRENOBLE","tottrips:   24414<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ITALIE<br />ITALIE","tottrips:   58740<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LA ROCHELLE VILLE<br />LA ROCHELLE VILLE","tottrips:   34386<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LAUSANNE<br />LAUSANNE","tottrips:   62358<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LAVAL<br />LAVAL","tottrips:   60090<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LE CREUSOT MONTCEAU MONTCHANIN<br />LE CREUSOT MONTCEAU MONTCHANIN","tottrips:  123240<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LE MANS<br />LE MANS","tottrips:  291264<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LILLE<br />LILLE","tottrips:  542352<br />fct_reorder(departure_station, tottrips, .desc = FALSE): LYON PART DIEU<br />LYON PART DIEU","tottrips:   56976<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MACON LOCHE<br />MACON LOCHE","tottrips:    1854<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MADRID<br />MADRID","tottrips:   44514<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MARNE LA VALLEE<br />MARNE LA VALLEE","tottrips:  351096<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MARSEILLE ST CHARLES<br />MARSEILLE ST CHARLES","tottrips:   81312<br />fct_reorder(departure_station, tottrips, .desc = FALSE): METZ<br />METZ","tottrips:  186198<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MONTPELLIER<br />MONTPELLIER","tottrips:   85908<br />fct_reorder(departure_station, tottrips, .desc = FALSE): MULHOUSE VILLE<br />MULHOUSE VILLE","tottrips:   80064<br />fct_reorder(departure_station, tottrips, .desc = FALSE): NANCY<br />NANCY","tottrips:  167286<br />fct_reorder(departure_station, tottrips, .desc = FALSE): NANTES<br />NANTES","tottrips:   58950<br />fct_reorder(departure_station, tottrips, .desc = FALSE): NICE VILLE<br />NICE VILLE","tottrips:   97722<br />fct_reorder(departure_station, tottrips, .desc = FALSE): NIMES<br />NIMES","tottrips:  429174<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PARIS EST<br />PARIS EST","tottrips: 1791168<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PARIS LYON<br />PARIS LYON","tottrips: 1498554<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PARIS MONTPARNASSE<br />PARIS MONTPARNASSE","tottrips:  370950<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PARIS NORD<br />PARIS NORD","tottrips:    8952<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PARIS VAUGIRARD<br />PARIS VAUGIRARD","tottrips:   46662<br />fct_reorder(departure_station, tottrips, .desc = FALSE): PERPIGNAN<br />PERPIGNAN","tottrips:  131244<br />fct_reorder(departure_station, tottrips, .desc = FALSE): POITIERS<br />POITIERS","tottrips:   62100<br />fct_reorder(departure_station, tottrips, .desc = FALSE): QUIMPER<br />QUIMPER","tottrips:   57468<br />fct_reorder(departure_station, tottrips, .desc = FALSE): REIMS<br />REIMS","tottrips:  191304<br />fct_reorder(departure_station, tottrips, .desc = FALSE): RENNES<br />RENNES","tottrips:   29736<br />fct_reorder(departure_station, tottrips, .desc = FALSE): SAINT ETIENNE CHATEAUCREUX<br />SAINT ETIENNE CHATEAUCREUX","tottrips:   27486<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ST MALO<br />ST MALO","tottrips:  117396<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ST PIERRE DES CORPS<br />ST PIERRE DES CORPS","tottrips:  142854<br />fct_reorder(departure_station, tottrips, .desc = FALSE): STRASBOURG<br />STRASBOURG","tottrips:   37788<br />fct_reorder(departure_station, tottrips, .desc = FALSE): STUTTGART<br />STUTTGART","tottrips:   78282<br />fct_reorder(departure_station, tottrips, .desc = FALSE): TOULON<br />TOULON","tottrips:   48954<br />fct_reorder(departure_station, tottrips, .desc = FALSE): TOULOUSE MATABIAU<br />TOULOUSE MATABIAU","tottrips:    3462<br />fct_reorder(departure_station, tottrips, .desc = FALSE): TOURCOING<br />TOURCOING","tottrips:   53376<br />fct_reorder(departure_station, tottrips, .desc = FALSE): TOURS<br />TOURS","tottrips:   76782<br />fct_reorder(departure_station, tottrips, .desc = FALSE): VALENCE ALIXAN TGV<br />VALENCE ALIXAN TGV","tottrips:   68274<br />fct_reorder(departure_station, tottrips, .desc = FALSE): VANNES<br />VANNES","tottrips:   42072<br />fct_reorder(departure_station, tottrips, .desc = FALSE): ZURICH<br />ZURICH"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":186.301369863014},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"The busiest departure stations in Europe","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-89558.4,1880726.4],"tickmode":"array","ticktext":["0","500000","1000000","1500000"],"tickvals":[0,500000,1000000,1500000],"categoryorder":"array","categoryarray":["0","500000","1000000","1500000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Total Trips","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,59.6],"tickmode":"array","ticktext":["MADRID","TOURCOING","BARCELONA","PARIS VAUGIRARD","ITALIE","ST MALO","SAINT ETIENNE CHATEAUCREUX","LAUSANNE","FRANCFORT","STUTTGART","ZURICH","MARNE LA VALLEE","PERPIGNAN","TOULOUSE MATABIAU","ANNECY","TOURS","DOUAI","BESANCON FRANCHE COMTE TGV","MACON LOCHE","REIMS","CHAMBERY CHALLES LES EAUX","LA ROCHELLE VILLE","NICE VILLE","LE CREUSOT MONTCEAU MONTCHANIN","GENEVE","QUIMPER","LAVAL","GRENOBLE","BELLEGARDE (AIN)","BREST","VANNES","DUNKERQUE","VALENCE ALIXAN TGV","TOULON","NANCY","METZ","MULHOUSE VILLE","ANGOULEME","ARRAS","NIMES","AIX EN PROVENCE TGV","ST PIERRE DES CORPS","DIJON VILLE","ANGERS SAINT LAUD","LE MANS","POITIERS","STRASBOURG","AVIGNON TGV","NANTES","MONTPELLIER","RENNES","BORDEAUX ST JEAN","LILLE","MARSEILLE ST CHARLES","PARIS NORD","PARIS EST","LYON PART DIEU","PARIS MONTPARNASSE","PARIS LYON"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59],"categoryorder":"array","categoryarray":["MADRID","TOURCOING","BARCELONA","PARIS VAUGIRARD","ITALIE","ST MALO","SAINT ETIENNE CHATEAUCREUX","LAUSANNE","FRANCFORT","STUTTGART","ZURICH","MARNE LA VALLEE","PERPIGNAN","TOULOUSE MATABIAU","ANNECY","TOURS","DOUAI","BESANCON FRANCHE COMTE TGV","MACON LOCHE","REIMS","CHAMBERY CHALLES LES EAUX","LA ROCHELLE VILLE","NICE VILLE","LE CREUSOT MONTCEAU MONTCHANIN","GENEVE","QUIMPER","LAVAL","GRENOBLE","BELLEGARDE (AIN)","BREST","VANNES","DUNKERQUE","VALENCE ALIXAN TGV","TOULON","NANCY","METZ","MULHOUSE VILLE","ANGOULEME","ARRAS","NIMES","AIX EN PROVENCE TGV","ST PIERRE DES CORPS","DIJON VILLE","ANGERS SAINT LAUD","LE MANS","POITIERS","STRASBOURG","AVIGNON TGV","NANTES","MONTPELLIER","RENNES","BORDEAUX ST JEAN","LILLE","MARSEILLE ST CHARLES","PARIS NORD","PARIS EST","LYON PART DIEU","PARIS MONTPARNASSE","PARIS LYON"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"588c75ae3266":{"x":{},"y":{},"text":{},"type":"bar"}},"cur_data":"588c75ae3266","visdat":{"588c75ae3266":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
train_subset <- small_trains %>% 
  filter(departure_station %in% c("PARIS LYON", "PARIS MONTPARNASSE", 
                          "LYON PART DIEU", "PARIS EST", "PARIS NORD")) %>% 
  group_by(departure_station, year) %>%
  summarize(tottrips = sum(total_num_trips)) 

train_subset
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["departure_station"],"name":[1],"type":["chr"],"align":["left"]},{"label":["year"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["tottrips"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"LYON PART DIEU","2":"2015","3":"138528"},{"1":"LYON PART DIEU","2":"2016","3":"130002"},{"1":"LYON PART DIEU","2":"2017","3":"132618"},{"1":"LYON PART DIEU","2":"2018","3":"141204"},{"1":"PARIS EST","2":"2015","3":"103758"},{"1":"PARIS EST","2":"2016","3":"106008"},{"1":"PARIS EST","2":"2017","3":"113526"},{"1":"PARIS EST","2":"2018","3":"105882"},{"1":"PARIS LYON","2":"2015","3":"461220"},{"1":"PARIS LYON","2":"2016","3":"437592"},{"1":"PARIS LYON","2":"2017","3":"458214"},{"1":"PARIS LYON","2":"2018","3":"434142"},{"1":"PARIS MONTPARNASSE","2":"2015","3":"383946"},{"1":"PARIS MONTPARNASSE","2":"2016","3":"370176"},{"1":"PARIS MONTPARNASSE","2":"2017","3":"393048"},{"1":"PARIS MONTPARNASSE","2":"2018","3":"351384"},{"1":"PARIS NORD","2":"2015","3":"95664"},{"1":"PARIS NORD","2":"2016","3":"89436"},{"1":"PARIS NORD","2":"2017","3":"96486"},{"1":"PARIS NORD","2":"2018","3":"89364"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
train_subset %>% 
  ggplot(aes(x = year, 
             y = tottrips,
             color = departure_station,
             shape = departure_station)) +
  geom_line() +
  geom_text(aes(label = departure_station)) +
  labs(title = "Total Trips by Station", 
       x = "",
       y = "",
       color = "departure_station") +
  theme(legend.position = "top",
        legend.title = element_blank()) +
  transition_reveal(year)
```

![](05_exercises_files/figure-html/unnamed-chunk-5-1.gif)<!-- -->


## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  mutate(cum_harvest = cumsum(daily_harvest_lb))%>%
  ggplot(aes(x = date, y = cum_harvest, fill = fct_reorder(variety, cum_harvest))) +
  geom_area()+
  labs(y = "Total Harvest in Pounds", x = "Date")+
  scale_fill_discrete(name = "variety")+
  transition_reveal(date)
```

![](05_exercises_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("tomatoes.gif")
```


```r
include_graphics("tomatoes.gif")
```

![](tomatoes.gif)<!-- -->



## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.38, bottom = 39.55, right = 2.62, top = 39.7), 
    maptype = "terrain",
    zoom = 11
)
```
  

```r
mallorca_map <- ggmap(mallorca_map)+
  geom_point(data=mallorca_bike_day7, 
             aes(x=lon,y=lat), color = 'red', size = 1.5)+
   geom_line(data=mallorca_bike_day7, 
             aes(x=lon,y=lat, color=ele), 
             alpha=.7, size = .5) +
  labs(x="Longitude", y="Latitude")+
  transition_reveal(time) +
  ggtitle("Time:{frame_along}")

animate(mallorca_map)
anim_save("mallorca_map.gif")
```


```r
include_graphics("mallorca_map.gif")
```

![](mallorca_map.gif)<!-- -->

**I would prefer this animation over the static map because this is a much more straightforward and efficient way to visualize the cycling track.**

  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
race <- bind_rows(panama_swim, panama_bike, panama_run )
```


```r
racemap <- get_stamenmap(
    bbox = c(left = -79.56, bottom = 8.9, right = -79.5, top = 9), 
    maptype = "terrain",
    zoom = 13
)
```


```r
  ggmap(racemap)+
  geom_point(data=race, 
             aes(x=lon,y=lat,  color = event),size = 2)+
   geom_path(data=race, 
             aes(x=lon,y=lat, color=event), 
             alpha=.7, size = .5) +
  labs(x="Longitude", y="Latitude")+
  transition_reveal(time) +
  ggtitle("Time:{frame_along}")

anim_save("panamarace.gif")
```

```r
include_graphics("panamarace.gif")
```

![](panamarace.gif)<!-- -->

  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
cum <- covid19 %>%
  group_by(state) %>%
  mutate(sevendaylag = lag(cases, 7, order_by = date))%>%
  replace_na(list(sevendaylag = 0))%>%
  ungroup()%>%
  mutate(new = cases - sevendaylag, totcases = cumsum(cases))%>%
  filter(totcases >= 20)%>%
  ggplot(aes(x=totcases, y = new, group = state))+
  geom_point()+
  geom_path()+
  geom_text(aes(label = state), color = "red", check_overlap = TRUE)+
  scale_x_log10(labels = scales::comma)+
  scale_y_log10(labels = scales::comma)+
  labs(title = "COVID Cases in the US (Cumulative and New", x = "Total Cases", y = "New Cases",
       subtitle = "Time:{frame_along}")+
  transition_reveal(date)

animate(cum, nframes = 200, duration = 30)

anim_save("covid19.gif")
```
  

```r
include_graphics("covid19.gif")
```

![](covid19.gif)<!-- -->
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  



```r
census_pop_est_2018 <- read_csv("us_census_2018_state_pop_est.csv") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
```


```r
covid19map <- covid19 %>%
  mutate(state = str_to_lower(state))%>%
  left_join(census_pop_est_2018)%>%
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000, dayofweek = wday(date))%>%
  filter(dayofweek == 5)%>%
  ggplot() +
  scale_fill_viridis_c(option = "C")+
  geom_map(aes(group = date, map_id = state, fill = cases_per_10000), map = states_map, color = "white")+
  expand_limits(x = states_map$long, y = states_map$lat)+
  theme_map()+
  labs(subtitle = "Date:{frame_time}")+
  transition_time(date)

animate(covid19map, nframes = 200, end_pause = 10)

anim_save("covid19map.gif")
```


```r
include_graphics("covid19map.gif")
```

![](covid19map.gif)<!-- -->


## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


https://github.com/yfang133/DanielFangExercise5

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
