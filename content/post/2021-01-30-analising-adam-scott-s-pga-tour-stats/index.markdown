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



[Adam Scott](https://www.pgatour.com/players/player.24502.adam-scott.html) is known for his great-looking golf swing and is one of the nicest guy on tour. Since the early 2000s Adam has had tremendous success across worldwide tours. This post will dig further into Adam's PGA tour stats and will try to explain the keys of his success.⛳

<img src="as_masters.jpg" width="100%" />
*Photo by Harry How/Getty Images*  
  

## Explore the data

Throughout this post we use data available on the [PGA tour website](https://www.pgatour.com/stats.html). When comparing to the field, we use data starting year 2000. We compile this data and process it using R. If you are curious of how data could be efficiently retrieved, please refer to the work done by [Jacob Dubbert](https://github.com/jdubbert/Scrape-golf-data-from-web).

### How does Adam Scott rank compared to peers?

Here are the 10 players who the most on the PGA tour since 2000:

```
## # A tibble: 10 x 3
##    player_name     wins top_10
##    <chr>          <dbl>  <dbl>
##  1 Tiger Woods       67    156
##  2 Phil Mickelson    31    148
##  3 Vijay Singh       26    135
##  4 Dustin Johnson    24    106
##  5 Rory McIlroy      18     90
##  6 Adam Scott        14     98
##  7 Jim Furyk         13    146
##  8 Justin Thomas     13     58
##  9 Bubba Watson      12     70
## 10 Ernie Els         12     91
```
Adam is 6th on the list with 14 wins (9th in terms of top 10s). Let's look at how it translates in terms of scoring average:


```
## # A tibble: 16 x 3
##    player_name      rounds score_average
##    <chr>             <int>         <dbl>
##  1 Tiger Woods         845          68.6
##  2 Rory McIlroy        568          69.4
##  3 Justin Thomas       547          69.7
##  4 Jim Furyk          1585          69.8
##  5 Jordan Spieth       683          69.8
##  6 Brooks Koepka       504          69.8
##  7 Dustin Johnson      985          69.8
##  8 Vijay Singh        1531          69.9
##  9 Phil Mickelson     1646          69.9
## 10 Sergio Garcia      1263          69.9
## 11 Hideki Matsuyama    606          69.9
## 12 Paul Casey          664          69.9
## 13 Webb Simpson       1055          70.0
## 14 Justin Rose        1156          70.0
## 15 Jason Day           939          70.0
## 16 Adam Scott         1127          70.0
```

Out of the players with more than 500 rounds since 2000 on the PGA tour, Adam Scott is 16th in scoring average with an average score just below 70. However, scoring average could be slightly biased (well, Tiger is leading it by some margin!), as it is a measure relative to the courses, not your peers. 

A better way to compare is to use the **strokes gained metric**. The PGA website has this statistic recorded since 2004. Strokes gained (total) is the per round average of the number of strokes a player was better or worse than the field average on the same course and event.
Looking at players with at least 200 rounds measured, Adam Scott ranks 17th in strokes gained total.


```
## # A tibble: 17 x 3
##    player_name       measured_rounds sg_total
##    <chr>                       <int>    <dbl>
##  1 Tiger Woods                   393     2.43
##  2 Rory McIlroy                  424     1.60
##  3 Jon Rahm                      243     1.54
##  4 Patrick Cantlay               237     1.45
##  5 Justin Thomas                 434     1.34
##  6 Jim Furyk                     958     1.30
##  7 Hideki Matsuyama              468     1.24
##  8 Dustin Johnson                760     1.24
##  9 Jordan Spieth                 531     1.18
## 10 Steve Stricker                611     1.16
## 11 Henrik Stenson                299     1.14
## 12 Bryson DeChambeau             291     1.14
## 13 Xander Schauffele             276     1.13
## 14 Sergio Garcia                 717     1.12
## 15 Justin Rose                   915     1.11
## 16 Luke Donald                   667     1.11
## 17 Adam Scott                    792     1.11
```
This is in line with the ranking in scoring average and trails the ranking in wins and top 10s. A few players above Adam in sg_total are younger and could surpass Adam in wins with time if they maintain their form (e.g. John Rahm, Justin Thomas, Bryson DeChambeau). It could also be a good reflection on Adam's ability to close out tournaments and get the wins. Jim Furyk and Steve Stricker have better strokes gained averages but trail Adam in wins.

## In which part of the game does Adam Scott performs best?
