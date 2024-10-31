Calculate initial reaction rate
================
Andrew Tolonen
2024

- [Introduction](#introduction)
- [Setup](#setup)
- [File IO](#file-io)
- [Functions](#functions)
- [Methods](#methods)
  - [Method 1](#method-1)
  - [Method 2](#method-2)

# Introduction

This notebook describes how to calculate the initial reaction rate
(vmax) of an enzyme. Initially, the consumption of substrate is linear.
However, as the substrate becomes limiting, the reaction rate decreases.
This script provides two methods to identify the initial points
corresponding to the linear portion of the curve. These points are used
to calculate a linear model for which the slope describes the vmax of
the enzyme.

In method 1, the slopes are calculated for varying number of points
starting from t=0 and the slopes are clustered. All points clustering
with the initial slope are included in the linear regression. In method
2, the slopes of all set of n points are calculated. The set with the
greatest slope is used for a linear regression. If desired, the
clustering approach used in method 1 could be applied to method 2 to
group points.

# Setup

``` r
knitr::opts_chunk$set(warning = F, message = F);
```

``` r
rm(list = ls());

library(tidyverse);
library(readxl);
library(knitr);
library(ggpubr);
library(ggtext);
library(gtable);
library(grid);
library(zoo); # library for analysis ordered indexed observations.

mytheme = theme(axis.text.x = element_text(size = 4), 
                axis.text.y = element_text(size = 4), 
               axis.title.x = element_text(size = 4), 
               axis.title.y = element_text(size = 4),
               strip.text.x = element_text(size = 4),
               legend.position = "none", 
               aspect.ratio =1,
               plot.caption=element_textbox_simple(padding = margin(10,0,10,0), hjust=0, size=10));
```

# File IO

``` r
# load reaction data
datafile = "/home/tolonen/Github/actolonen/Public/Analysis_Lab/Enzymes/Data/reactionRate.xlsx";
input = read_excel(datafile, sheet = "Rate", col_names = TRUE, skip = 0);

head(input, 10)
```

    ## # A tibble: 10 × 2
    ##    Minutes Substrate
    ##      <dbl>     <dbl>
    ##  1     0       0.689
    ##  2     0.1     0.511
    ##  3     0.2     0.42 
    ##  4     0.3     0.353
    ##  5     0.4     0.313
    ##  6     0.5     0.277
    ##  7     0.6     0.26 
    ##  8     0.7     0.227
    ##  9     0.8     0.201
    ## 10     0.9     0.179

# Functions

``` r
getSlope = function(d) # function to calc the slope of a linear regression
{
  d = as.data.frame(d);
  m = lm(Substrate ~ Minutes, d);
  myslope = m$coefficient[2];
  return(myslope);
}
```

# Methods

## Method 1

Calculate linear model from t=0 using increasing numbers of points. This
method assumes the curve is composed of two parts: the linear part and
the non-linear part. The points are divided into the linear vs
non-linear groups by K-means and the cluster containing the greater
slope is selected.

1.  Calc the slopes of linear models of increasing numbers of data
    points
2.  Cluster the slopes into two clusters by K-means
3.  Select datapoints in cluster with high initial reaction rate.

``` r
mydata = input;
slopes.all = numeric(0); # delcare empty vector

# Method 1: calculate the slope of the linear model for increasing numbers of points
for (x in 1:nrow(mydata)) 
{
  test.data = head(mydata, x);
  my.slope = getSlope(test.data);
  slopes.all[x] = my.slope;
} 
 
slopes.all[1] = 0; # change NA to zero

# # add col of slopes to data.
mydata = mydata %>%
  mutate(Slopes = slopes.all);

# cluster the slopes of the linear regression (cluster 1 = good and cluster 2 = bad)
co.cl = kmeans(mydata$Slopes, 2); 

# b.points are points included in linear part of curve
b.points = which(co.cl$cluster == match(min(co.cl$centers), co.cl$centers));

# RES is positions of points in data with indices in b.points
RES = mydata[b.points,];

# calc lin regression for points in linear portion of curve
test = lm(Substrate ~ Minutes, RES);

# calc regression line
yintercept = test$coefficients[1];
myslope = test$coefficients[2];
Substrate_calc = myslope * mydata$Minutes + yintercept;
RES = cbind(mydata, Substrate_calc);

# calc max vals for axes
my.ylim = max(mydata$Substrate);
my.xlim = max(mydata$Minutes);

# regression line
regline = paste("Product = ", round(myslope, 2), "* minutes + ", round(yintercept, 2), sep = "");

# plot data (red points) and regression line (black line)
myplot1 = ggplot(mydata, aes(x=Minutes, y=Substrate)) +
  ggtitle("Method 1: data and linear regression line")+
  geom_point(size=1, color = "red") +
  xlab("Minutes") + 
  ylab("Substrate") +
  geom_line(aes(x=Minutes, y=Substrate_calc))+
  coord_cartesian(xlim=c(0,my.xlim), ylim = c(0, my.ylim))+
 scale_x_continuous(breaks=seq(0, my.xlim, my.xlim/10))+
 scale_y_continuous(breaks=seq(0, my.ylim, my.ylim/10))+
   geom_text(x=6, y=0.1, label=regline) +
  theme_classic();

myplot1
```

<figure>
<img
src="initialReactionRate_methods_files/figure-gfm/calc%20linear%20model%201-1.png"
alt="Fig: Enzyme catalyzed accumulation of product. Data points show product concentration and the black line shows the rate of product formation for the initial, linear portion of the curve. Points were selected starting from t=0 and calculating the slope for increasing numbers of points. The slopes were clustered by K-means and all points that cluster with the intial slopes are included for calculating the linear regression." />
<figcaption aria-hidden="true">Fig: Enzyme catalyzed accumulation of
product. Data points show product concentration and the black line shows
the rate of product formation for the initial, linear portion of the
curve. Points were selected starting from t=0 and calculating the slope
for increasing numbers of points. The slopes were clustered by K-means
and all points that cluster with the intial slopes are included for
calculating the linear regression.</figcaption>
</figure>

## Method 2

Calculate linear model using sliding window. Here, instead of assuming
the data is divided into two parts (linear and non-linear), we identify
the set of n contiguous points with the greatest slope. These points are
used to define the slope (max reaction rate).

``` r
# define size of sliding window.
window.width = 4; 

# Goal: select the number of initial data points composing the initial, linear portion of the curve. 
mydata = input;
slopes.all = numeric(0); # delcare empty vector

# Method 2: calculate slope across rolling window of data points  
co = rollapply(mydata, width = window.width, getSlope, by.column=F); # apply a function across a rolling window. 

# add zeros corresponding to final datapoints for which we can't calculate a slope
final.zeros = rep(0, window.width-1);
co = c(co, final.zeros); # add 2 initial zeros

# add col of slopes and row numbers to data.
mydata = mydata %>%
  mutate(Slopes = co) %>%
  mutate(rownum = 1:nrow(mydata));

# find datapoints with greatest slope
min.slope = min(mydata$Slopes);
getrow = mydata %>%
  filter(Slopes == min.slope);
firstrow = getrow$rownum;
RES = slice(mydata, firstrow:(firstrow+(window.width-1)));

# calc lin regression for points in linear portion of curve
test = lm(Substrate ~ Minutes, RES);

# calc regression line
yintercept = test$coefficients[1];
myslope = test$coefficients[2];
Substrate_calc = myslope * test.data$Minutes + yintercept;
RES = cbind(mydata, Substrate_calc);

# calc max vals for axes
my.ylim = max(mydata$Substrate);
my.xlim = max(mydata$Minutes);

# regression line
regline = paste("Product = ", round(myslope, 2), "* minutes + ", round(yintercept, 2), sep = "");

# plot data (red points) and regression line (black line)
myplot2 = ggplot(mydata, aes(x=Minutes, y=Substrate)) +
  ggtitle("Method 2: data and linear regression line")+
  geom_point(size=1, color = "red") +
  xlab("Minutes") + 
  ylab("Substrate") +
  geom_line(aes(x=Minutes, y=Substrate_calc))+
  coord_cartesian(xlim=c(0,my.xlim), ylim = c(0, my.ylim))+
 scale_x_continuous(breaks=seq(0, my.xlim, my.xlim/10))+
 scale_y_continuous(breaks=seq(0, my.ylim, my.ylim/10))+
 geom_text(x=6, y=0.1, label=regline) +
  theme_classic();

myplot2
```

<figure>
<img
src="initialReactionRate_methods_files/figure-gfm/calc%20linear%20model%202-1.png"
alt="Fig: Enzyme catalyzed accumulation of product. Data points show product concentration and the black line shows the rate of product formation for the initial, linear portion of the curve. Points were selected based on sliding window on n points. The sliding window with the greatest slope was used to calculate the linear regression." />
<figcaption aria-hidden="true">Fig: Enzyme catalyzed accumulation of
product. Data points show product concentration and the black line shows
the rate of product formation for the initial, linear portion of the
curve. Points were selected based on sliding window on n points. The
sliding window with the greatest slope was used to calculate the linear
regression.</figcaption>
</figure>
