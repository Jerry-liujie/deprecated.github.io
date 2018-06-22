---
layout:     post
title:      Uncorrelated vs Independent
subtitle:   这俩有什么区别?
date:       2018-06-22
author:     Jie
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Statistics
---

**Covariance** is a measure of the joint variability of two random variables. If the greater values of one variable mainly correspond with the greater values of the other variable, and the same holds for the lesser values, (i.e., the variables tend to show similar behavior), the covariance is positive. In the opposite case, when the greater values of one variable mainly correspond to the lesser values of the other, (i.e., the variables tend to show opposite behavior), the covariance is negative. 

The **sign of the covariance** therefore shows the tendency in the **linear relationship** between the variables. The normalized version of the covariance, the correlation coefficient,  shows by its magnitude the strength of the linear relation.

 Two random variables, X,Y, are said to be uncorrelated if their covariance, E(XY) − E(X)E(Y), is zero. 
>If two variables are uncorrelated, there is no linear relationship between them.

If X and Y are independent, with finite second moments, then they are uncorrelated. However, not all uncorrelated variables are independent. For example, if X is a continuous random variable uniformly distributed on [−1, 1] and $$Y = X^2$$, then X and Y are uncorrelated even though X determines Y.

![Pearson](img/post-bg-debug.png)

If the variables are independent, Pearson's correlation coefficient is 0, but the converse is not true because the correlation coefficient detects only linear dependencies between two variables. However, in the special case when X and Y are jointly normal, uncorrelatedness is equivalent to independence.