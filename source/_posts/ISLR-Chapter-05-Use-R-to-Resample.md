---
title: '[ ISLR ] Chapter 05 - Use R to Resample'
date: 2019-02-15 10:50:07
tags:
	- Programing
	- Data Analysis
	- Statistical Learning
categories:
	- Programing
	- R
	- ISLR
---

- As is described in chapter 5, various method can be used to estimate test error rate
- This notes would covers **The validation set approach**, **LOOCV**,**k-Fold CV**, **Bootstrap**

<!--more-->

# The Validation Set Approach

## Divide the Subset

```R
library(ISLR)
set.seed(1)
train=sample(392,196)
```

## Fit the Model

```R
lm.fit=lm(mpg~horsepower,data=Auto,subset=train)
```

## Get MSE

```R
attach(Auto)
mean((mpg-predict(lm.fit,Auto))[-train]^2) #The reason why auto is inputed: including the validation set
```

## Fit the Polynomial Model

```R
lm.fit2 = lm(mpg~poly(horsepower,2),data = Auto,subset=train)
mean((mpg-predict(lm.fit2,Auto))[-train]^2)

lm.fit3 = lm(mpg~poly(horsepower,3),data = Auto,subset=train)
mean((mpg-predict(lm.fit3,Auto))[-train]^2)
```

## Alter the Training Set

```R
set.seed(2)
train = sample(392,196)

lm.fit=lm(mpg~horsepower,data=Auto,subset=train)
mean((mpg-predict(lm.fit,Auto))[-train]^2)
lm.fit2 = lm(mpg~poly(horsepower,2),data = Auto,subset=train)
mean((mpg-predict(lm.fit2,Auto))[-train]^2)
lm.fit3 = lm(mpg~poly(horsepower,3),data = Auto,subset=train)
mean((mpg-predict(lm.fit3,Auto))[-train]^2)
```

# LOOCV

## Perform Regression

- Perform linear regression using `glm`

```R
glm.fit = glm(mpg~horsepower,data=Auto)
```

## Get LOOCV Statistic

```R
library(boot)
cv.err = cv.glm(Auto,glm.fit)
cv.err$delta
```

- The test error get above is about 24.23
- The first is the standard CV estimate, while the other is a **Bias corrected version**

```R
cv.error = rep(0,5)
for (i in 1:5){
    glm.fit = glm(mpg~poly(horsepower,i),data=Auto)
    cv.error[i] = cv.glm(Auto,glm.fit)$delta[1]
}
cv.error
```

- The result is: ` 24.23151 19.24821 19.33498 19.42443 19.03321`

## k-Fold Test

```R
set.seed(17)
cv.error.10 = rep(0,10)
for (i in 1:10){
    glm.fit=glm(mpg~poly(horsepower,i),data=Auto)
    cv.error.10[i] = cv.glm(Auto,glm.fit,K=10)$delta[1]
}
cv.error.10
```

# The Bootstrap

## Procedure

1. Writing a function calculating the estimator

2. boot() from library boot to perform bootstrap

## Write the function:

```R
> alpha.fn = function(data,index){
+ X=data$X[index]
+ Y=data$Y[index]
+ return((var(Y)-cov(X,Y))/(var(X)+var(Y)-2*cov(X,Y)))
+ }

alpha.fn(Portfolia,1:100) #GET THE RESULT FOR ALL 100 Obervations
```

## Use `Sample()` to Resample

```R
set.seed(1)
alpha.fn(Portfolio,sample(100,100,replace=T)) #replace: can repeat?,100: range, time
```

## Use `boot()` to Simplify the Process

```R
boot(Portfolio,alpha.fn,R=1000)
```

# Estimate the Accuracy of an LR

## Write Function

```
> boot.fn = function(data,index)
+ return(coef(lm(mpg~horsepower,data=data,subset=index)))
```

## All Observations:

```R
boot.fn(Auto,1:392)
```

## Random Sampling:

```R
boot.fn(Auto,sample(392,392,replace=T))
boot.fn(Auto,sample(392,392,replace=T))
```

- Bootstrap:
- `boot(Auto,boot.fn,1000)`

## Compare the Result

- Because the formula to compute $SE(\hat{\beta}_0)$ depends on some assumptions but bootstrap does not.
- So **Bootstrap** is a better estimate