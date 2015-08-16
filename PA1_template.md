PA1_template.md
===============

This is my R markdown file for Peer Assessment 1.

Here we are going to load some data and clean it where appropriate.


'''{r}
install.packages("curl")
library("curl")

temp <- tempfile()
download.file("https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip",temp)
data <- read.csv(unz(temp, "activity.csv"))
unlink(temp)

'''

What is the mean total number of steps taken per day? You can ignore missing values for this part.
Here we will calculate the total number of steps taken per day, plot the total number of steps on a histogram, and calculate and report the mean and median of the total number of steps taken per day.

'''{r}
install.packages("ggplot2")
library("ggplot2")


steps.byday <- aggregate(steps~date,data=data,sum)

total_steps <- sum(steps.byday$steps,na.rm=TRUE)
print(paste("The total number of steps taken per day is",total_steps))

ggplot(steps.byday,aes(x=steps)) + geom_histogram(binwidth=1000,colour="black",fill="white")

mean_steps <- mean(steps.byday$steps,na.rm=TRUE)
print(paste("The mean number of steps taken per day is",mean_steps))
median_steps <- median(steps.byday$steps,na.rm=TRUE)
print(paste("The median number of steps taken per day is",median_steps))
'''

What is the average daily activity pattern?
Here we will make a time series plot of the 5-minute intervale and the average number of steps taken, averaged across all days. We will also report which 5-minute interval contains the maximum number of steps.


'''{r}
average.steps <- aggregate(steps~interval,data=data,mean)
ggplot(data=average.steps, aes(x=interval, y=steps, group=1)) + geom_line(colour="red",size=1)

max.steps <- max(average.steps$steps,na.rm=TRUE)
max.interval <- average.steps[which(average.steps$steps==max.steps),1]
print(paste("The 5-minute interval that contains the maximum number of steps is",max.interval))
'''

Inputing missing values.
Here we will calculate and report the total number of missing values in the data set. Missing values will be filled in with the average number of steps taken on that given day. If all values for that day are missing, values will be filled with 0. And we will make a histogram of the total number of steps taken each day and calculate and report the mean and median total number of steps taken each day.


'''{r}
install.packages("ggplot2")
library("ggplot2")

total.missing <- sum(is.na(data$steps))


date <- unique(data$date)
data_temp <- NULL
data2 <- NULL


  for (d in date){
    data.date <- data[which(data$date==d),]
    row.count <- nrow(data.date) 
    
    for (i in 1:row.count){
  
      if(is.na(data.date[i,1])==TRUE){
          data.date[i,1] <- mean(data.date$steps,na.rm=TRUE)
          if(is.nan(data.date[i,1])){
          data.date[i,1] <- 0
      }
    
    }
  }  
  data_temp <- data.date
  data2 <- rbind(data2, data_temp)
}

steps.byday2 <- aggregate(steps~date,data=data2,sum)

total_steps2 <- sum(steps.byday2$steps,na.rm=TRUE)
print(paste("The total number of steps taken per day is",total_steps2))

ggplot(steps.byday2,aes(x=steps)) + geom_histogram(binwidth=1000,colour="black",fill="white")

mean_steps2 <- mean(steps.byday2$steps,na.rm=TRUE)
print(paste("The mean number of steps taken per day is",mean_steps2))
median_steps2 <- median(steps.byday2$steps,na.rm=TRUE)
print(paste("The median number of steps taken per day is",median_steps2))
'''

Are there differences in activity patterns between weekdays and weekends?
Here we will create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or a weekend day. We will also make a panel plot containing a time series plot of the 5-minute interval and the average number of steps taken, averaged across all weekday days oor weekend days.


'''{r}
day.data <- NULL
data2$day <- weekdays(as.Date(data2$date))
day <- unique(data2$day)

for(d in day){
  day_sub <- data2[which(data2$day==d),]
    if(d=="Saturday"){
      day_sub$daylevel <- "weekend"
    } else if(d=="Sunday") {
      day_sub$daylevel <- "weekend"
    } else {
      day_sub$daylevel <- "weekday"
    }
  day.data <- rbind(day.data,day_sub)
}

day.steps <- aggregate(steps~interval+daylevel,data=day.data,sum)
weekend <- day.steps[which(day.steps$daylevel=="weekend"),]
weekday <- day.steps[which(day.steps$daylevel=="weekday"),]

par(mfrow=c(2,1))
plot(weekend$interval,weekend$steps,type="l",xlab="Interval",ylab="Number of Steps",main="weekend")
plot(weekday$interval,weekday$steps,type="l",xlab="Interval",ylab="Number of steps",main="weekday")
'''
