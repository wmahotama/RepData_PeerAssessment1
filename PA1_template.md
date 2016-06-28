Loading and preprocessing the data
----------------------------------

Load the data and assign to the object "raw". Then check the dimension,
head, and structure of the data.

    raw <- read.csv("./activity/activity.csv")

What is the mean total number of steps taken per day?
-----------------------------------------------------

Calculate the total number of steps taken per day.

    steps.day <- aggregate(data=raw,steps~date,sum)
    print(steps.day)

    ##          date steps
    ## 1  2012-10-02   126
    ## 2  2012-10-03 11352
    ## 3  2012-10-04 12116
    ## 4  2012-10-05 13294
    ## 5  2012-10-06 15420
    ## 6  2012-10-07 11015
    ## 7  2012-10-09 12811
    ## 8  2012-10-10  9900
    ## 9  2012-10-11 10304
    ## 10 2012-10-12 17382
    ## 11 2012-10-13 12426
    ## 12 2012-10-14 15098
    ## 13 2012-10-15 10139
    ## 14 2012-10-16 15084
    ## 15 2012-10-17 13452
    ## 16 2012-10-18 10056
    ## 17 2012-10-19 11829
    ## 18 2012-10-20 10395
    ## 19 2012-10-21  8821
    ## 20 2012-10-22 13460
    ## 21 2012-10-23  8918
    ## 22 2012-10-24  8355
    ## 23 2012-10-25  2492
    ## 24 2012-10-26  6778
    ## 25 2012-10-27 10119
    ## 26 2012-10-28 11458
    ## 27 2012-10-29  5018
    ## 28 2012-10-30  9819
    ## 29 2012-10-31 15414
    ## 30 2012-11-02 10600
    ## 31 2012-11-03 10571
    ## 32 2012-11-05 10439
    ## 33 2012-11-06  8334
    ## 34 2012-11-07 12883
    ## 35 2012-11-08  3219
    ## 36 2012-11-11 12608
    ## 37 2012-11-12 10765
    ## 38 2012-11-13  7336
    ## 39 2012-11-15    41
    ## 40 2012-11-16  5441
    ## 41 2012-11-17 14339
    ## 42 2012-11-18 15110
    ## 43 2012-11-19  8841
    ## 44 2012-11-20  4472
    ## 45 2012-11-21 12787
    ## 46 2012-11-22 20427
    ## 47 2012-11-23 21194
    ## 48 2012-11-24 14478
    ## 49 2012-11-25 11834
    ## 50 2012-11-26 11162
    ## 51 2012-11-27 13646
    ## 52 2012-11-28 10183
    ## 53 2012-11-29  7047

Make a histogram of the total number of steps taken each day

    hist(steps.day$steps)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-3-1.png)

Calculate and report the mean and median of the total number of steps
taken per day

    steps.mean <- mean(steps.day$steps)
    print(steps.mean)

    ## [1] 10766.19

    steps.median <- median(steps.day$steps)
    print(steps.median)

    ## [1] 10765

What is the average daily activity pattern?
-------------------------------------------

Make a time series plot (i.e. type = "l") of the 5-minute interval
(x-axis) and the average number of steps taken, averaged across all days
(y-axis)

    steps.interval <- aggregate(data = raw, steps~interval,mean)
    plot(steps.interval$interval,steps.interval$steps, type = "l", xlab = "Time Interval", ylab = "Number of Steps", main = "Average Daily Activity")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-5-1.png)

Which 5-minute interval, on average across all the days in the dataset,
contains the maximum number of steps?

    max.step <- max(steps.interval$steps)
    max.interval <- subset(steps.interval, steps == max.step)
    print(max.interval)

    ##     interval    steps
    ## 104      835 206.1698

Imputing missing values
-----------------------

Calculate and report the total number of missing values in the dataset
(i.e. the total number of rows with NAs)

    sum(!complete.cases(raw))

    ## [1] 2304

Replace NA values with the average number of steps based on the interval
at which the NA value occurs.

    test <- is.na(raw$steps)
    yes <- steps.interval$steps[match(raw$interval,steps.interval$interval)]
    no <- raw$steps

    steps.imputed <- ifelse(test,yes,no)

Create a new dataset that is equal to the original dataset but with the
missing data filled in.

    data.imputed <- transform(raw,steps = steps.imputed)

Make a histogram of the total number of steps taken each day and
Calculate and report the mean and median total number of steps taken per
day.

Calculate the total number of steps taken per day.

    steps.day.imputed <- aggregate(data=data.imputed,steps~date,sum)
    print(steps.day.imputed)

    ##          date    steps
    ## 1  2012-10-01 10766.19
    ## 2  2012-10-02   126.00
    ## 3  2012-10-03 11352.00
    ## 4  2012-10-04 12116.00
    ## 5  2012-10-05 13294.00
    ## 6  2012-10-06 15420.00
    ## 7  2012-10-07 11015.00
    ## 8  2012-10-08 10766.19
    ## 9  2012-10-09 12811.00
    ## 10 2012-10-10  9900.00
    ## 11 2012-10-11 10304.00
    ## 12 2012-10-12 17382.00
    ## 13 2012-10-13 12426.00
    ## 14 2012-10-14 15098.00
    ## 15 2012-10-15 10139.00
    ## 16 2012-10-16 15084.00
    ## 17 2012-10-17 13452.00
    ## 18 2012-10-18 10056.00
    ## 19 2012-10-19 11829.00
    ## 20 2012-10-20 10395.00
    ## 21 2012-10-21  8821.00
    ## 22 2012-10-22 13460.00
    ## 23 2012-10-23  8918.00
    ## 24 2012-10-24  8355.00
    ## 25 2012-10-25  2492.00
    ## 26 2012-10-26  6778.00
    ## 27 2012-10-27 10119.00
    ## 28 2012-10-28 11458.00
    ## 29 2012-10-29  5018.00
    ## 30 2012-10-30  9819.00
    ## 31 2012-10-31 15414.00
    ## 32 2012-11-01 10766.19
    ## 33 2012-11-02 10600.00
    ## 34 2012-11-03 10571.00
    ## 35 2012-11-04 10766.19
    ## 36 2012-11-05 10439.00
    ## 37 2012-11-06  8334.00
    ## 38 2012-11-07 12883.00
    ## 39 2012-11-08  3219.00
    ## 40 2012-11-09 10766.19
    ## 41 2012-11-10 10766.19
    ## 42 2012-11-11 12608.00
    ## 43 2012-11-12 10765.00
    ## 44 2012-11-13  7336.00
    ## 45 2012-11-14 10766.19
    ## 46 2012-11-15    41.00
    ## 47 2012-11-16  5441.00
    ## 48 2012-11-17 14339.00
    ## 49 2012-11-18 15110.00
    ## 50 2012-11-19  8841.00
    ## 51 2012-11-20  4472.00
    ## 52 2012-11-21 12787.00
    ## 53 2012-11-22 20427.00
    ## 54 2012-11-23 21194.00
    ## 55 2012-11-24 14478.00
    ## 56 2012-11-25 11834.00
    ## 57 2012-11-26 11162.00
    ## 58 2012-11-27 13646.00
    ## 59 2012-11-28 10183.00
    ## 60 2012-11-29  7047.00
    ## 61 2012-11-30 10766.19

Calculate and report the mean and median of the total number of steps
taken per day

    steps.mean.imputed <- mean(steps.day.imputed$steps)
    print(steps.mean.imputed)

    ## [1] 10766.19

    steps.median.imputed <- median(steps.day.imputed$steps)
    print(steps.median.imputed)

    ## [1] 10766.19

Make a histogram of the total number of steps taken each day

    hist(steps.day.imputed$steps)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-12-1.png)

Do these values differ from the estimates from the first part of the
assignment? Yes, the values differ from the estimates from the first
part of this assignment.

What is the impact of imputing missing data on the estimates of the
total daily number of steps? Imputing the missing data on the estimates
of the total daily number of steps increases the median to equal the
mean.

Are there differences in activity patterns between weekdays and weekends?
-------------------------------------------------------------------------

Create a new column to store the day of the week.

    data.imputed$day <- weekdays(as.Date(data.imputed$date))

Create a new column which classifies the day of the week into two
categories, weekday or weekend.

    weekday <- c("Monday","Tuesday","Wednesday","Thursday","Friday")
    classification <- ifelse(is.element(data.imputed$day,weekday),"Weekday","Weekend")
    data.imputed$dow <- as.factor(classification)

Subset data by weekend and weekday classification

    weekday.data <- subset(data.imputed, dow == "Weekday")
    weekend.data <- subset(data.imputed, dow == "Weekend")

    str(weekday.data)

    ## 'data.frame':    12960 obs. of  5 variables:
    ##  $ steps   : num  1.717 0.3396 0.1321 0.1509 0.0755 ...
    ##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
    ##  $ day     : chr  "Monday" "Monday" "Monday" "Monday" ...
    ##  $ dow     : Factor w/ 2 levels "Weekday","Weekend": 1 1 1 1 1 1 1 1 1 1 ...

    str(weekend.data)

    ## 'data.frame':    4608 obs. of  5 variables:
    ##  $ steps   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 6 6 6 6 6 6 6 6 6 6 ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
    ##  $ day     : chr  "Saturday" "Saturday" "Saturday" "Saturday" ...
    ##  $ dow     : Factor w/ 2 levels "Weekday","Weekend": 2 2 2 2 2 2 2 2 2 2 ...

Calculate the activity patterns during the weekdays and weekends

    par(mfrow=c(1,2))

    weekday.activity <- aggregate(data = weekday.data, steps~interval,mean)
    weekday.plot <- plot(weekday.activity$interval,weekday.activity$steps, xlab = "Time Interval", ylab = "Number of Steps", ylim = c(0,250), main = "Average Weekday Activity", type = "l")

    weekend.activity <- aggregate(data = weekend.data, steps~interval,mean)
    weekend.plot <- plot(weekend.activity$interval,weekend.activity$steps, xlab = "Time Interval", ylab = "Number of Steps", ylim = c(0,250), main = "Average Weekend Activity", type = "l")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-16-1.png)
