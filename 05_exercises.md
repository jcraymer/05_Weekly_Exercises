---
title: 'Weekly Exercises #5'
author: "Juliet Cramer"
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
library(googlesheets4) # for reading googlesheet data
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
library(shiny)         # for creating interactive apps
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
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
lettuce_bar_graph <- garden_harvest %>% 
  filter(vegetable == "lettuce") %>%
  ggplot(aes(y=fct_rev(fct_infreq(variety)))) +
  geom_bar(fill="darkgreen") + 
  labs(title = "How Many Times Did Lisa Harvest Each Different Type of Lettuce?" ,
       x = "count",
       y = "Variety")
  ggplotly(lettuce_bar_graph)
```

<!--html_preserve--><div id="htmlwidget-7f72ce7719475ea5077e" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-7f72ce7719475ea5077e">{"x":{"data":[{"orientation":"v","width":[1,3,9,27,29],"base":[0.55,1.55,2.55,3.55,4.55],"x":[0.5,1.5,4.5,13.5,14.5],"y":[0.9,0.9,0.9,0.9,0.9],"text":["count:  1<br />fct_rev(fct_infreq(variety)): 0.9","count:  3<br />fct_rev(fct_infreq(variety)): 0.9","count:  9<br />fct_rev(fct_infreq(variety)): 0.9","count: 27<br />fct_rev(fct_infreq(variety)): 0.9","count: 29<br />fct_rev(fct_infreq(variety)): 0.9"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,100,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":148.310502283105},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"How Many Times Did Lisa Harvest Each Different Type of Lettuce?","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.45,30.45],"tickmode":"array","ticktext":["0","10","20","30"],"tickvals":[0,10,20,30],"categoryorder":"array","categoryarray":["0","10","20","30"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,5.6],"tickmode":"array","ticktext":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"tickvals":[1,2,3,4,5],"categoryorder":"array","categoryarray":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Variety","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"7529311defb6":{"y":{},"type":"bar"}},"cur_data":"7529311defb6","visdat":{"7529311defb6":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
lettuce_weight_distribution <- garden_harvest %>% 
  filter(vegetable %in% c("lettuce")) %>%
  ggplot(aes(x=weight)) +
  geom_histogram(fill="lightblue",
         color="white", 
         bins = 30) + 
  labs(title = "Distribution Weight of Lettuce Harvests",
       x = "Weight (g)")
ggplotly(lettuce_weight_distribution)
```

<!--html_preserve--><div id="htmlwidget-df8db6ba539ebf7df0e4" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-df8db6ba539ebf7df0e4">{"x":{"data":[{"orientation":"v","width":[10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[10.8275862068966,21.6551724137931,32.4827586206897,43.3103448275862,54.1379310344828,64.9655172413793,75.7931034482758,86.6206896551724,97.448275862069,108.275862068966,119.103448275862,129.931034482759,140.758620689655,151.586206896552,162.413793103448,173.241379310345,184.068965517241,194.896551724138,205.724137931034,216.551724137931,227.379310344828,238.206896551724,249.034482758621,259.862068965517,270.689655172414,281.51724137931,292.344827586207,303.172413793103,314,324.827586206897],"y":[7,9,1,6,6,6,6,7,6,3,2,2,3,1,0,0,1,0,0,2,0,0,0,0,0,0,0,0,0,1],"text":["count: 7<br />weight:  10.82759","count: 9<br />weight:  21.65517","count: 1<br />weight:  32.48276","count: 6<br />weight:  43.31034","count: 6<br />weight:  54.13793","count: 6<br />weight:  64.96552","count: 6<br />weight:  75.79310","count: 7<br />weight:  86.62069","count: 6<br />weight:  97.44828","count: 3<br />weight: 108.27586","count: 2<br />weight: 119.10345","count: 2<br />weight: 129.93103","count: 3<br />weight: 140.75862","count: 1<br />weight: 151.58621","count: 0<br />weight: 162.41379","count: 0<br />weight: 173.24138","count: 1<br />weight: 184.06897","count: 0<br />weight: 194.89655","count: 0<br />weight: 205.72414","count: 2<br />weight: 216.55172","count: 0<br />weight: 227.37931","count: 0<br />weight: 238.20690","count: 0<br />weight: 249.03448","count: 0<br />weight: 259.86207","count: 0<br />weight: 270.68966","count: 0<br />weight: 281.51724","count: 0<br />weight: 292.34483","count: 0<br />weight: 303.17241","count: 0<br />weight: 314.00000","count: 1<br />weight: 324.82759"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(173,216,230,1)","line":{"width":1.88976377952756,"color":"rgba(255,255,255,1)"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":43.1050228310502},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Distribution Weight of Lettuce Harvests","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-10.8275862068965,346.48275862069],"tickmode":"array","ticktext":["0","100","200","300"],"tickvals":[0,100,200,300],"categoryorder":"array","categoryarray":["0","100","200","300"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Weight (g)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.45,9.45],"tickmode":"array","ticktext":["0.0","2.5","5.0","7.5"],"tickvals":[0,2.5,5,7.5],"categoryorder":"array","categoryarray":["0.0","2.5","5.0","7.5"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"7529432098f3":{"x":{},"type":"bar"}},"cur_data":"7529432098f3","visdat":{"7529432098f3":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).



```r
small_trains %>% 
  filter(year == 2018, departure_station == c("REIMS", "PARIS EST", "PARIS LYON"))%>%
  select(-delay_cause, -delayed_number) %>% 
  dplyr::distinct() %>% 
  group_by(departure_station, month) %>%
  summarise(prop_late_departure = sum(num_late_at_departure)/sum(total_num_trips)) %>%
  ggplot(aes(x = month, 
             y = prop_late_departure, 
             fill = departure_station)) + 
  geom_area() + 
  labs(title = "Proportion of Late Departures for Each Month For 3 Stations", 
       x = " ", 
       y = " ")
```

![](05_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->



## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tomato_reveal<-garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>%
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>% 
  mutate(total = cumsum(daily_harvest_lb)) %>%
  ungroup() %>% 
  mutate(variety = fct_reorder(variety, total, max)) %>% 
  ggplot(aes(x = date, 
             y = total, 
             fill = variety)) +
  geom_area() +
  labs(title = "Cumulative Harvest of Tomatoes") +
  transition_reveal(date)
  anim_save("tomato_reveal.gif", tomato_reveal)
```


```r
knitr::include_graphics("tomato_reveal.gif")
```

![](tomato_reveal.gif)<!-- -->

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
mallorca_map <- get_stamenmap(bbox = c(left = 2.2, bottom = 39.2, right = 3.7, top = 40), 
    maptype = "terrain",
    zoom = 9)
```


```r
ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
            aes(x = lon, y = lat, color = ele)) + 
  geom_point(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele),
            size = .3, color = "red") +
  labs(title = "Mallorca Bike Ride", 
       subtitle = "Time: {frame_along}") +
  transition_reveal(time)
  anim_save("mallorca_map.gif")
```


```r
knitr::include_graphics("../../mallorca_map.gif")
```

![](../../mallorca_map.gif)<!-- -->

  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
new_data <- bind_rows(panama_swim, panama_bike, panama_run)
panama<- get_stamenmap(bbox = c(left = min(new_data$lon), bottom = min(new_data$lat), right = max(new_data$lon), top =  max(new_data$lat)), map_type = "terrain", zoom = 13)

ggmap(panama) + 
  geom_path(data = new_data, 
            aes(x = lon, y = lat)) + 
  geom_point(data = new_data, 
             aes(x = lon, y = lat, color = event),
            size = 3) +
  labs(title = "Heather's Rout of the Iornman", 
       subtitle = "Time: {frame_along}") + 
  theme_map() +
  transition_reveal(time)
  anim_save("panama.gif")
```


```r
knitr::include_graphics("../../panama.gif")
```

![](../../panama.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.


```r
covid19 %>%
  group_by(state) %>% 
  mutate(cases_last_week = lag(cases, 7, order_by = date)) %>%
  replace_na(list(cases_last_week = 0)) %>%
  mutate(new_cases = cases - cases_last_week) %>% 
  filter(cases >= 20) %>% 
  ggplot(aes(x = cases, y = new_cases, group = state)) + 
  geom_path() +
  geom_point(size = .8, color = "red") + 
  geom_text(aes(label = state), check_overlap = TRUE) + 
  labs(title = "Covid-19 Cases", 
       subtitle = "Time: {frame_along}") + 
  scale_x_log10(label = scales::comma) + 
  scale_y_log10(label = scales::comma) + 
  transition_reveal(date)
  anim_save("covid.gif")
```


```r
knitr::include_graphics("../../covid.gif")
```

![](../../covid.gif)<!-- -->

  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see. The code below gives the population estimates for each state. Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays. HINT: use `group = date` in `aes()`.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
  covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>% 
  mutate(num_cases_per_10000 = (cases/est_pop_2018)*10000, day_week = wday(date, label = TRUE)) %>%
  filter(day_week == "Fri") %>% 
  ggplot() + 
  geom_map(map = states_map, 
           aes(map_id = state, 
               fill = num_cases_per_10000, group = date)) +
 # creates the map "background"
  expand_limits(x = states_map$long, y = states_map$lat) + 
  labs(title = "Recent Cumulative Number of COVID-19 Cases / 10000 People", 
       fill = "Number of cases per 10000",
       subtitle = "Time: {closest_state}" ) + 
  theme_map() + 
  theme(legend.background = element_blank()) +
  transition_states(date)
  anim_save("covid_map.gif")
```


```r
knitr::include_graphics("../../covid_map.gif")
```

![](../../covid_map.gif)<!-- -->

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed.
  

  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.




**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
