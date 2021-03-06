<<<<<<< HEAD
# PA1_template

## Loading and preprocessing the data


```r
activity_data <- read.csv("C:/Users/paulomauricio/Documents/Jhon's Hopkins/Reproducible research/activity.csv")
str(activity_data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```
Set date format and load required libraries


```r
library(lubridate)
library(dplyr)
library(ggplot2)
```


```r
activity_data$date  <- ymd(activity_data$date)
activity_data$steps <- as.numeric(activity_data$steps)
```

## What is mean total number of steps taken per day?

### Calculate the total number of steps taken per day


```r
day_mean <- activity_data %>% group_by(date) %>% summarise(day_mean = mean(steps,na.rm = TRUE))
```

### If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day


```r
day_total <- activity_data %>% group_by(date) %>% summarise(day_total = sum(steps,na.rm = TRUE))
hist(day_total$day_total,main="Total steps per day",xlab="")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

### Calculate and report the mean and median of the total number of steps taken per day


```r
mean(day_total$day_total,na.rm = TRUE)
```

```
## [1] 9354.23
```

```r
median(day_total$day_total,na.rm = TRUE)
```

```
## [1] 10395
```

## What is the average daily activity pattern?

### Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
five_min_average <- activity_data %>% group_by(interval) %>% summarise(average = mean(steps,na.rm = TRUE))
plot(ts(five_min_average$average),xlab="",ylab="",main="Time series 5-Minute Interval")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
five_min_average[which.max(five_min_average$average),]
```

```
## # A tibble: 1 x 2
##   interval  average
##      <int>    <dbl>
## 1      835 206.1698
```

## Imputing missing values

### Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
summary(activity_data$steps)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    0.00    0.00    0.00   37.38   12.00  806.00    2304
```

### Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

The strategy was to complete the missing values with the mean for the 5-minute interval.  


```r
new.activity <- inner_join(activity_data,five_min_average,by="interval")
new.activity$steps_fill <- ifelse(is.na(new.activity$steps)==TRUE,new.activity$average,new.activity$steps)
```

### Create a new dataset that is equal to the original dataset but with the missing data filled in


```r
activity_data <- inner_join(activity_data,five_min_average,by="interval")
activity_data$steps <- ifelse(is.na(activity_data$steps)==TRUE,activity_data$average,activity_data$steps)
```

### Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
day_total_2 <- activity_data %>% group_by(date) %>% summarise(day_total = sum(steps,na.rm = TRUE))
hist(day_total_2$day_total,main="Total steps per day",xlab="")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

Histogram of new dataset appears to be normally distributed

## Are there differences in activity patterns between weekdays and weekends?

### Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


```r
activity_data$week <- ifelse(weekdays(activity_data$date)=="domingo" | weekdays(activity_data$date)=="s?bado","weekend","weekday")
```

### Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
five_min_average_2 <- activity_data %>% group_by(interval,week) %>% summarise(average = mean(steps,na.rm = TRUE))
qplot(interval,average,data = five_min_average_2,facets = .~week,geom = "path",
      xlab = "",ylab = "")
```

![](PA1_template_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
=======
# PA1_template

## Loading and preprocessing the data


```r
activity_data <- read.csv("C:/Users/paulomauricio/Documents/Jhon's Hopkins/Reproducible research/activity.csv")
str(activity_data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```
Set date format and load required libraries


```r
library(lubridate)
library(dplyr)
library(ggplot2)
```


```r
activity_data$date  <- ymd(activity_data$date)
activity_data$steps <- as.numeric(activity_data$steps)
```

## What is mean total number of steps taken per day?

### Calculate the total number of steps taken per day


```r
day_mean <- activity_data %>% group_by(date) %>% summarise(day_mean = mean(steps,na.rm = TRUE))
```

### If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day


```r
day_total <- activity_data %>% group_by(date) %>% summarise(day_total = sum(steps,na.rm = TRUE))
hist(day_total$day_total,main="Total steps per day",xlab="")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

### Calculate and report the mean and median of the total number of steps taken per day


```r
mean(day_total$day_total,na.rm = TRUE)
```

```
## [1] 9354.23
```

```r
median(day_total$day_total,na.rm = TRUE)
```

```
## [1] 10395
```

## What is the average daily activity pattern?

### Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
five_min_average <- activity_data %>% group_by(interval) %>% summarise(average = mean(steps,na.rm = TRUE))
plot(ts(five_min_average$average),xlab="",ylab="",main="Time series 5-Minute Interval")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
five_min_average[which.max(five_min_average$average),]
```

```
## # A tibble: 1 x 2
##   interval  average
##      <int>    <dbl>
## 1      835 206.1698
```

## Imputing missing values

### Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
summary(activity_data$steps)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    0.00    0.00    0.00   37.38   12.00  806.00    2304
```

### Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

The strategy was to complete the missing values with the mean for the 5-minute interval.  


```r
new.activity <- inner_join(activity_data,five_min_average,by="interval")
new.activity$steps_fill <- ifelse(is.na(new.activity$steps)==TRUE,new.activity$average,new.activity$steps)
```

### Create a new dataset that is equal to the original dataset but with the missing data filled in


```r
activity_data <- inner_join(activity_data,five_min_average,by="interval")
activity_data$steps <- ifelse(is.na(activity_data$steps)==TRUE,activity_data$average,activity_data$steps)
```

### Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
day_total_2 <- activity_data %>% group_by(date) %>% summarise(day_total = sum(steps,na.rm = TRUE))
hist(day_total_2$day_total,main="Total steps per day",xlab="")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

Histogram of new dataset appears to be normally distributed

## Are there differences in activity patterns between weekdays and weekends?

### Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


```r
activity_data$week <- ifelse(weekdays(activity_data$date)=="domingo" | weekdays(activity_data$date)=="s?bado","weekend","weekday")
```

### Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
five_min_average_2 <- activity_data %>% group_by(interval,week) %>% summarise(average = mean(steps,na.rm = TRUE))
qplot(interval,average,data = five_min_average_2,facets = .~week,geom = "path",
      xlab = "",ylab = "")
```

![](PA1_template_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
>>>>>>> d66443387601d8699f8c6731da89378b3aeb7c5f
