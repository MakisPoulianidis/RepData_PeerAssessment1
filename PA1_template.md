# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

##### 1. Load the data (i.e. read.csv())


```r
activity <- read.csv("activity.csv")
```

##### 2. Process/transform the data (if necessary) into a format suitable for your analysis 

First, call libraries dplyr & lubridate

```r
library("dplyr")
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library("lubridate")
```
*At this point, no further processing/transforming is needed.*


## What is mean total number of steps taken per day?

##### 1. Make a histogram of the total number of steps taken each day

Summarise the data per day and set readable column names

```r
sum.of.steps<-summarise(group_by(activity,date),sum(steps, rm.na=TRUE))
names(sum.of.steps)<-c("Date","Sum.Of.Steps")
```
Create the histogram

```r
hist(sum.of.steps$Sum.Of.Steps, col="blue",xlim=c(0,25000))
```

![](./PA1_template_files/figure-html/unnamed-chunk-4-1.png) 


##### 2. Calculate and report the mean and median total number of steps taken per day

Calculating the mean number of total steps per day...

```r
mean(sum.of.steps$Sum.Of.Steps, na.rm=TRUE)
```

```
## [1] 10767.19
```
....and the median number of total steps per day.

```r
median(sum.of.steps$Sum.Of.Steps, na.rm=TRUE)
```

```
## [1] 10766
```

## What is the average daily activity pattern?
##### 1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

First calculate the total number of steps per 5-minute interval divided by the distinct number of days and set readable column names.

```r
sum.of.steps.int<-summarise(group_by(activity,interval),sum(steps, na.rm=TRUE)/n_distinct(date))
names(sum.of.steps.int)<-c("interval","Sum.Of.Steps")
```

Next create the time-series plot.

```r
plot(type="l",sum.of.steps.int$interval,sum.of.steps.int$Sum.Of.Steps)       
```

![](./PA1_template_files/figure-html/unnamed-chunk-8-1.png) 


##### 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
filter(sum.of.steps.int,Sum.Of.Steps==max(Sum.Of.Steps))
```

```
## Source: local data frame [1 x 2]
## 
##   interval Sum.Of.Steps
## 1      835     179.1311
```

## Imputing missing values

##### 1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

##### 2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

##### 3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

##### 4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?



## Are there differences in activity patterns between weekdays and weekends?
##### 1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

##### 2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). 
