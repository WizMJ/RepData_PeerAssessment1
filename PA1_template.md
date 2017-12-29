---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---
Code for reading in the dataset and/or processing the data
Histogram of the total number of steps taken each day
Mean and median number of steps taken each day
Time series plot of the average number of steps taken
The 5-minute interval that, on average, contains the maximum number of steps
Code to describe and show a strategy for imputing missing data
Histogram of the total number of steps taken each day after missing values are imputed
Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
All of the R code needed to reproduce the results (numbers, plots, etc.) in the report

## Loading and preprocessing the data

```r
setwd("D:/01 Minki Philosophy/10 Data Science/08 github/coursera/5. reproducible research/RepData_PeerAssessment1")
library(ggplot2)
unzip(zipfile = "activity.zip")
data <- read.csv("activity.csv")
dim(data)
```

```
## [1] 17568     3
```

```r
colnames(data)
```

```
## [1] "steps"    "date"     "interval"
```
Data consists of 17,568 observations and 3 variables.

## What is mean total number of steps taken per day?

Let's answer the questions below:
+ Calculate the total number of steps taken per day
+ Make a histogram of the total number of steps taken each day
+ Calculate and report the mean and median of the total number of steps taken per day


```r
steps.daysum <- sapply(split(data$steps,data$date), sum, na.rm = TRUE)
steps.daysum <- data.frame(date=names(steps.daysum), steps=steps.daysum, row.names =NULL)
ggplot(steps.daysum, aes(x = steps)) + geom_histogram(binwidth = 500) 
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


```r
median(steps.daysum$steps)
```

```
## [1] 10395
```

```r
mean(steps.daysum$steps)
```

```
## [1] 9354.23
```

## What is the average daily activity pattern?

Let's make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
steps.intavg <- sapply(split(data$steps, data$interval), mean, na.rm=TRUE)
steps.intavg <- data.frame(interval=as.numeric(names(steps.intavg)), steps=steps.intavg, row.names =NULL)
ggplot(data = steps.intavg, aes(x = interval, y = steps)) + geom_line() + xlab("5-minute interval") + ylab("average number of steps taken")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

Now, let's find the 5-minute interval that has the maximum number of steps.


```r
max.value <- max(steps.intavg$steps)
max.value
```

```
## [1] 206.1698
```

```r
steps.intavg[which.max(steps.intavg$steps),1]
```

```
## [1] 835
```
the `835th` interval has the maximum value of steps, `206.17`

## Imputing missing values

Let's see how many missing values are in the dataset.

```r
sum(is.na(data$steps))
```

```
## [1] 2304
```

```r
sum(is.na(data$date))
```

```
## [1] 0
```

```r
sum(is.na(data$interval))
```

```
## [1] 0
```

Only the data in steps column is missing, which is `2,304`


```r
a <- sapply(split(data$steps,data$date), median, na.rm = TRUE)
```

2. filling in all of the missing values
3. histogram


## Are there differences in activity patterns between weekdays and weekends?
