---
layout: post
title: 'Mechine Learning notes (1)'
subtitle: 'Andrew Ng Mechine Learning notes'
date: 2020-01-06
categories: 机器学习 深度学习
cover: '../assets/img/v2-7ae16ac45777844149cee5e11a49c414_1200x500.jpg'
tags: 机器学习 深度学习
---

>这是学习吴恩达《机器学习》的相关笔记
>
>相关内容：[深度学习计划](https://loner1024.top/深度学习计划.html)

# What is machine learning

There isn't a well accepted definition of what is and what isn't machine learning.

* Arthur Samuel (1959). Machine Learning: Filed of study that gives computers the ability to learn without being explicitly programmed. （在没有明确设置的情况下使计算机具有学习能力的研究领域）
* TomMitchell (1998) Well-posedLearning Problem:  A computer program is said to learn from experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E. （一个适当的学习问题定义如下，计算机程序从经验E中学习，解决某一任务T，进行某一性能度量P，通过P测定在T上的表现因E而提高）  

**Suppose your email program watches which emails you do or do not mark as spam, and based on that learns how to better filter spam. What is the task T in this setting?**

 **A. Classifying emails as spam or not spam.** ✅️ T

B. Watching you label emails as spam or not spam. E

C.  The number (or fraction) of emails correctly classified as spam/not spam. P

D. None of the above—this is not a machine learning problem

# Supervised Leaening

**"right answers" given** -**->** **more right answers**

**Regression: Predict continuous valued output**

**Classification: Prodict a discrete valued output**

Problem 1: You have a large inventory of identical items. You want to predict how many of these items will sell over the next 3 months.

Problem 2: You’d like software to examine individual customer accounts, and for each account decide if it has been hacked/compromised.

Problem 1 is a regression problem, Problem 2 is a Classification problem.

# Unspervised Learning

The unspervised learning algorithm may break these data into these two separate clusters. This is called clustring algorithm.（聚类算法）

**Cooktail part problem algorithm**
$$
[W,s,v] = svd((repmat(sum(x.*x,1),size(x,1),1).*x)*x');
$$

# Problem environment

**Octave**, open source software, many learning algotithms become just a few lines of code to implement. 

**Of the following examples, which would you address using an unsupervised learning algorithm? (Check all that apply.)**

Given email labeled as spam/not spam, learn a spam filter.

Given a set of news articles found on the web, group them into set of articles about the same story. ✅️

Given a database of customer data, automatically discover market segments and group customers into different market segments. ✅️

Given a dataset of patients diagnosed as either having diabetes or not, learn to classify new patients as having diabetes or not.