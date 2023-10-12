---
title: "Lab-3: Multiple linear regression"
author: "Youssouph Cissokho"
date: "2023-10-12 02:37:59"
fontsize: 11pt 
header-inlcude:
  \usepackage{fvextra}
  \DefineVerbatimEnvironment{Highlighting}{Verbatim}{breaklines,commandchars=\\\{\}}
output:
  html_document:
    toc_depth:
    number_sections: true
  fig_caption: yes
urlcolor: blue
linkcolor: red
md_document:
    variant: markdown_strict+backtick_code_blocks+autolink_bare_uris
---

\definecolor{tcol}{HTML}{0000FF}



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

$$
y=\beta_{0}+\beta_{1} x_{1}+\beta_{2} x_{2}+\ldots+\beta_{k} x_{k}+\varepsilon
$$

where 

* $\beta_i,\;\;i=1\cdots,k$ are the regression coefficients; 
* $\varepsilon$ is the random error i.e.
$$
\epsilon \sim N(0, \sigma^2).
$$
ie they are *independent and identically distributed* (iid) normal random variables with mean `\(0\)` and variance $\sigma^2$.

`Goal` to estimate the regression coefficients    $\beta_i,\;\;i=1\cdots,k$.

## View structure of the dataset {#12}

We will use the `parenthood` datset from the textbook **Learning statistics with R** by `Danielle Navarro`. The question is to find out why Dan is so very grumpy (i.e. `bad-tempered and miserable`) the next day? Maybe, she is  not getting enough sleep. We drew some scatterplots to help us examine the relationship between the amount of sleep I get, and my grumpiness the following day

Now let's examine the `data` in details: The columns are 

* `baby.sleep`: the baby's sleep level (amount);
* `dan.grump` : danielle's grumpiness level the next day;
* `dan.sleep` : danielle's sleep level.


```r
data_parent <- read.csv("parenthood.csv")  # load the parenthood data set 
head(data_parent, n = 5)  # print the first 5 rows of the data
```

```
##   ID dan.sleep baby.sleep dan.grump day
## 1  1      7.59      10.18        56   1
## 2  2      7.91      11.66        60   2
## 3  3      5.14       7.92        82   3
## 4  4      7.71       9.61        55   4
## 5  5      6.68       9.75        67   5
```

```r
str(data_parent)  # display the internal structure of the               # elements of the dataset
```

```
## 'data.frame':	100 obs. of  5 variables:
##  $ ID        : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ dan.sleep : num  7.59 7.91 5.14 7.71 6.68 5.99 8.19 7.19 7.4 6.58 ...
##  $ baby.sleep: num  10.18 11.66 7.92 9.61 9.75 ...
##  $ dan.grump : int  56 60 82 55 67 72 53 60 60 71 ...
##  $ day       : int  1 2 3 4 5 6 7 8 9 10 ...
```

```r
dim(data_parent)  # gives the dimension 
```

```
## [1] 100   5
```

## Visualization of the dataset {#13}

The `GGally` provides a function named `ggpairs` which is the ggplot2 equivalent of the pairs function in R.

```r
# install.packages('GGally')
library(GGally)
ggpairs(data_parent, columns = 2:4)
```

<img src="/post/getting-started/MLR_files/figure-html/unnamed-chunk-1-1.png" width="672" />

# Multiple linear regression {#1}

We will do 3 different regressions:

* Using all the predictors (`baby.sleep` and `dan.sleep`);
* Using one predictor at a time (`baby.sleep` or `dan.sleep`);
* Finally, we will choose the `best model.`

##  MLR with the full model {#2}


```r
# MLR with all the predictors
Full_model = lm(dan.grump ~ dan.sleep + baby.sleep, data = data_parent)
Full_model  # print the coefficients
```

```
## 
## Call:
## lm(formula = dan.grump ~ dan.sleep + baby.sleep, data = data_parent)
## 
## Coefficients:
## (Intercept)    dan.sleep   baby.sleep  
##   125.96557     -8.95025      0.01052
```
Hence, the multiple linear regression model is

y=126 -9* **dan.sleep** + 0.011* **baby.sleep**

The coefficient associated with **dan.sleep** is quite large, suggesting that every `hour of sleep she loses makes her a lot grumpier`. However, the coefficient for **baby.sleep** is very small, suggesting that it doesn’t really matter how much sleep her son gets; not really. What matters as far as her grumpiness goes, is how much sleep she gets. 

Once the coefficients are estimated, we face two immediate questions:

1. `What is the overall adequacy of the model`? 
2. `Which specific regressors (coefficients) seem important`?

### The significance of regression coefficients {#14}


```r
# MLR with all the predictors
Full_model = lm(dan.grump ~ dan.sleep + baby.sleep, data = data_parent)
summary(Full_model)  # print the coefficients
```

```
## 
## Call:
## lm(formula = dan.grump ~ dan.sleep + baby.sleep, data = data_parent)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -11.0345  -2.2198  -0.4016   2.6775  11.7496 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 125.96557    3.04095  41.423   <2e-16 ***
## dan.sleep    -8.95025    0.55346 -16.172   <2e-16 ***
## baby.sleep    0.01052    0.27106   0.039    0.969    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.354 on 97 degrees of freedom
## Multiple R-squared:  0.8161,	Adjusted R-squared:  0.8123 
## F-statistic: 215.2 on 2 and 97 DF,  p-value: < 2.2e-16
```

The third column gives you the t-statistic (each of the form `\(t=\frac{\hat{\beta}}{se(\hat{\beta})}\)`), and it’s actual p value for each of these tests (in the fourth column):

* The coef. intercept ($\hat{\beta}_0=125.97$) and the coef. of `dan.sleep` ($\hat{\beta}_1=-8.9$) are statistically significant i.e. p value ($p<0.001$).
* However, the coef. of `baby.sleep` ($\hat{\beta}_2=0.011$) is statistically not significant.

`Conclusion`: The `baby.sleep` variable has no significant effect; all the work is being done by the `dan.sleep` variable.

### Analysis-of-variance table and test for significance of regression of the model {#15}

```r
# MLR with all the predictors
anova(Full_model)
```

```
## Analysis of Variance Table
## 
## Response: dan.grump
##            Df Sum Sq Mean Sq  F value Pr(>F)    
## dan.sleep   1 8159.9  8159.9 430.4751 <2e-16 ***
## baby.sleep  1    0.0     0.0   0.0015 0.9691    
## Residuals  97 1838.7    19.0                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
The F-test that is useful for checking the `performance of the model as a whole.`

In this case, the model performs significantly better since the F_stat is `\(215.2\)` with a p value `\(p < .001\)`. Moreover, the adjusted `\(R^2=0.812\)` indicates that the regression model accounts for `\(81.2\%\)` of the variability in the outcome measure.

## Linear Regression with "dan.sleep" predictor {#3}



```r
# SLR with dan.sleep predictors
Dan_model = lm(dan.grump ~ dan.sleep, data = data_parent)
summary(Dan_model)  # print the coefficients
```

```
## 
## Call:
## lm(formula = dan.grump ~ dan.sleep, data = data_parent)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -11.025  -2.213  -0.399   2.681  11.750 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 125.9563     3.0161   41.76   <2e-16 ***
## dan.sleep    -8.9368     0.4285  -20.85   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.332 on 98 degrees of freedom
## Multiple R-squared:  0.8161,	Adjusted R-squared:  0.8142 
## F-statistic: 434.9 on 1 and 98 DF,  p-value: < 2.2e-16
```

```r
anova(Dan_model)  # In SlR t and F tests are the same 
```

```
## Analysis of Variance Table
## 
## Response: dan.grump
##           Df Sum Sq Mean Sq F value    Pr(>F)    
## dan.sleep  1 8159.9  8159.9  434.91 < 2.2e-16 ***
## Residuals 98 1838.7    18.8                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
Hence, the simple linear regression model is

y=126 -9* **dan.sleep**

* The coef. intercept ($\hat{\beta}_0=125.97$) and the coef. of `dan.sleep` ($\hat{\beta}_1=-8.9$) are statistically significant i.e. p value ($p<0.001$).
* Moreover, the model performs significantly good, since the F_stat is `\(434.9\)` with a p value `\(p < .001\)`. Moreover, the adjusted `\(R^2=0.814\)` indicates that the regression model accounts for `\(81.4\%\)` of the variability in the outcome measure.

## Linear Regression with "baby.sleep" predictor {#4}



```r
# SLR with baby.sleep predictors
Baby_model = lm(dan.grump ~ baby.sleep, data = data_parent)
summary(Baby_model)  # print the coefficients
```

```
## 
## Call:
## lm(formula = dan.grump ~ baby.sleep, data = data_parent)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -21.4190  -5.0049  -0.0587   4.9567  23.7275 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  85.7817     3.3528  25.585  < 2e-16 ***
## baby.sleep   -2.7421     0.4035  -6.796 8.45e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 8.327 on 98 degrees of freedom
## Multiple R-squared:  0.3203,	Adjusted R-squared:  0.3134 
## F-statistic: 46.18 on 1 and 98 DF,  p-value: 8.448e-10
```

```r
anova(Baby_model)  # In SlR t and F tests are the same 
```

```
## Analysis of Variance Table
## 
## Response: dan.grump
##            Df Sum Sq Mean Sq F value    Pr(>F)    
## baby.sleep  1 3202.7  3202.7  46.184 8.448e-10 ***
## Residuals  98 6795.9    69.3                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
Hence, the simple linear regression model is

y=126 0.011* **baby.sleep**

* The coef. intercept ($\hat{\beta}_0=125.97$) and the coef. of `baby.sleep` ($\hat{\beta}_2=-2.74$) are statistically significant i.e. p value ($p<0.001$).
* Moreover, the **model performs significantly good**, since the F_stat is `\(46.2\)` with a p value `\(p < .001\)`. Moreover, the adjusted `\(R^2=0.313\)` indicates that the regression model accounts for `\(31.3\%\)` of the variability in the outcome measure.

# Model accuracy assessment and Model selection {#5}
One fairly major problem that remains is the problem of`model selection`. That is, if we have a data set that contains several variables, which ones should we include as predictors, and which ones should we not include? In other words, we have a problem of `variable selection`. Firstly, the two principles:

* Pick out a smallish number of possible predictors that are of theoretical interest;
* there is a trade off between simplicity and goodness of fit. You need to avoid throwing in too many variables.

Remember `Ockham’s razor` principle
>do not multiply entities beyond necessity. ie don’t >chuck in a bunch of largely irrelevant predictors just >to boost your

## Based on adjusted `\(R^2\)`, RSE and Mean$_{se}$ {#6}

The model accuracy can be assessed by examining the `adjusted R-squared` `\(R^2\)`, `Mean squared error` and `Residual Standard Error (RSE)`. 

We will summarize these values in this table.

| |Full model | Model 1 (with ban.sleep)| Model 2 (with baby.sleep)
|:---------|:------------:|:------------:|:------------:
|	`\(R^2_{adj}\)`| **81.2**$\%$| **81.4**$\%$| `\(31.3\%\)`
| RSE |	**4.354**| **4.332**| 8.327
| Mean$_{se}$ |**8159.9** |**8159.9**|3202.7 

**Note** a small Mean$_{se}$ is good.

Full model and model 1 (with `dan.sleep` as predictor) seem to have the same performance (higther `\(R^2_{adj}\)` and lower `MSE`) and better than model 2 (with `baby.sleep` as a predictor). Now which one to choose? 
**IMPORTANT**, `Ockham’s razor` principle: the Simpler the better. Therefore, I assume Model 2 is simpler than Model 1 full model). That is 

y=126 -9* **dan.sleep**

## Based on the Akaike information criterion (AIC; Akaike, 1974) {#7}

The AIC for a model that has `K predictors` plus an intercept is:
$$
\mathrm{AIC}=\frac{\mathrm{SS}_{r e s}}{\hat{\sigma}^{2}}+2 K
$$
The `smaller the AIC value, the better the model performance is`

```r
AIC(Full_model, Dan_model)
```

```
##            df      AIC
## Full_model  4 582.9513
## Dan_model   3 580.9528
```
Since the AIC of Model 1 (`dan.model`) is the lowest, then it's the best.

## based on Anova {#8}

tests the hypothesis that 

* `\(H_0\)`: model 1 (`dan_model`), selected predictors.
* `\(H_1\)`: full model ( all the predictors)

```r
anova(Dan_model, Full_model)
```

```
## Analysis of Variance Table
## 
## Model 1: dan.grump ~ dan.sleep
## Model 2: dan.grump ~ dan.sleep + baby.sleep
##   Res.Df    RSS Df Sum of Sq      F Pr(>F)
## 1     98 1838.7                           
## 2     97 1838.7  1  0.028576 0.0015 0.9691
```
Since the p value `\(p=0.969 > 0.05\)`, hence, we fail to reject the null hypthesis i.e. `Model 1 is the winner`.

`NOTE:` This approach to regression, in which we add `all of our covariates` into a **null model**, and then add the `variables of interest` into an **alternative model**, and then compare the two models in hypothesis testing framework, is often referred to as `hierarchical regression`.

# Confidence intervals for the parameters {#9}

Recall that the `\(100(l − \alpha)\)` percent confidence interval for the regression coefficient `\(\beta_j,\;j = 0, 1, \cdots ,k\)`, as
$$
\hat{\beta}_{j}-t_{\alpha / 2, n-p} \sqrt{\hat{\sigma}^{2} C_{j j}} \leq \beta_{j} \leq \hat{\beta}_{j}+t_{\alpha / 2, n-p} \sqrt{\hat{\sigma}^{2} C_{i j}}
$$
where,

`\(C_{jj}\)` is the `\(j^{th}\)` diagonal element of the `\((X'X)^{−1}\)` matrix. In R, a `\(95\%\)` CIs for the parameters are obtained using **confint()**.


```r
confint(Full_model, level = 0.95)
```

```
##                  2.5 %      97.5 %
## (Intercept) 119.930125 132.0010063
## dan.sleep   -10.048710  -7.8517895
## baby.sleep   -0.527462   0.5485109
```

```r
confint(Dan_model, level = 0.95)
```

```
##                  2.5 %    97.5 %
## (Intercept) 119.971000 131.94158
## dan.sleep    -9.787161  -8.08635
```
# 95% CI on the mean when danielle's sleep level is at 9.2 and 10.33 {#10}

Recall that, the `\(100(l-\alpha)\)` percent confidence interval on the mean response at the point `\(\textbf{x}_0=(x_{01},x_{02},\cdots,x_{0k})'\)` is

$$
\hat{y}_{0}-t_{\alpha / 2, n-p} \sqrt{\hat{\sigma}^{2} \mathbf{x}_{0}^{\prime}\left(\mathbf{X}^{\prime} \mathbf{X}\right)^{-1} \mathbf{x}_{0}} \leq E\left(y \mid x_{0}\right) \leq \hat{y}_{0}+t_{\alpha / 2, n-p} \sqrt{\hat{\sigma}^{2} \mathbf{x}_{0}^{\prime}\left(\mathbf{X}^{\prime} \mathbf{X}\right)^{-1} \mathbf{x}_{0}}
$$
where,

* `\(\textbf{x}_0=(x_{01},x_{02},\cdots,x_{0k})\)` are feature values;
* `\((X'X)^{−1}\)` is the covariate matrix. In R, we use `predict` function with `interval="confidence:"`


```r
new <- data.frame(dan.sleep = c(9.2, 10.33))  # create a new data

# this provides a 95% confidence interval on the mean
# response
predict(Dan_model, new, interval = "confidence")
```

```
##        fit      lwr      upr
## 1 43.73814 41.65230 45.82398
## 2 33.63960 30.65184 36.62737
```
Hence, 

* the CI of the mean response for `dan.sleep=9.2` is `\(41.74\leq E\left(y \mid x_{0}\right) \leq 45.82\)`;
* the CI of the mean response for `dan.sleep=10.33` is `\(30.64\leq E\left(y \mid x_{0}\right) \leq 36.63\)`.


# Summary

In this presentation, we discussed about 

* [Multiple linear regression](#1)
    a. [Multiple LR with full model](#2)
    c. [LR with one predictor](#3) and [here](#4)
* [Model selection](#5)
    a. [Based on adjusted `\(R^2\)`, RSE and Mean$_{se}$](#6)
    b. [Based on AIC](#7)
    c. [Based on Anova](#8)
* [Confidence intervals for the parameters](#9)
* [CI on the mean response](#10)
  

# Problem to do (p3.8 page 123)

The data in Table B.5 present the performance of a chemical process as a function of sever controllable process variables.

a. Fit a multiple regression model relating `\(CO_2\)` product (y) to total solvent ($x_6$) and hydrogen consumption ($x_7$)
b. Test for significance of regression. Calculate `\(R^2\)` and `\(R^2_{Adj}\)`.
c. Using t tests determine the contribution of `\(x_6\)` and `\(x_7\)` to the model.
d. Construct `\(95\%\)` CIs on `\(\beta_6\)` and `\(\beta_7\)`.
e. Refit the model using only `\(x_6\)` as the regressor. Test for significance of regression and calculate `\(R^2\)` and `\(R^2_{Adj}\)`. Discuss your findings. Based on these statistics, are you satisfied with this model?
f. Construct a `\(95\%\)` CI on `\(\beta_6\)` using the model you fit in part e. Compare the length of this CI to the length of the CI in part d. Does this tell you anything important about the contribution of `\(x_7\)` to the model?
g. Compare the 2 models

