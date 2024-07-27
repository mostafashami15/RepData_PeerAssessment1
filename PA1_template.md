# Introduction

It is now possible to collect a large amount of data about personal
movement using activity monitoring devices such as a Fitbit, Nike
Fuelband, or Jawbone Up. These types of devices are part of the
“quantified self” movement – a group of enthusiasts who take
measurements about themselves regularly to improve their health, find
patterns in their behavior, or because they are tech geeks. However,
these data remain under-utilized both because the raw data are hard to
obtain and there is a lack of statistical methods and software for
processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring
device. This device collects data at 5-minute intervals throughout the
day. The data consists of two months of data from an anonymous
individual collected during the months of October and November, 2012,
and includes the number of steps taken in 5-minute intervals each day.

# Loading and Preprocessing the Data

In this section, I load the data from the CSV file, display the first
few rows, and check for missing values.

    # Load necessary libraries
    library(ggplot2)

    # Load the data from the CSV file
    data <- read.csv("activity.csv")

    # Display the first few rows of the dataset
    head(data)

    ##   steps       date interval
    ## 1    NA 2012-10-01        0
    ## 2    NA 2012-10-01        5
    ## 3    NA 2012-10-01       10
    ## 4    NA 2012-10-01       15
    ## 5    NA 2012-10-01       20
    ## 6    NA 2012-10-01       25

    # Check for missing values in the dataset
    sum(is.na(data$steps))

    ## [1] 2304

# Mean Total Number of Steps Taken per Day

## Calculate the Total Number of Steps per Day

Here, I calculate the total number of steps taken each day, ignoring
missing values.

    # Calculate the total number of steps taken each day, ignoring missing values
    total_steps_per_day <- aggregate(steps ~ date, data = data, sum, na.rm = TRUE)
    total_steps_per_day

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

## Histogram of Total Steps per Day

Next, I create a histogram of the total number of steps taken each day.

    # Create a histogram of the total number of steps taken each day

    hist(total_steps_per_day$steps, main = "Total Steps per Day", xlab = "Steps", breaks = 20)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-136-1.png)

    png("figure/total_steps_per_day.png")
    dev.off()

    ## quartz_off_screen 
    ##                 2

## Mean and Median of Total Steps per Day

I then calculate and report the mean and median of the total number of
steps taken per day.

    # Calculate the mean and median of the total number of steps taken per day
    mean_steps <- mean(total_steps_per_day$steps)
    median_steps <- median(total_steps_per_day$steps)
    mean_steps

    ## [1] 10766.19

    median_steps

    ## [1] 10765

# Average Daily Activity Pattern

## Time Series Plot of 5-Minute Intervals and Average Steps

In this section, I calculate the average number of steps taken in each
5-minute interval across all days and create a time series plot.

    # Load the ggplot2 library for creating the plot
    library(ggplot2)

    # Calculate the average number of steps taken in each 5-minute interval across all days
    avg_steps_per_interval <- aggregate(steps ~ interval, data = data, mean, na.rm = TRUE)

    # Create a time series plot of the average number of steps taken in each 5-minute interval
    ggplot(avg_steps_per_interval, aes(x = interval, y = steps)) +
      geom_line() +
      labs(title = "Average Daily Activity Pattern", x = "Interval", y = "Number of Steps")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-138-1.png)

    png("figure/Average Daily Activity Pattern.png")
    dev.off()

    ## quartz_off_screen 
    ##                 2

## 5-Minute Interval with Maximum Steps

I identify the 5-minute interval that, on average, contains the maximum
number of steps.

    # Identify the 5-minute interval that, on average, contains the maximum number of steps
    max_interval <- avg_steps_per_interval[which.max(avg_steps_per_interval$steps), ]
    max_interval

    ##     interval    steps
    ## 104      835 206.1698

# Imputing Missing Values

## Total Number of Missing Values

First, I calculate the total number of missing values in the dataset.

    # Calculate the total number of missing values in the dataset
    total_missing <- sum(is.na(data$steps))
    total_missing

    ## [1] 2304

## Filling Missing Values Strategy

I devise a strategy to fill in the missing values by using the average
for that 5-minute interval.

    # Create a copy of the dataset to fill missing values
    data_filled <- data

    # Fill in missing values using the average for that 5-minute interval
    for (i in 1:nrow(data_filled)) {
      if (is.na(data_filled$steps[i])) {
        data_filled$steps[i] <- avg_steps_per_interval$steps[avg_steps_per_interval$interval == data_filled$interval[i]]
      }
    }

## New Histogram with Filled Data

I then calculate the total number of steps taken each day with the
filled data and create a new histogram.

    # Calculate the total number of steps taken each day with filled data
    total_steps_per_day_filled <- aggregate(steps ~ date, data = data_filled, sum)

    # Create a histogram of the total number of steps taken each day with filled data
    hist(total_steps_per_day_filled$steps, main = "Total Steps per Day (Filled)", xlab = "Steps", breaks = 20)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-142-1.png)

    png("figure/total_steps_per_day_fille.png")
    dev.off()

    ## quartz_off_screen 
    ##                 2

## New Mean and Median

Finally, I calculate the mean and median of the total number of steps
taken per day with the filled data and compare these values to the
original estimates.

    # Calculate the mean and median of the total number of steps taken per day with filled data
    mean_steps_filled <- mean(total_steps_per_day_filled$steps)
    median_steps_filled <- median(total_steps_per_day_filled$steps)
    mean_steps_filled

    ## [1] 10766.19

    median_steps_filled

    ## [1] 10766.19

# Differences in Activity Patterns between Weekdays and Weekends

## Create a New Factor Variable for Weekday/Weekend

I create a new factor variable in the dataset indicating whether a given
date is a weekday or weekend.

    # Convert the date column to Date class
    data_filled$date <- as.Date(data_filled$date)

    # Create a new factor variable indicating whether a given date is a weekday or weekend
    data_filled$day_type <- ifelse(weekdays(data_filled$date) %in% c("Saturday", "Sunday"), "weekend", "weekday")

## Panel Plot of Average Steps per Interval by Day Type

I calculate the average number of steps taken in each 5-minute interval,
separately for weekdays and weekends, and create a panel plot.

    # Load the lattice library for creating the panel plot
    library(lattice)

    # Calculate the average number of steps taken in each 5-minute interval, separately for weekdays and weekends
    avg_steps_per_interval_filled <- aggregate(steps ~ interval + day_type, data = data_filled, mean)

    # Create a panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
    xyplot(steps ~ interval | day_type, data = avg_steps_per_interval_filled, type = "l", layout = c(1, 2),
           xlab = "Interval", ylab = "Number of Steps", main = "Average Number of Steps per Interval")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-145-1.png)

    png("figure/Average Number of Steps per Interval.png")
    dev.off()

    ## quartz_off_screen 
    ##                 2
