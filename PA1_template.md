# Reproducible Research: Peer Assessment 1

    steps: Number of steps taking in a 5-minute interval (missing values are coded as NA)

    date: The date on which the measurement was taken in YYYY-MM-DD format

    interval: Identifier for the 5-minute interval in which measurement was taken



## Loading and preprocessing the data


```r
data <- read.csv(file = "activity.csv",header = TRUE,sep = ",")
```


## What is mean total number of steps taken per day?

Calculate the total number of steps taken per day.
We produce additional data frame without NA.


```r
dataNoNA <- data[complete.cases(data),]
stepsPerDay <- aggregate(dataNoNA$steps,by=list(dataNoNA$date), FUN=sum)
colnames(stepsPerDay) <- c("date","count")
```

Draw histogram:


```r
stat <- paste("Steps per day (max:",max(stepsPerDay[,"count"])," min:",min(stepsPerDay[,"count"]),
                   " median:",median(stepsPerDay[,"count"]), " mean:", round(mean(stepsPerDay[,"count"]),2),")")
hist(stepsPerDay[,"count"], main = "Histogram of calculated steps per day",xlab=stat)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

## What is the average daily activity pattern?

Average value is marked by red on following time series:


```r
plot(stepsPerDay[,"date"],stepsPerDay[,"count"],type="l")
abline(h=mean(stepsPerDay[,"count"]),col="red")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

- Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
stepsPerInterval <- aggregate(dataNoNA$steps,by=list(dataNoNA$interval), FUN=sum)
colnames(stepsPerInterval) <- c("interval","count")
stepsPerInterval[which.max(stepsPerInterval[,"count"]),]
```

```
##     interval count
## 104      835 10927
```


## Imputing missing values

- Number of NA:

```r
length(data[,1]) - length(dataNoNA[,1])
```

```
## [1] 2304
```

- Create new data set and fill missing data with zeros

```r
newData <- data 
newData[is.na(newData$steps),"steps"] <- 0
newData[is.na(newData$interval),"interval"] <- 0
stepsPerDayFilled <- aggregate(newData$steps,by=list(newData$date), FUN=sum)
hist(stepsPerDayFilled[,"x"])
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 


## Are there differences in activity patterns between weekdays and weekends?
