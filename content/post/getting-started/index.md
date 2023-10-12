---
authors:
- admin
- 吳恩達
categories:
- Demo
- 教程
date: "2023-10-08T00:00:00Z"
draft: false
featured: false
image:
  caption: 'Image credit: [**Google**](https://www.google.com/search?q=image+multiple+regression&sca_esv=571674645&rlz=1C5CHFA_enCA920CA920&tbm=isch&sxsrf=AM9HkKkm4zX1pqP9gfRm6MYzD4fcMIWzug:1696746025667&source=lnms&sa=X&ved=2ahUKEwiX9ojB5-WBAxWyg4kEHQdYARUQ_AUoAXoECAMQAw&biw=1440&bih=815&dpr=1#imgrc=JumTr2l7yWQUWM&imgdii=YTnpx3FklWFt-M)'
  focal_point: ""
  placement: 2
  preview_only: false
lastmod: "2020-12-13T00:00:00Z"
projects: 
subtitle: "Welcome \U0001F44B!!  A regression model that involves more than `one regressor variable` is called a multiple regression model. Fitting and analyzing these models is discussed in this lab. The results are extensions of those in `lab2` for simple linear regression"
summary: "A regression model that involves more than `one regressor variable` is called a multiple regression model. Fitting and analyzing these models is discussed in this lab. The results are extensions of those in `lab2` for simple linear regression"
tags:
- Academic
title: Multiple linear regression
---


`Table of content`

In this presentation, we will do the following 

* [Preliminaries](#11);
    a. [View structure of the dataset](#12);
    b. [Visualization of the dataset](#13);
* [Multiple linear regression](#1);
    a. [Multiple LR with full model](#2);
        i. [Significance of regression coefficients](#14)
        ii. [Significance of the model](#15)
    b. [LR with one predictor](#3) and [here](#4)
* [Model selection](#5)
    a. [Based on adjusted `\(R^2\)`, RSE and Mean$_{se}$](#6)
    b. [Based on AIC](#7)
    c. [Based on Anova](#8)
* [Confidence intervals for the parameters](#9)
* [CI on the mean response](#10)


<br>
<br>
<br>
<br>
<br>
<br>

# Preliminaries {#11}
A regression model that involves more than `one regressor variable` is called a multiple regression model. Fitting and analyzing these models is discussed in this lab. The results are extensions of those in `lab2` for simple linear regression.

The `multiple linear regression model with k` regressors (predictors) is given by 

`$$
y=\beta_{0}+\beta_{1} x_{1}+\beta_{2} x_{2}+\ldots+\beta_{k} x_{k}+\varepsilon
$$`

where 

* `\(\beta_i,\;\;i=1\cdots,k\)` are the regression coefficients; 
* `\(\varepsilon\)` is the random error i.e.
$$
\epsilon \sim N(0, \sigma^2).
$$
ie they are *independent and identically distributed* (iid) normal random variables with mean `\(0\)` and variance `\(\sigma^2\)`.

`Goal` to estimate the regression coefficients    `\(\beta_i,\;\;i=1\cdots,k\)`.
