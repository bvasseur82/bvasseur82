---
title: Analising Adam Scott's PGA Tour stats
author: Benoit Vasseur
date: '2021-01-30'
slug: adam-scott-pga
categories: 
  - rstats
  - golf
tags:
  - rstats
  - golf
subtitle: ''
summary: Look into Adam Scott's PGA tour stats to explain his success 
authors: []
lastmod: ''
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---




Lately I've been publishing [screencasts](https://juliasilge.com/category/tidymodels/) demonstrating how to use the [tidymodels](https://www.tidymodels.org/) framework, from starting out with first modeling steps to tuning more complex models. Today's screencast walks through how to tune and choose hyperparameters using this week's [`#TidyTuesday` dataset](https://github.com/rfordatascience/tidytuesday) on NCAA women's basketball tournaments. ⛳

`{{% youtube "krw7OkUCk84" %}}`{=html}

</br>

Here is the code I used in the video, for those who prefer reading instead of or in addition to video.

## Explore the data

Our modeling goal is to estimate the relationship of [expected tournament wins by seed from this week's #TidyTuesday dataset](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-10-06/readme.md). This is similar to the ["average" column in the FiveThirtyEight table in this article](https://fivethirtyeight.com/features/tom-izzo-is-the-best-coach-in-modern-ncaa-tournament-history-by-far/). This was what I was most interested in when I saw this data, but I was pretty confused about what was going on this table at first! Many thanks to [Tom Mock](https://themockup.blog/) for helping out my understanding.

Let's start by reading in the data.


```r
library(tidyverse)
tournament <- read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-10-06/tournament.csv')

tournament
```

```
## # A tibble: 2,092 x 19
##     year school  seed conference conf_w conf_l conf_percent conf_place reg_w
##    <dbl> <chr>  <dbl> <chr>       <dbl>  <dbl>        <dbl> <chr>      <dbl>
##  1  1982 Arizo…     4 Western C…     NA     NA         NA   -             23
##  2  1982 Auburn     7 Southeast…     NA     NA         NA   -             24
##  3  1982 Cheyn…     2 Independe…     NA     NA         NA   -             24
##  4  1982 Clems…     5 Atlantic …      6      3         66.7 4th           20
##  5  1982 Drake      4 Missouri …     NA     NA         NA   -             26
##  6  1982 East …     6 Independe…     NA     NA         NA   -             19
##  7  1982 Georg…     5 Southeast…     NA     NA         NA   -             21
##  8  1982 Howard     8 Mid-Easte…     NA     NA         NA   -             14
##  9  1982 Illin…     7 Big Ten        NA     NA         NA   -             21
## 10  1982 Jacks…     7 Southwest…     NA     NA         NA   -             28
## # … with 2,082 more rows, and 10 more variables: reg_l <dbl>,
## #   reg_percent <dbl>, how_qual <chr>, x1st_game_at_home <chr>,
## #   tourney_w <dbl>, tourney_l <dbl>, tourney_finish <chr>, full_w <dbl>,
## #   full_l <dbl>, full_percent <dbl>
```

We can look at the mean wins by seed.


```r
tournament %>%
  group_by(seed) %>% 
  summarise(exp_wins = mean(tourney_w, na.rm = TRUE)) %>% 
  ggplot(aes(seed, exp_wins)) + 
  geom_point(alpha = 0.8, size = 3) +
  labs(y = "tournament wins (mean)")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="2400" />

Let's visualize all the tournament results, not just the averages.


```r
tournament %>%
  ggplot(aes(seed, tourney_w)) +
  geom_bin2d(binwidth = c(1, 1), alpha = 0.8) +
  scale_fill_gradient(low = "gray85", high = "midnightblue") +
  labs(fill = "number of\nteams", y = "tournament wins")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="2100" />

We have a lot of options to deal with data like this (curvy, integers, all greater than zero) but one straightforward option are [splines](https://www.tmwr.org/recipes.html#spline-functions). Splines aren't perfect for this because they aren't constrained to stay greater than zero or to always decrease, but they work pretty well and can be used in lots of situations. We have to choose the **degrees of freedom** for the splines.


```r
library(splines)

plot_smoother <- function(deg_free) {
  p <- ggplot(tournament, aes(seed, tourney_w)) + 
    geom_bin2d(binwidth = c(1, 1), alpha = 0.8) +
    scale_fill_gradient(low = "gray85", high = "midnightblue") +
    geom_smooth(
      method = lm, se = FALSE, color = "black",
      formula = y ~ ns(x, df = deg_free)
    ) +
    labs(fill = "number of\nteams", y = "tournament wins",
         title = paste(deg_free, "spline terms"))
  
  print(p)
}

walk(c(2, 4, 6, 8, 10, 15), plot_smoother)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-.gif" width="2100" />
