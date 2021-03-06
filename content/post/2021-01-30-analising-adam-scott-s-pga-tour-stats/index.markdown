---
title: Analising Adam Scott's PGA Tour stats
author: Benoit Vasseur
date: '2021-02-06'
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



[Adam Scott](https://www.pgatour.com/players/player.24502.adam-scott.html) is known for his great-looking golf swing and being one of the nicest guy on tour. Since the early 2000s Adam has had tremendous success across worldwide tours. This post will dig further into Adam's PGA tour stats and will try to explain the keys of his success.⛳

<img src="as_masters.jpg" width="100%" />

*Photo by Harry How/Getty Images*

## Explore the data

Throughout this post we use data available on the [PGA tour website](https://www.pgatour.com/stats.html). When comparing to the field, we use data starting year 2000. We compile this data and process it using R. If you are curious of how data could be efficiently retrieved, please refer to the nice work done by [Jacob Dubbert](https://github.com/jdubbert/Scrape-golf-data-from-web).

### How does Adam Scott rank compared to peers?

Here are the 10 players who won the most on the PGA tour since 2000:


```
## # A tibble: 10 x 3
##    player_name     wins top_10
##    <chr>          <dbl>  <dbl>
##  1 Tiger Woods       67    156
##  2 Phil Mickelson    31    148
##  3 Vijay Singh       26    135
##  4 Dustin Johnson    24    106
##  5 Rory McIlroy      18     90
##  6 Adam Scott        14     99
##  7 Jim Furyk         13    146
##  8 Justin Thomas     13     58
##  9 Bubba Watson      12     70
## 10 Ernie Els         12     91
```

Adam is 6th on the list with 14 wins (8th in terms of top 10s). Let's look at how it translates in terms of scoring average:


```
## # A tibble: 20 x 3
##    player_name      rounds score_average
##    <chr>             <int>         <dbl>
##  1 Tiger Woods         845          68.6
##  2 Rory McIlroy        572          69.4
##  3 Justin Thomas       547          69.7
##  4 Jim Furyk          1585          69.8
##  5 Jordan Spieth       685          69.8
##  6 Dustin Johnson      985          69.8
##  7 Brooks Koepka       506          69.8
##  8 Vijay Singh        1531          69.9
##  9 Sergio Garcia      1263          69.9
## 10 Phil Mickelson     1650          69.9
## 11 Hideki Matsuyama    610          69.9
## 12 Paul Casey          664          69.9
## 13 Webb Simpson       1055          70.0
## 14 Justin Rose        1156          70.0
## 15 Adam Scott         1131          70.0
## 16 Jason Day           941          70.0
## 17 Tony Finau          605          70.1
## 18 Kenny Perry         941          70.1
## 19 Rickie Fowler       888          70.1
## 20 David Toms         1361          70.1
```

Out of the players with more than 500 rounds since 2000 on the PGA tour, Adam Scott is 15th in scoring average with an average score just below 70. However, scoring average could be slightly biased (well, Tiger is leading it by some margin!), as it is a measure relative to the courses, not your peers.

A better way to compare is to use the strokes gained metrics. The PGA website has this statistic recorded since 2004. Strokes gained (total) is the per round average of the number of strokes a player was better or worse than the field average on the same course and event. Looking at players with at least 200 rounds measured, Adam Scott ranks 16th in strokes gained total.


```
## # A tibble: 20 x 3
##    player_name       measured_rounds sg_total
##    <chr>                       <int>    <dbl>
##  1 Tiger Woods                   393     2.43
##  2 Rory McIlroy                  427     1.61
##  3 Jon Rahm                      246     1.54
##  4 Patrick Cantlay               237     1.45
##  5 Justin Thomas                 434     1.34
##  6 Jim Furyk                     958     1.30
##  7 Dustin Johnson                760     1.24
##  8 Hideki Matsuyama              471     1.23
##  9 Jordan Spieth                 532     1.17
## 10 Steve Stricker                614     1.16
## 11 Xander Schauffele             279     1.16
## 12 Henrik Stenson                299     1.14
## 13 Bryson DeChambeau             291     1.14
## 14 Sergio Garcia                 717     1.12
## 15 Justin Rose                   915     1.11
## 16 Adam Scott                    795     1.11
## 17 Luke Donald                   667     1.11
## 18 Paul Casey                    480     1.09
## 19 Brooks Koepka                 394     1.08
## 20 Phil Mickelson                949     1.07
```

This is in line with the ranking in scoring average and trails the ranking in wins and top 10s. A few players above Adam in strokes gained total are younger and could surpass Adam in wins with time if they maintain their form (e.g. John Rahm, Justin Thomas, Bryson DeChambeau). It could also be a good reflection on Adam's ability to close out tournaments and get the wins. Jim Furyk and Steve Stricker have better strokes gained averages but trail Adam in wins.

### In which part of the game does Adam Scott performs best?

The strokes gained total metric can be broken down into 4 categories: off-the-tee, approach-to-green, around-the-green and putting. The total strokes gained is the sum of the strokes gained for each category. The first two categories relate to the long game, the last two to the short game.

The plot below shows how some of the best players perform in the long and short game: players on the right hand side have an above average long game; players on the top have an above average short game (and there is Tiger Woods!).

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="2400" />
Adam Scott over-performs in the long game and under-performs in the short game. We can break down further and first look at strokes gained long, i.e. strokes gained off-the-tee + strokes approach-to-green:


```
## # A tibble: 362 x 4
##    player_name      sg_long sg_tee sg_approach
##    <chr>              <dbl>  <dbl>       <dbl>
##  1 Tiger Woods         1.63  0.402       1.23 
##  2 Rory McIlroy        1.46  0.978       0.480
##  3 Jon Rahm            1.16  0.816       0.342
##  4 Dustin Johnson      1.16  0.745       0.412
##  5 Hideki Matsuyama    1.14  0.386       0.751
##  6 Kenny Perry         1.13  0.639       0.496
##  7 Adam Scott          1.12  0.485       0.634
##  8 Henrik Stenson      1.08  0.335       0.744
##  9 Corey Conners       1.08  0.581       0.496
## 10 Joe Durant          1.07  0.607       0.464
## # … with 352 more rows
```
Out of the 360 players with more than 200 rounds measured, Adam is 7th in strokes gained approach-to-green, 19th in strokes gained off-the-tee and in total 7th in strokes gained in the long game.

We can look at the same for the short game:

```
## # A tibble: 362 x 4
##    player_name     sg_short sg_around sg_putt
##    <chr>              <dbl>     <dbl>   <dbl>
##  1 Luke Donald        0.919    0.391    0.528
##  2 Steve Stricker     0.865    0.394    0.470
##  3 Aaron Baddeley     0.846    0.319    0.527
##  4 Corey Pavin        0.813    0.385    0.427
##  5 Tiger Woods        0.804    0.242    0.562
##  6 Brandt Snedeker    0.793    0.273    0.520
##  7 Bryce Molder       0.789    0.268    0.522
##  8 Denny McCarthy     0.769    0.0185   0.751
##  9 Jesper Parnevik    0.754    0.120    0.634
## 10 Greg Chalmers      0.731    0.0958   0.635
## # … with 352 more rows
```
Luke Donald is leading this category with Tiger Woods 5th. Adam Scott is only 207th out of the 360 players selected. The below shows that Adam Scott relatively struggles with putting.


```
## # A tibble: 1 x 4
##   player_name sg_short sg_around sg_putt
##   <chr>          <dbl>     <dbl>   <dbl>
## 1 Adam Scott  -0.00473    0.0929 -0.0976
```
### How Adam Scott's strokes gained values have evolved throughout the years?

So far, we have looked at average values for the 2004-2021 period. Let's now look at the trends in Adam's game. The below chart shows the evolution of the yearly average of each strokes gained category:

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="2400" />

Adam seemed to have struggled a lot with putting around 2010. Noticeably in the recent years, his game has been more constantly above average in all aspects. His strong suits have become less dominant (long game) but putting is not hurting him as much. 2021 weakness around the green is likely due to small samples.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="2400" />

If Adam manages to keep up the positive trend in putting and maintain strengths in other areas, more wins and top 10s should come his way!! 
