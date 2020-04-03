\# Reproducible Research: Peer Assessment 1
-------------------------------------------

title: ‘Reproducible Research: Peer Assessment 1’ author: “Mahendra S
Payaal” date: “4/2/2020”

------------------------------------------------------------------------

Setting Global Options
----------------------

    knitr::opts_chunk$set(fig.path = "figure/")
    library(markdown)

    ## Warning: package 'markdown' was built under R version 3.6.3

Loading and preprocessing the data
----------------------------------

    activity <- read.csv("C:/Users/mahendra.payaal/Documents/coursera/data/activity.csv",stringsAsFactors = FALSE)
    activity  <- transform( activity, date = as.Date(date,"%Y-%m-%d"))

What is mean total number of steps taken per day?
-------------------------------------------------

### Total number of steps taken everyday

    steps_daily_Total <- as.data.frame(with(activity,tapply(steps,date,sum,na.rm=TRUE)))
    rownames(steps_daily_Total) <-c(1:61)
    colnames(steps_daily_Total) <- "Daily_Steps_Sum"
    steps_daily_Total

    ##    Daily_Steps_Sum
    ## 1                0
    ## 2              126
    ## 3            11352
    ## 4            12116
    ## 5            13294
    ## 6            15420
    ## 7            11015
    ## 8                0
    ## 9            12811
    ## 10            9900
    ## 11           10304
    ## 12           17382
    ## 13           12426
    ## 14           15098
    ## 15           10139
    ## 16           15084
    ## 17           13452
    ## 18           10056
    ## 19           11829
    ## 20           10395
    ## 21            8821
    ## 22           13460
    ## 23            8918
    ## 24            8355
    ## 25            2492
    ## 26            6778
    ## 27           10119
    ## 28           11458
    ## 29            5018
    ## 30            9819
    ## 31           15414
    ## 32               0
    ## 33           10600
    ## 34           10571
    ## 35               0
    ## 36           10439
    ## 37            8334
    ## 38           12883
    ## 39            3219
    ## 40               0
    ## 41               0
    ## 42           12608
    ## 43           10765
    ## 44            7336
    ## 45               0
    ## 46              41
    ## 47            5441
    ## 48           14339
    ## 49           15110
    ## 50            8841
    ## 51            4472
    ## 52           12787
    ## 53           20427
    ## 54           21194
    ## 55           14478
    ## 56           11834
    ## 57           11162
    ## 58           13646
    ## 59           10183
    ## 60            7047
    ## 61               0

### Histogram of the total number of steps taken each day

    hist(steps_daily_Total$Daily_Steps_Sum,main ="Histogram of Total Daily Steps over Two Months", xlab = "Daily Steps")
    abline(v=mean(steps_daily_Total$Daily_Steps_Sum,na.rm = TRUE),lwd =2,col="blue")
    text(8000,15,"Mean",col="blue")

    abline(v=median(steps_daily_Total$Daily_Steps_Sum,na.rm = TRUE),lwd =2,col="magenta")
    text(12000,25,"Median",col="magenta")

![](figure/histogram_of_total_daily_steps-1.png)

### Mean total number of steps taken per day

    mean_daily <- mean(steps_daily_Total$Daily_Steps_Sum,na.rm = TRUE)

      The mean of total daily steps is 9354.2295082

### Median of total daily steps

    median_daily <- median(steps_daily_Total$Daily_Steps_Sum)

       The median of total daily steps is 10395

What is the average daily activity pattern?
-------------------------------------------

### Time series plot of the average number of steps taken

    steps_interval_Average <- as.data.frame(with(activity,tapply(steps,interval,mean,na.rm=TRUE)))
    names(steps_interval_Average) <- "Average_steps"
    plot(as.numeric(rownames(steps_interval_Average)),steps_interval_Average$Average_steps,type ="l",xlab="24 Hour Interval of 5 Min each",ylab="Number of Steps")
    title("Time Plot of Average Steps in Every 5 Minutes Interval")

![](figure/plot_average_steps_by_interval-1.png)

### The 5-minute interval that, on average, contains the maximum number of steps

    max_interval <- which.max(steps_interval_Average[,1])
    names_interval <- rownames(steps_interval_Average)
    max_interval<-names_interval[104]
    max_int <- sub("(\\d+)(\\d{2})","\\1:\\2",max_interval)

      The average maximun  steps are in 5 minute interval from 8:35

Imputing missing values
-----------------------

### Total number of missing values

    total_NA <- sum(is.na(activity$steps))

       The total NAs in data frame  are 2304

### Code to describe and show a strategy for imputing missing data and creating a new dataset

-   ‘NA’ values are replaced by the average values for that time
    interval. The steps are given below
-   A vector“avg\_steps” is created which has average value of all
    intervals
-   This vector is replicated 61 times to become equal to rows of
    “activity” dataframe
-   A new data set “activity\_new” is created
-   Through a ‘For’ loop ‘NA’ values are replaced by average values of
    that interval

<!-- -->

    avg_steps <- steps_interval_Average[,1]
    avg_steps <- c(rep(avg_steps,times =61))
    activity_new <- activity
    for (i in 1:nrow(activity_new)) {
          if(is.na(activity_new$steps[i])){
                    activity_new$steps[i] <- avg_steps[i]
                     }
    }
    steps_daily_Total_new <- as.data.frame(with(activity_new,tapply(steps,date,sum,na.rm=TRUE)))
    rownames(steps_daily_Total_new) <-c(1:61)
    colnames(steps_daily_Total_new) <- "Daily_Steps_Sum"

### Histogram of the total number of steps taken each day after missing values are imputed

    hist(steps_daily_Total_new$Daily_Steps_Sum,main ="Histogram of Total Daily Steps after Imputing", xlab = "Daily Steps")
    abline(v=mean(steps_daily_Total_new$Daily_Steps_Sum,na.rm = TRUE),lwd =2,col="blue")
    text(9000,15,"Mean",col="blue")
    text(12225,25,"Median",col="magenta")
    abline(v=median(steps_daily_Total_new$Daily_Steps_Sum,na.rm = TRUE),lwd =2,col="magenta")

![](figure/histogram_total_daily_steps_imputed-1.png)

### Mean of total number of steps taken per day after imputing

    mean_daily_imputed <-mean(steps_daily_Total_new$Daily_Steps_Sum,na.rm = TRUE)

       The mean of total daily steps after imputing is 1.0766189\times 10^{4}

### Median of total number of steps taken per day after imputing

    median_daily_imputed <- median(steps_daily_Total_new$Daily_Steps_Sum)

      The median of total daily steps after imputing is 1.0766189\times 10^{4}
      
      The mean and median earlier were 9354.23 and 10395. After imputing the mean and median has increased and become equal, and the new value is 10766.19 

Are there differences in activity patterns between weekdays and weekends?
-------------------------------------------------------------------------

### Creating a new factor variable based on weekday and weekend

-   The ‘date’ variable of “activity\_new” dataframe has been identified
    as “weekday” and “weekend”
-   The date variable is replaced with “weekday” or “weekend”
-   Two subsets of new dataset -“activity\_new”, are created based on
    “weekday” and “weekend”
-   Average is taken by “tapply” across all intervals for the days for
    two subsets

<!-- -->

    wday <- weekdays(activity_new$date)
    for( i in 1:17568) {
        if(wday[i] =="Saturday"|wday[i]=="Sunday"){
             wday[i] <-"weekend"
        }
      else {wday[i] <-"weekday" }
      
    }
    activity_new$date <- wday
    weekday_steps <- subset(activity_new,date=="weekday")
    weekend_steps <- subset(activity_new,date=="weekend")
    steps__weekday <- as.data.frame(with(weekday_steps,tapply(steps,interval,mean,na.rm=TRUE)))
    steps__weekend <- as.data.frame(with(weekend_steps,tapply(steps,interval,mean,na.rm=TRUE)))
    names(steps__weekday) <- "Average_steps"
    names(steps__weekend) <- "Average_steps"

### Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends

    par(mfrow=c(1,2),mar=c(4,4,3,1))
    plot(as.numeric(rownames(steps__weekday)),steps__weekday$Average_steps,type ="l",xlab="24 Hour Interval of 5 Min each",ylab="Average Steps")
    title("Time Plot for Weekdays")

    plot(as.numeric(rownames(steps__weekend)),steps__weekend$Average_steps,type ="l",xlab="24 Hour Interval of 5 Min each",ylab="Average Steps",xlim =c(0000,2359))
    title("Time Plot  for Weekend")

![](figure/weekday%20versus%20weekend-1.png)
