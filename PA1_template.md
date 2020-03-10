---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

```r
library(tidyverse)
```

```
## Registered S3 method overwritten by 'dplyr':
##   method           from
##   print.rowwise_df
```

```
## Registered S3 methods overwritten by 'dbplyr':
##   method         from
##   print.tbl_lazy     
##   print.tbl_sql
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.2.1     √ purrr   0.3.3
## √ tibble  2.1.3     √ dplyr   0.8.4
## √ tidyr   1.0.2     √ stringr 1.4.0
## √ readr   1.3.1     √ forcats 0.4.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(knitr)
Sys.setlocale("LC_TIME","English")

dt <- read.csv("activity.csv", sep = ",")
```


## What is mean total number of steps taken per day?
1. Shows a histogram of the total number of steps taken each day

```r
dt_pday <- dt %>% group_by(date) %>% summarise(tot_steps = sum(steps, na.rm = TRUE))

ggplot(dt_pday, aes(tot_steps)) + geom_histogram(binwidth = 1000) +
  labs(title = "The total number of steps taken each day", x = "The total number of steps", y = "Frequency") +
  theme_bw()
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```r
mean_pday <- round(mean(dt_pday$tot_steps, na.rm = TRUE), 0)
median_pday <- median(dt_pday$tot_steps, na.rm = TRUE)
```
2. The mean of the total number of steps taken per day is 9354 and the median of the total number of steps taken per day is 10395.


## What is the average daily activity pattern?
1. Shows a time series plot of the 5-minute interval

```r
dt_pitv <- dt %>% group_by(interval) %>% summarise(avg_steps = mean(steps, na.rm = TRUE))

ggplot(dt_pitv, aes(x = interval, y = avg_steps)) + geom_line() +
  labs(title = "The average number of steps taken each interval", x = "Interval", y = "The average number of steps") +
  theme_bw()
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)

```r
max_pitv <- round(max(dt_pitv$avg_steps))
```
2. Amongst the 5-minute interval, 206 contains the maximum number of steps.


## Imputing missing values

```r
na_steps <- sum(is.na(dt$steps))
```
1. The total number of missing values in the dataset is 2304.

2. Shows a histogram of the total number of steps taken each day.

```r
dt1 <- dt
dt1[is.na(dt1$steps), 1] <- 0

dt1_pday <- dt1 %>% group_by(date) %>% summarise(tot_steps = sum(steps, na.rm = TRUE))

ggplot(dt1_pday, aes(tot_steps)) + geom_histogram(binwidth = 1000) +
  labs(title = "The total number of steps taken each day", x = "The total number of steps", y = "Frequency") +
  theme_bw()
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

```r
mean1_pday <- round(mean(dt1_pday$tot_steps, na.rm = TRUE), 0)
median1_pday <- median(dt1_pday$tot_steps, na.rm = TRUE)
```
3. The mean of the total number of steps taken per day is 9354 and the median of the total number of steps taken per day is 1.0395 &times; 10<sup>4</sup>.


## Are there differences in activity patterns between weekdays and weekends?
1. Shows a panel plot containing a time series plot of the 5-minute interval by weekday

```r
dt1$date <- as.Date(as.character(dt1$date), format = "%Y-%m-%d")
dt1 <- dt1 %>% mutate(day = weekdays(date, abbreviate = TRUE),
                                weekday = case_when(day %in% c("Mon", "Tue", "Wed", "Thu", "Fri") ~ "weekday",
                                                    day %in% c("Sat", "Sun") ~ "weekend"))

dt1_pitv <- dt1 %>% group_by(weekday, interval) %>% summarise(avg_steps = mean(steps, na.rm = TRUE))

ggplot(dt1_pitv, aes(x = interval, y = avg_steps)) + geom_line() +
  labs(title = "The average number of steps taken each interval by day", x = "Interval", y = "The average number of steps") +
  facet_wrap(~ weekday) +
  theme_bw()
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

## Makes PA1_template.md and PA1_template.html files with knit2html() function

```r
knitr::knit2html(input = "PA1_template.Rmd", output = "PA1_template.md", force_v1 = TRUE)
```

```
## 
## 
## processing file: PA1_template.Rmd
```

```
##   |                                                                       |                                                               |   0%  |                                                                       |....                                                           |   7%
##   ordinary text without R code
## 
##   |                                                                       |.........                                                      |  14%
## label: unnamed-chunk-9 (with options) 
## List of 1
##  $ results: chr "hide"
## 
##   |                                                                       |..............                                                 |  21%
##   ordinary text without R code
## 
##   |                                                                       |..................                                             |  29%
## label: unnamed-chunk-10
```

```
##   |                                                                       |......................                                         |  36%
##    inline R code fragments
## 
##   |                                                                       |...........................                                    |  43%
## label: unnamed-chunk-11
```

```
##   |                                                                       |................................                               |  50%
##    inline R code fragments
## 
##   |                                                                       |....................................                           |  57%
## label: unnamed-chunk-12
##   |                                                                       |........................................                       |  64%
##    inline R code fragments
## 
##   |                                                                       |.............................................                  |  71%
## label: unnamed-chunk-13
```

```
##   |                                                                       |..................................................             |  79%
##    inline R code fragments
## 
##   |                                                                       |......................................................         |  86%
## label: unnamed-chunk-14
```

```
##   |                                                                       |..........................................................     |  93%
##   ordinary text without R code
## 
##   |                                                                       |...............................................................| 100%
## label: unnamed-chunk-15
```

```
## output file: PA1_template.md
```
