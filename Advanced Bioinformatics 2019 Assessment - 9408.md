---
title: "Advanced Bioinformatics 2019 Assessment"
author: '9408'
date: "28/03/2020"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

#Task 1

Using the sum() function and : operator, write an expression in the code snippet to evaluate the sum of all integers between 5 and 55. 

```{r}
sum(5:55)
```

#Task 2

Write a function called sumfun with one input parameter, called n, that calculates the sum of all integers between 5 and n. Use the function to do the calculation for n = 10, n = 20, and n = 100 and present the results. 

```{r}
n<-c(10, 20, 100)
sumfun=(n*(n+5))/2
print(sumfun)
```

#Task 3

The famous Fibonacci series is calculated as the sum of the two preceding members of the sequence, where the first two steps in the sequence are 1, 1. Write an R script using a for loop to calculate and print out the first 12 entries of the Fibonacci series. 

```{r}
len <- 12
fibvals <- numeric(len)
fibvals[1] <- 1
fibvals[2] <- 1
for (x in 3:len) {(fibvals[x] <- fibvals[x-1]+fibvals[x-2])} 

fibvals
```

#Task 4

With the mtcars dataset bundled with R, use ggplot to generate a box of miles per gallon (in the variable mpg) as a function of the number of gears (in the variable gear). Tip: use the as.factor function to convert the gear variable from numeric into factor. For bonus point: use the fill aesthetic to colour bars by number of gears (Tip: see ggplot2 cheat sheet at RStudio.com for different ggplot options). 

```{r}
library(ggplot2)

Task4<-ggplot(mtcars, aes(as.factor(gear), mpg, fill=as.factor(gear))) + geom_boxplot()+labs(title="Miles per Gallon as a Function of the Number of gears", fill="Number of Gears", x="Number of Gears", y="Miles per Gallon")
print(Task4)

```

## Task 5

Using the cars dataset and the function lm, fit a linear relationship between speed (in units of mph) and breaking distance (in units of feet) in the variable distance. What are the fitted slope and intercept of the line, and their standard erros? 

```{r}
y<-cars$dist
x<-cars$speed

#Build the linear model
mymodel <- lm(formula = y~x)
summary_mymodel<-summary(mymodel)


## extracting intercept, slope, standard deviation:
slp <- summary_mymodel$coefficients[2] #slope
intcp <- summary_mymodel$coefficients[1] #intercept
SD <- summary_mymodel$coefficients[,2]

print(paste0("The Slope = ", slp, " The Standard Deviation for The Slope = ", SD[2]))
print(paste0("The Intercept = ", intcp, " The Standard Deviation for The Intercept = ", SD[1]))

```

What are the units used for the variables in the dataset? breaking distance in feet and speed in miles per hour. 

## Task 6

Use ggplot to plot the data points from Task 6 and the linear fit. 

```{r}
Task5<-ggplot(data=cars, aes(x=speed, y=dist))+geom_point()+labs(y="Breaking Distance (feet)", x="Speed (mph)")+geom_smooth(method='lm', formula="y~x")
print(Task5)
```

## Task 7

Again using the cars dataset, now use linear regression (lm) to estimate the average reaction time for the driver to start breaking (in seconds). 

To simplify matters you may assume that once breaking commences, breaking distance is proportional to the square of the speed. 

Explain the steps in your analysis. Do you get reasonable results? Finally, use ggplot to plot the data points and the fitted relationship. (10pt)

Do you get reasonable results? yes

Finally, use ggplot to plot the data points and the fitted relationship.  

```{r}
ReactionTime <- lm(dist~I(speed^2)+0, data=cars) #make intercapt 0 beucase at 0 speed the stopping speed will also be 0
summary(ReactionTime)

#units for conversion
feetinamile<-5280
secondsinahour<-3600

#calculate the reaction distance 
AverageRDmph<-mean(ReactionTime$residuals)
AverageRDftph<-AverageRDmph*feetinamile
AverageRDftps<-AverageRDftph/secondsinahour
#find the converted mean of speed
MS<-mean(((cars$speed)/3600)*5480) 
#find the average reaction time 
AverageRTftps<-AverageRDftps/MS
print (paste0("The average reaction time in feet per second is ", AverageRTftps))

library(ggplot2)
ggplot(cars, aes(speed,dist))+geom_point()+labs(y="Distance for Vehlical to Stop (feet)", x="Speed of Vehical (mph)")+geom_smooth(method = "lm", formula = "y~I(x^2)+0")
```
