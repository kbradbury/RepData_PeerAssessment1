# PA_Assignment1



This is my R markdown file for Peer Assessment 1.

Here we are going to load some data and clean it where appropriate.



```r
FilePath <- "F:/Reproducible Research/Peer Assessment 1/RepData_PeerAssessment1/repdata-data-activity/"
data <- read.csv(paste(FilePath,"activity.csv",sep=""))
```

What is the mean total number of steps taken per day? You can ignore missing values for this part.
Here we will calculate the total number of steps taken per day, plot the total number of steps on a histogram, and calculate and report the mean and median of the total number of steps taken per day.


```r
library("ggplot2")
```

```
## Warning: package 'ggplot2' was built under R version 3.0.3
```

```r
steps.byday <- aggregate(steps~date,data=data,sum)

total_steps <- sum(steps.byday$steps,na.rm=TRUE)
print(paste("The total number of steps taken per day is",total_steps))
```

```
## [1] "The total number of steps taken per day is 570608"
```

Plot the total number of steps in a histogram.


```
## png 
##   2
```

Calculate the mean number of steps.


```r
mean_steps <- mean(steps.byday$steps,na.rm=TRUE)
print(paste("The mean number of steps taken per day is",mean_steps))
```

```
## [1] "The mean number of steps taken per day is 10766.1886792453"
```

```r
median_steps <- median(steps.byday$steps,na.rm=TRUE)
print(paste("The median number of steps taken per day is",median_steps))
```

```
## [1] "The median number of steps taken per day is 10765"
```

What is the average daily activity pattern?
Here we will make a time series plot of the 5-minute intervale and the average number of steps taken, averaged across all days. We will also report which 5-minute interval contains the maximum number of steps.



```
## png 
##   2
```

```
## [1] "The 5-minute interval that contains the maximum number of steps is 835"
```

Inputing missing values.
Here we will calculate and report the total number of missing values in the data set. Missing values will be filled in with the average number of steps taken on that given day. If all values for that day are missing, values will be filled with 0. And we will make a histogram of the total number of steps taken each day and calculate and report the mean and median total number of steps taken each day.



```
## [1] "The total number of steps taken per day is 570608"
```

```
## png 
##   2
```

```
## [1] "The mean number of steps taken per day is 9354.22950819672"
```

```
## [1] "The median number of steps taken per day is 10395"
```

Are there differences in activity patterns between weekdays and weekends?
Here we will create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or a weekend day. We will also make a panel plot containing a time series plot of the 5-minute interval and the average number of steps taken, averaged across all weekday days oor weekend days.



```
## png 
##   2
```
