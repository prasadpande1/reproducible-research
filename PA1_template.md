---
title: "Reproducible Project 1"
author: "Prasad P"
date: "29 October 2018"
output:
  word_document: default
  html_document: default
  pdf_document: default
---
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
par(mfrow=c(1,1))
```
## R Markdown
#1. Code for reading in the dataset and/or processing the data
```{r Readfile, echo=TRUE}
actv_file <- read.csv("C:/Users/prasad.pande/Documents/activity.csv", header = TRUE)

```
#2. Histogram of the total number of steps taken each day
```{r Stepsperday, echo=TRUE}
Stepsperday <- tapply(actv_file$steps, actv_file$date, sum)
summary(Stepsperday)
hist(Stepsperday, main = "Histogram showing steps per day")
```

#3. Mean and median number of steps taken each day
```{r meanandmedian, echo=TRUE}
Meanperday <- mean(Stepsperday, na.rm=TRUE)
Meanperday
Medianperday <- median(Stepsperday, na.rm = TRUE)
Medianperday
```
#4. Time series plot of the average number of steps taken
```{r averagesteps, echo=TRUE}
intervalsteps <- tapply(actv_file$steps, actv_file$interval, mean, na.rm = TRUE)
#colnames(intervalsteps) <- c("Steps", "Interval")
plot(as.numeric(names(intervalsteps)), intervalsteps, xlab="Interval", ylab="Step Count",type = "l", main = "Time series plot of the average number of steps")
```
#5. The 5-minute interval that, on average, contains the maximum number of steps
```{r maxsteps, echo=TRUE}
maxsteps <- names(sort(intervalsteps, decreasing = TRUE)[1])
maxsteps

```
#6. Code to describe and show a strategy for imputing missing data
```{r impute, echo=TRUE}
library(tidyr)
#tidyr package is used for using replace_NA. I will be replacing NA by mean of steps
actv_file$steps <- actv_file$steps %>% replace_na(as.integer(mean(actv_file$steps, na.rm = TRUE)))
summary(actv_file)

```
#7. Histogram of the total number of steps taken each day after missing values are imputed
```{r Stepsperday1, echo=TRUE}
class(actv_file)
Stepsperday <- tapply(actv_file$steps, actv_file$date, sum)
summary(Stepsperday)
hist(Stepsperday, main = "Histogram showing steps per day post imputation")
```
#8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
```{r}
actv_file.date1 <- ifelse(weekdays(as.Date(actv_file$date))== "Saturday"|weekdays(as.Date(actv_file$date))== "Sunday","Weekend", "Weekday")

actv_file.weekend <- tapply(actv_file[actv_file.date1 == "Weekend",]$steps,
                                  actv_file[actv_file.date1 == "Weekend",]$interval,
                                  mean, na.rm=TRUE)
actv_file.weekday <- tapply(actv_file[actv_file.date1 == "Weekday",]$steps,
                                  actv_file[actv_file.date1 == "Weekday",]$interval,
                                  mean, na.rm=TRUE)
# setting the 1 rows and 2 column plot
par(mfrow=c(1,2))

#Time series plot of the average number of steps on Weekdays
plot(as.numeric(names(actv_file.weekday)), actv_file.weekday, type = "l",
     xlab = "Interval", ylab = "Steps", main = "Time series plot of the average number of steps on Weekdays" )

#Time series plot of the average number of steps on Weekend
plot(as.numeric(names(actv_file.weekend)), actv_file.weekend, type = "l",
     xlab = "Interval", ylab = "Steps", main = "Time series plot of the average number of steps on Weekend" )

```

#9. All of the R code needed to reproduce the results (numbers, plots, etc.) in the report

