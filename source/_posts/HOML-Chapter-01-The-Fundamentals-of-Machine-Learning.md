---
title: '[ HOML ] Chapter 01 - The Fundamentals of Machine Learning'
date: 2019-02-15 15:17:11
tags:
	- Machine Learning
	- Python
	- Programing
categories:
	- Programing
	- Python
	- Hands On Machine Learning
---

- A famous up on YouTube recommand a book called \<Hands On Machine Learning with Scikit-Learn and TensorFlow\>
- The notes covers: **Why use machine learning**, **types of machine learning system**, **Main Challenges of Machine Learning**

<!--more-->

 # Why Use Machine Learning

- Problems for which  existing solutions require a lot of hand-tuning or long lists of rules(spam filer).
- Complex problems for which there is no good solution at all using a traditional approach(speech recognition).

- Fluctuating environments: a Machine Learning system can adapt to new data.(speech recognition)
- Getting insights about complex problems and large amounts of data(data mining)

# Types

## Supervised/Unsupervised Learning

### Supervised Learning

- Classification
- Regression
- Popular algorithms
  - KNN
  - Linear Regression
  - Logistic Regression
  - Support Vector Machines
  - Decision Trees and Random Forests
  - Neural networks(Partly)

### Unsupervised learning

- popular tasks:
  - cluster
  - visualization
  - dimensionality reduction
  - anomaly detection
- Clustering
  - k-means
  - Hierarchical Cluster Analysis
  - Expectation Maximization
- Visualization and dimensionality reduction
  - PCA
  - Kernel PCA
  - Locally-Linear Embedding
  - t-distributed Stochastic Neighbor Embedding
- Association rule learning
  - Apriori
  - Eclat

### Semisupervised learning

- Tasks
  - photo-hosting services(cluster the same person and name the person)
- Algorithms
  - deep belief networks

### Reinforcement Learning

- **agent**, the learning system observe the environment itself, select and perform actions, and get **rewards** or  **penalty**  in return.

## Batch and Online Learning

### Batch learning

- Also called **offline learning**, as it is trained using all the available data and apply what is has learned.
- training on the full set of data

### Online  learning

- feeding it data instances sequentially, either individually or by small goups called *mini-batches*
- good for limited computing resource: you can discard your data if the model is updated
- **Learning rate** is an important parameter for an online learning system, deciding how fast they should adapt to changing data or how fast it forget old data.
- Challenge: bad data feeding can greatly decline the performance of the system
  - You should monitor your system closely and promptly switch learning off.

## Instance/Model Based Learning

### Instance based learning

{%asset_img IBL.png%}

### Model based learning

{%asset_img MBL.png%}

- To measure the fitness of the mdoel:
  - you can specify a performance measure. define a **utility function(fitness function)**
  - or define a **cost function** define how bad the model is.
- Code examples:

```python
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import sklearn

oecd_bli = pd.read_csv('datasets/lifesat/oecd_bli_2015.csv',thousands=',')
gdp_per_capita = pd.read_csv("datasets/lifesat/gdp_per_capita.csv",thousands=',',delimiter='\t',encoding='latin1',na_values='n/a')

#oecd_bli.INEQUALITY.drop_duplicates()
oecd_bli = oecd_bli[oecd_bli["INEQUALITY"]=="TOT"]
oecd_bli = oecd_bli.pivot(index="Country", columns="Indicator", values="Value")
oecd_bli

gdp_per_capita.rename(columns={"2015": "GDP per capita"}, inplace=True) #把原本叫2015的列重命名为gdp...
gdp_per_capita.set_index("Country", inplace=True)
full_country_stats = pd.merge(left=oecd_bli, right=gdp_per_capita, #index=True 使用index作为合并的公共键
                                  left_index=True, right_index=True)
full_country_stats.sort_values(by="GDP per capita", inplace=True)

remove_indices = [0, 1, 6, 8, 33, 34, 35]
keep_indices = list(set(range(36)) - set(remove_indices))
country_stats = full_country_stats[["GDP per capita", 'Life satisfaction']].iloc[keep_indices]

x = np.c_[country_stats["GDP per capita"]] #将数组拆开（一号轴）然后重组（二号轴）‘
y = np.c_[country_stats["Life satisfaction"]]
country_stats.plot(kind='scatter', x="GDP per capita", y='Life satisfaction')
plt.show()
# Select a linear model
model = sklearn.linear_model.LinearRegression()

# Train the model
model.fit(X, y)

# Make a prediction for Cyprus
X_new = [[22587]]  # Cyprus' GDP per capita
print(model.predict(X_new)) # outputs [[ 5.96242338]]
```

- {%asset_img post.png%}



# Main Challenges of ML

## Insufficient Quantity of Training Data

- The Unreasonable effectiveness of data

## Nonrepresentative Training Data

- Sample selection bias
- US presidential election in 1936

## Poor-Quality Data

- Outliers
- Missing a few features

## Irrelevant features

- Process Feature Engineering
  - feature selection
  - feature extraction
  - create new features by gathering new data

## Overfitting the Training Data

- **Regularization** : constraining a model to make it simpler and reduce the risk of overfitting
- degrees of freedom: the number of parameters

- **Hyper-parameter** to control for the amount of regularization

## Underfitting the Training Data

- The model is too **simple**