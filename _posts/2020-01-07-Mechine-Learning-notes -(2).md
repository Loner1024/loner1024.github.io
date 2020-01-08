---
layout: post
title: 'Mechine Learning notes (2)'
subtitle: 'Linear regression with one variable'
date: 2020-01-07
categories: 机器学习 深度学习
cover: '../assets/img/v2-7ae16ac45777844149cee5e11a49c414_1200x500.jpg'
tags: 机器学习 深度学习
---

>这是学习吴恩达《机器学习》的相关笔记
>
>相关内容：[深度学习计划](https://loner1024.top/深度学习计划.html)

# Model representation

**Supervised Learning**: Give the "right aswer" for each example in the data.

**Predict Problem**: Predict real-valued-value output. 

**Training set of housing prices (Portland, OR)**:

| **Size in feet^2** **(**x**)** | **Price ($) in 1000's (**y**)** |
| ------------------------------ | ------------------------------- |
| 2104                           | 460                             |
| 1416                           | 232                             |
| 1534                           | 315                             |
| 852                            | 178                             |
| …                              | …                               |

Notation:
**m** = Number of training examples

**x**’s = “input” variable / features

**y**’s = “output” variable / “target” variable

In this data set , m = 47

![截屏2020-01-07下午7.46.19](../assets/img/截屏2020-01-07下午7.46.19.png)

**h：**hypothesis，假设函数，代价函数

**How do we represent h **

![截屏2020-01-07下午7.50.00](../assets/img/截屏2020-01-07下午7.50.00.png)

**Linear regression with one variable. Univariate （单变量） linear regression.**（一元线性回归）

# Cost function



![截屏2020-01-07下午7.52.38](../assets/img/截屏2020-01-07下午7.52.38.png)

![截屏2020-01-07下午7.59.03](../assets/img/截屏2020-01-07下午7.59.03.png)

$minimize \frac{1}{2m}\sum^{m}_{i=1}(h_Θ(x^i)-y^i)^2$

*among them:* 

$h_Θ(x^i)=Θ_0+Θ_{x^{(i)}}$

*and then:*

$J(Θ_0,Θ_1)=\frac{1}{2m}\sum^{m}_{i=1}(h_Θ(x^i)-y^i)^2$

*whitch is:*

$minimize J(Θ_0,Θ_1)$

This function is **cost function**（代价函数）. Also called **squared error function**（平方误差函数）.

# Cost function intution

Using this sipmlified definition of hypothesizing cost function, let's try to understand the cost function concept better.

![截屏2020-01-07下午8.44.15](../../Library/Application Support/typora-user-images/截屏2020-01-07下午8.44.30.png)

For each value of Θ , we wound up with a different value of function J, we could use his to trace out this plot on the right. 

![截屏2020-01-07下午8.44.55](../assets/img/截屏2020-01-07下午8.44.55.png)

The optimization objective for our learning algorithm is we want choose the value of Θ that minimizes  J .

Back the origin function

![截屏2020-01-07下午8.50.57](../assets/img/截屏2020-01-07下午8.50.57.png)

When use two parameters, the plot maybe:

![截屏2020-01-07下午9.07.23](../assets/img/截屏2020-01-07下午9.07.23.png)

**bowl function**

# Gradient descent

（梯度下降）

Have some function $J(Θ_0,Θ_1)$

Want $min J(Θ_0,Θ_1)$

**Outline**：

* Start with some $Θ_0,Θ_1$
* Keep changing  $Θ_0,Θ_1$ to reduce $J(Θ_0,Θ_1)$ until we hopegully end up at a minimum

## Gradient descent algorithm

![截屏2020-01-07下午10.21.43](../assets/img/截屏2020-01-07下午10.21.43.png)

**:=** Denote assignment

**α** learning rate（学习率）, it basically controls how big a step we take downhill with gradient descent.

# Gradient descent intuition

**About derivative**

![截屏2020-01-07下午11.05.45](../assets/img/截屏2020-01-07下午11.05.45.png)

**About α**

If α is too small, gradient descent can be slow.

If α is too large, gradient descent can overshoot the minimum. It may fail to converge, or even diverge.



If $Θ_1$ at a local optimum or the local minimum, the derivative would be equal to 0, in gradient descent update, $Θ_1$ unchanged.

Gradient descent can converge to a local minimum, even with the learning rate α fixed.

As we approach a local minimum, gradient descent will automatically take smaller steps. So, no need to decrease α over time. 



![image-20200107230842471](../assets/img/image-20200107230842471.png)

妙啊！通过导数来控制步长，逐渐收敛。

# Gradient descent for linear regression
**Gradient descent algorithm**

![image-20200108191130290](../assets/img/image-20200108191130290.png)



cost function for liner regression is always going to be a convex function（凸函数）.

**“Batch” Gradient Descent**

“Batch”: Each step of gradient descent uses all the training examples.