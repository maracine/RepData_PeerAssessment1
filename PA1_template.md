---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


```r
library("ggplot2")
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
setwd("/Users/Michel/Dropbox/Coursera/Data Science - John Hopkin's/Reproducible Research/week2/Course Project 1")
```

## Loading and preprocessing the data
### We want to ignore NA values

```r
activities <- read.csv("activity.csv")
filtered_activities <- na.omit(activities)
```

## What is mean total number of steps taken per day?

```r
total_steps_per_day <- aggregate(steps ~ date, activities, sum)


mean_steps <- mean(total_steps_per_day$steps)
median_steps <- median(total_steps_per_day$steps)

qplot(total_steps_per_day$steps, binwidth=1000, xlab="total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

The mean of the of the total number of steps is 10766.  
The median of the of the total number of steps is 10765. 


## What is the average daily activity pattern?

```r
average_steps_per_interval <- aggregate(steps ~ interval, activities, mean)


plot(average_steps_per_interval$interval, average_steps_per_interval$steps, type="l", xlab="Interval", 
     ylab="Average number of steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
Max_steps <- max(average_steps_per_interval$steps)
Max_interval <- average_steps_per_interval[average_steps_per_interval$steps == Max_steps,]$interval
```
The interval  835 on average contains the maximum number of steps, which is  206.

## Replace missing values by using the mean for the 5-minute interval
### We will re-use the average_steps_per_interval data frame...

```r
missing_values = sum(!complete.cases(activities))

activities2 <- transform(activities, steps = ifelse(is.na(activities$steps), average_steps_per_interval$steps[match(activities$interval, average_steps_per_interval$interval)], activities$steps))
```

## Are there differences in activity patterns between weekdays and weekends?
