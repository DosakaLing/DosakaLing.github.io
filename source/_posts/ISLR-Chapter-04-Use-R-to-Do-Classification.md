---
title: '[ ISLR ] Chapter 04 - Use R to Do Classification'
date: 2019-02-12 21:50:01
tags:
	- Classification
	- R
categories:
	- Programing
	- R
	- ISLR
---

- It's time to apply [Classification Model](https://dosakaling.github.io/Data-Analysis/Statistical-Learning/ISLR-Chapter-04-Classification/) into practice!
- The notes would cover: **Data Description, Logistic Regression, LDA, QDA, KNN ** and an **Example**

<!--more-->

# Stock Market Data

- percentage returns for the S&P 500 stock index over 1250 days, from 2011 to end of 2005.
- Load the data

```R
library(ISLR)
names(Smarket)
dim(Smarket)
summary(Smarket)
pairs(Smarket) #散点矩阵图
```

- A glance at the dataset:

{%asset_img smarket.png%}

- Use `cor(matrix)` to view the correlations between predictors, if some predictors are **qualitative**, they should be removed

```R
cor(Smarket[,-9])
attach(Smarket)
plot(Volume)
```

# Logistic Regression

## Model

- Use `glm(model formula)` to fit a **generalized linear model** including logistic regression, indicated by pass **family=binomial**

```R
glm.fit = glm(Direction~Lag1+Lag2+Lag3+Lag4+Lag5+Volume,data=Smarket,family=binomial)
summary(glm.fit)
```

- Refit the model using subset of data:

```R
glm.fit = glm(Direction~Lag1+Lag2+Lag3+Lag4+Lag5+Volume,data=Smarket,family=binomial,subset=train)
glm.probs = predict(glm.fit,Smarket.2005,type="response")
```

- Refit the model using less predictors:

```R
glm.fit = glm(Direction~Lag1+Lag2, data=Smarket,family=binomial,subset=train)
glm.probs = predict(glm.fit,Smarket.2005,type="response")
```



## Predict

- `predict(glm.fit)` predict the logit($\beta_0+\beta_1X​$)
- `predict(glm.fit,type=response)` predict the probability of **up**
- The value is the probability of **up** because of what  `contrasts(Direction)` show

```
> contrasts(Direction)
		Up
Down 	0
Up		1
```

- Convert the predict value to **label**

```R
glm.pred = rep("Down",1250)
glm.pred[glm.probs>0.5] = "Up"
```

- To predict a **particular day**

```
predict(glm.fit,newdata=data.frame(Lag1=c(1.2,1.5),Lag2=c(1.1,-0.8)),type="response")
```



## Assessment 

### Confusion Matrix

```R
table(glm.pred,Direction)
```

### Correction Rate

```R
mean(glm.pred==Direction)
```

### Separate the training data

```R
train = (Year<2005)
Smarket.2005 = Smarket[!train,]
dim(Smarket.2005)
Direction.2005 = Direction[!train]
```

- The same code in Python would be:
  - `Smarket['2005'] = Smarket[Smarket.Year>2005]`
  - `Direction['2005'] = Smarket[Smarket.Year>2005].Direction`

### Refit

- See the above

### Re-assess(subset data)

```R
glm.pred = rep("Down",252)
glm.pred[glm.probs>0.5] = "Up"
table(glm.pred,Direction.2005)
mean(glm.pred==Direction.2005)
```

### Re-assess(less predictor)

```R
glm.pred = rep("Down",252)
glm.pred[glm.probs>0.5] = "Up"
table(glm.pred,Direction.2005)
```

# LDA

## Fit Model

- Use `lad(formula)` to fit a LDA model, which is part of `MASS` package

```R
lda.fit = lda(Direction~Lag1+Lag2,data=Smarket,subset=train)
```

- result:

```R
Call:
lda(Direction ~ Lag1 + Lag2, data = Smarket, subset = train)

Prior probabilities of groups:
    Down       Up 
0.491984 0.508016 

Group means:
            Lag1        Lag2
Down  0.04279022  0.03389409
Up   -0.03954635 -0.03132544

Coefficients of linear discriminants:
            LD1
Lag1 -0.6420190
Lag2 -0.5135293
```

## Assessment

- `plot(lda.fit)` produces plot of the *linear discriminants*
- `lda.pred=predict(lda.fit,Smarket.2005)`
- `names(lda.pred)`
  - **class** contains LDA's predictions about the movement of the market
  - **posterior** a matrix whose kth column contains the posterior probability belong to the kth class
  - **x** contains the linear discriminants
- `lda.class = lda.pred$class`
- `table(lda.class,Direction.2005)`
- `mean(lda.class==Direction.2005)`

## Change the Threshold

- `sum(lda.pred$posterior[,1]>=0.5)`
- `sum(lda.pred$posterior[,1]>0.9)`

- **\* remember to test whether class is correspond to the larger probability**
  - `lda.pred$posterior[1:20,1]`
  - `lda.class[1:20]`