---
title: '[ ITDM ] Lecture 3 - Data Cleaning'
date: 2019-03-15 15:47:40
tags:
	- Data Cleaning
	- Data Mining
categories:
	- Programing
	- R
	- Data Mining
---

- Outline:
  - Data
  - Data selection
  - Data cleaning
  - Data normalization
  - Data discretization
    - equal width, equal depth, cluster analysis
  - Dimension reduction
    - Feature selection
    - Feature extraction: PCA

<!--more-->

# Data

- A collection of data objects and their attributes
- **Attribute**: property or characteristic of an object
  - variable, field, characteristic, or feature

- A collection of attrubures describe an object
  - record, point, case, sample, entity, or instance

## Types of attrubutes

- Nominal
  - id numbers, eye color, zip codes
- Ordinal
  - rankings, grades, height
- Interval
  - calendar dates, temperatures in Celsius or Fahrenheit
- Ratio
  - temperature in Kelvin, length, time, counts

## Properties of Attributes

- Distinctness: =&!=
- Order: <>
- Addition: + -
- Multiplication: */

---

- Nominal attribute: distinctness
- Ordinal attribute: distinctness & order
- interval attribute: distinctness, order & addition
- ratio attribute: all four properties

# Data Selection

## Sampling

- main technique
- often used for both **preliminary** investigation of the data and the **final** data analysis
- Why: Obtaining the **entire** dataset is too expensive or impossible
  - Processing the data is too expensive or time consuming

## Key Principle of Sampling: Representative

- using a sample should work as well as the entire dataset
- representative if it has approximately the **same distribution of properties** as the original set of data

## Types of Sampling

- Simple random sampling
- Sampling without replacement
- Sampling with replacement

- Stratified sampling

```R
sample(x, size, replace=FALSE, prob=NULL)
#sample 5 observations from grade
set.seed(1)
sampled_rows = sample(1:nrow(grade),5)
sampled_grade = grade[sampled_rows,]

#or equally using:
set.seed(1)
sampled_grade = grade[saple(1:nrow(grade), 5),]
```

# Data Cleaning

## Steps of Data Cleaning

- real-world data tends to be incomplete, noisy, and inconsistent
  - Handle missing values
  - Quality-check
  - Deduplicate

## Missing values

- Types:
  - Missing completely at random
  - missing at random
  - missing not at random

## Handling Missing Values

- Delete the observation
- Fill in missing values with:
  - the attribute mean, max, min, 0 or specified value
  - the attribute mean for all samples belonging to the same class (smarter)
  - the most probable value: inference-based such as Bayesian formula or decision tree

# Data Normalization

## Decimal Scaling

- $v' = \frac{v}{10^j}$
  - where j is the smallest integer such that max(|v'|)<1

## Min-Max Normalization

- linear transformation among the original data
- $v' = \frac{v-min_A}{max_A-min_A}(newMax_A-newMin_A)+new\_min_A$

## Z-Score Normalization

- 正态化

```R
x=c(1,3,5,7)
scale(x, center=T, scale=F) #不收拢
scale(x) #标准化
```

# Data Discretization

- Divide the range of a continuous attribute into intervals
- Reasons
  - algorithms only accept categorical attributes
  - reduce data size by discretization

## Equal-width Discretization

- L = $\frac{max-min}{N}$
- Characteristics:
  - The most straightforward method, but not handle well on **skewed** data.
  - Sensitive to N and **outliers**

## Equal-depth Discretization

- Divides the range into N intervals, each containing approximately the same number of samples.
- Characteristics
  - Good data scaling
  - Not so sensitive to outliers

## Cluster Analysis

- Discretize a numeric attribute by **partitioning** the values of the attribute into clusters or groups.
- Characteristics
  - 