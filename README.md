---
title: "Motor MPG Data Analysis"
author: "P V Vineet"
date: "Nov 20, 2020"
output: html_document
---

#### Executive Summary
This report analyzed the relationship between transmission type (manual or 
automatic) and miles per gallon (MPG). The report set out to determine which 
transmission type produces a higher MPG. The `mtcars` dataset was used for this
analysis. A t-test between automatic and manual transmission vehicles shows that
manual transmission vehicles have a 7.245 greater MPG than automatic 
transmission vehicles.

#### Load Data
Load the dataset and convert categorical variables to factors.
```{r results='hide', message=FALSE}
library(ggplot2)
data(mtcars)
head(mtcars, n=3)
dim(mtcars)
mtcars$cyl <- as.factor(mtcars$cyl)
mtcars$vs <- as.factor(mtcars$vs)
mtcars$am <- factor(mtcars$am)
mtcars$gear <- factor(mtcars$gear)
mtcars$carb <- factor(mtcars$carb)
attach(mtcars)
```

#### Exploratory Analysis
 Exploratory Box graph that compares Automatic and Manual 
transmission MPG. The graph leads us to believe that there is a  
increase in MPG when for vehicles with a manual transmission vs automatic.

##### Statistical Inference
T-Test transmission type and MPG
```{r}
tResults <- t.test(mpg ~ am)
tResults$p.value
```
The T-Test rejects the null hypothesis that the difference between transmission
types is 0.  
```{r}
tResults$estimate
```
The difference estimate between the 2 transmissions is 7.24494 MPG in favor of 
manual.

##### Regression Analysis
Fit the full model of the data
```{r results='hide'}
FMT <- lm(mpg ~ ., data = mtcars)
summary(FMT)  # results hidden
summary(FMT)$coeff  # results hidden
```
Since none of the coefficients have a p-value less than 0.05 we cannot conclude
which variables are more statistically significant. 

Backward selection to determine which variables are most statistically 
significant
```{r results='hide'}
sFit <- step(FMT)
summary(sFit) # results hidden
summary(sFit)$coeff # results hidden
```

The p-values also are statistically significantly because they
have a p-value less than 0.05. The coefficients conclude that increasing the 
number of cylinders from 4 to 6 with decrease the MPG by 3.03.  Further 
increasing the cylinders to 8 with decrease the MPG by 2.16.  Increasing the 
horsepower is decreases MPG 3.21 for every 100 horsepower.
A Manual transmission improves the MPG by1.81.

#### Residuals & Diagnostics
Plot
**See Appendix Figure II**

The plots conclude:

1. The randomness of the Residuals vs. Fitted plot supports the assumption of
independence
2. Since all points are within the 0.05 lines, the Residuals vs. Leverage 
concludes that there are no outliers
```{r}
sum((abs(dfbetas(sFit)))>1)
```

#### Conclusion
There is a difference in MPG based on transmission type. A manual transmission
will have a slight MPG boost. However, it seems that weight, horsepower, & 
number of cylinders are more statistically significant when determining MPG.

### Appendix Figures

#### I
```{r echo=FALSE}
  boxplot(mpg ~ am, 
          xlab="Transmission Type (0 = Automatic, 1 = Manual)", 
          ylab="MPG",
          main="MPG by Transmission Type")
```

#### II
```{r echo=FALSE}
par(mfrow = c(2, 2))
plot(sFit)
```
