---
title: '[ CMOR ] Lecture 04 - Linear Programming'
date: 2019-03-19 10:13:21
tags:
	- Operations Research
categories:
	- Operations Research
	- Managerial operations research
---

# LP Formulaiton Guidelines

<!--more-->

## Guidelines for formulation:

1. Understand the problem
2. ask the following three questions:
   1. decision variables
   2. what measure should we use to ompare alternative sets of decisions
   3. what restrictions
3. mathematical representation of the objective function in terms of the decision variables
4. write the constraints in terms of the decision vatiables
5. check your formulation!
   - Does it make  sense?
   - Is there any data in the problem you haven't used?

# Post Office

- The number of full-time employees needed  on each day is as follows:
  - 7,3,5,9,4,6,1分别对应周一到周日
- 公会要求，每个全职雇员应该连续工作五天并且连续休息两天
- 邮局必须满足需求，使用全职雇员
- 目标：最小化使用的雇员数量
- 题目说明：
  - ![](lp1q.png)

## 解答：

![](lp1.png)

- 另外讨论两种Alternative Question
- 如果有临时工：
  - 目标：$min(\sum^{7}_{i=1}[c_ix_i+c_i'y_i])$ 
  - s.t.: Example of Monday: $x_1+x_4+x_5+x_6+x_7+y_1 == 7$

# Blending

- three basic blends in 1 pound bags: special, mountain dark, mil regular
- two different types of beans to produce the blends: brazilian and mild
- blend recipe:

| Blend   | Mix requirements                                   | selling price(dollar/LB) |
| ------- | -------------------------------------------------- | ------------------------ |
| special | 40% brazilian, no more than 30% mild               | 6.5                      |
| dark    | 60% Brazilian, no more than 10% mild               | 5.25                     |
| regular | no more than 60% brazilian, at least 30% brazilian | 3.75                     |

- brazilian coffee beans: 2 dollar/pound, while mild is 1.25/pound
- brazilian <= 210 pounds and mild coffee <= 300 pounds
- To maximize the profit

- ![](be.png)

## 解答：

![](lp2.png)

![](lp2a.png)

# 投资组合：

- ![](lp3q.png)
- 不去管下面的示例表
- 本质上要最小化专款专用的投入额

## 解答：

![](tuition.png)

# 生产问题：

![](pro.png)

## 解答：

![](proa.png)

- 不是说变量越少越好，有时候多一些变量会更好

# 路径问题：

![](net.png)

## 解答：

- 每条线一个变量
- ![](neta.png)

# 分配问题：

![](assq.png)

![](assa.png)

![](assa2.png)

# 总结：

- ![](summ.png)