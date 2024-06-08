Calculate enzyme vmax based on initial reaction rate
================
Andrew Tolonen
2024

# Introduction

This notebook describes how to calculate the initial reaction rate
(vmax) of an enzyme. Initially, the accumulation of product is linear.
However, as the substrate becomes limiting, the reaction rate decreases.
This script provides a method to identify the initial points
corresponding to the linear portion of the curve. These points are used
to calculate vmax of the enzyme.

## Setup

Knitr options

``` r
knitr::opts_chunk$set(warning = F, message = F);
```

File setup

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

Function to calculate slope of linear model

``` r
getSlope = function(d) # function to calc the slope of a linear regression
{
  d = as.data.frame(d);
  m = lm(Product ~ Minutes, d);
  myslope = m$coefficient[2];
  return(myslope);
}
```

Load data

``` r
# load reaction data
datafile = "/home/tolonen/Github/actolonen/Public/Analysis_Lab/Enzyme_Kinetics/reactionRate.xlsx";
input = read_excel(datafile, sheet = "Rate", col_names = TRUE, skip = 0);
```

Method 1: calculate linear model from t=0 using increasing numbers of
points

``` r
# Goal: select the number of initial data points composing the initial, linear portion of the curve. This method assumes the curve is composed of two parts: the linear part and the non-linear part. 

# 1. Calc the slopes of linear models of increasing numbers of data points
# 2. Cluster the slopes into two clusters by K-means
# 3. Select datapoints in cluster with high initial reaction rate.

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
b.points = which(co.cl$cluster == match(max(co.cl$centers), co.cl$centers));

# RES is positions of points in data with indices in b.points
RES = mydata[b.points,];

# calc lin regression for points in linear portion of curve
test = lm(Product ~ Minutes, RES);

# calc regression line
yintercept = test$coefficients[1];
myslope = test$coefficients[2];
Product_calc = myslope * mydata$Minutes + yintercept;
RES = cbind(mydata, Product_calc);

# calc max vals for axes
my.ylim = max(mydata$Product);
my.xlim = max(mydata$Minutes);

# regression line
regline = paste("Product = ", round(myslope, 2), "* minutes + ", yintercept, sep = "");

# plot data (red points) and regression line (black line)
myplot1 = ggplot(mydata, aes(x=Minutes, y=Product)) +
  ggtitle("Method 1: data and linear regression line")+
  geom_point(size=1, color = "red") +
  xlab("Minutes") + 
  ylab("Product") +
  geom_line(aes(x=Minutes, y=Product_calc))+
  coord_cartesian(xlim=c(0,my.xlim), ylim = c(0, my.ylim))+
 scale_x_continuous(breaks=seq(0, my.xlim, my.xlim/10))+
 scale_y_continuous(breaks=seq(0, my.ylim, my.ylim/10))+
   geom_text(x=6, y=0.1, label=regline) +
  theme_classic();

myplot1
```

<figure>
<img
src="reactionRate_method_files/figure-gfm/calc%20linear%20model%201-1.png"
alt="Fig: Enzyme catalyzed accumulation of product. Data points show product concentration and the black line shows the rate of product formation for the initial, linear portion of the curve. Points were selected starting from t=0 and selecting increasing numbers of points." />
<figcaption aria-hidden="true">Fig: Enzyme catalyzed accumulation of
product. Data points show product concentration and the black line shows
the rate of product formation for the initial, linear portion of the
curve. Points were selected starting from t=0 and selecting increasing
numbers of points.</figcaption>
</figure>

Method 2: calculate linear model using sliding window

``` r
# Goal: select the number of initial data points composing the initial, linear portion of the curve. This method assumes the curve is composed of two parts: the linear part and the non-linear part. 

# 1. Calc the slopes of linear models of increasing numbers of data points
# 2. Cluster the slopes into two clusters by K-means
# 3. Select datapoints in cluster with high initial reaction rate.

mydata = input;
slopes.all = numeric(0); # delcare empty vector

# Method 2: calculate slope across rolling window  data points  
co = rollapply(mydata, width = 3, getSlope, by.column=F); # apply a function across a rolling window. 

co = c(0, 0, co); # add 2 initial zeros

# # add col of slopes to data.
mydata = mydata %>%
  mutate(Slopes = co);

# cluster the slopes of the linear regression (cluster 1 = good and cluster 2 = bad)
co.cl = kmeans(mydata$Slopes, 2); 

# b.points are points included in linear part of curve
b.points = which(co.cl$cluster == match(max(co.cl$centers), co.cl$centers));

# RES is positions of points in data with indices in b.points
RES = mydata[b.points,];

# calc lin regression for points in linear portion of curve
test = lm(Product ~ Minutes, RES);

# calc regression line
yintercept = test$coefficients[1];
myslope = test$coefficients[2];
Product_calc = myslope * test.data$Minutes + yintercept;
RES = cbind(mydata, Product_calc);

# calc max vals for axes
my.ylim = max(mydata$Product);
my.xlim = max(mydata$Minutes);

# regression line
regline = paste("Product = ", round(myslope, 2), "* minutes + ", yintercept, sep = "");

# plot data (red points) and regression line (black line)
myplot2 = ggplot(mydata, aes(x=Minutes, y=Product)) +
  ggtitle("Method 2: data and linear regression line")+
  geom_point(size=1, color = "red") +
  xlab("Minutes") + 
  ylab("Product") +
  geom_line(aes(x=Minutes, y=Product_calc))+
  coord_cartesian(xlim=c(0,my.xlim), ylim = c(0, my.ylim))+
 scale_x_continuous(breaks=seq(0, my.xlim, my.xlim/10))+
 scale_y_continuous(breaks=seq(0, my.ylim, my.ylim/10))+
 geom_text(x=6, y=0.1, label=regline) +
  theme_classic();

myplot2
```

<figure>
<img
src="reactionRate_method_files/figure-gfm/calc%20linear%20model%202-1.png"
alt="Fig: Enzyme catalyzed accumulation of product. Data points show product concentration and the black line shows the rate of product formation for the initial, linear portion of the curve. Points were selected based on sliding window." />
<figcaption aria-hidden="true">Fig: Enzyme catalyzed accumulation of
product. Data points show product concentration and the black line shows
the rate of product formation for the initial, linear portion of the
curve. Points were selected based on sliding window.</figcaption>
</figure>
