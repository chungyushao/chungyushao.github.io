---
layout: post
title: "Machine Learning Basic 1: from linear regression to learning, classification"
excerpt: "linear regression, learning, gradient, gradient descent, classification"
categories: knownotes
tags: [ML]
comments: true
share: true
author: chungyu
---
> Mainly from the lecture notes of [ Practical Data Analysis](http://www.datasciencecourse.org/)


# Linear Regression Logic

## 1. Suppose that the peak demand approximately **fits a linear model**
![Model Assumption]({{site.url}}/images/ml/model-assumption.png)

## 2. Define the objective function so that we can decide the good/bad of assumed parameters in the model
> One possibility: minimize some difference between this line and the observed data, e.g. squared loss

![Objective function]({{site.url}}/images/ml/objectivefnc.png)

## 3. Use derivatives to find the value of parameters that will give best result of objective function
> We want to minimize the squared loss

![derivatives1]({{site.url}}/images/ml/derivatives1.png)
![derivatives2]({{site.url}}/images/ml/derivatives2.png)

## 4. Use retrieved parameters to predict values
![prediction]({{site.url}}/images/ml/predict.png)

# From Linear Regression to "Learning"
> in many domains, it is difficult to hand-build a predictive model, but easy to collect lots of data;
> machine learning provides a way to **automatically infer the predictive model** from data

![prediction]({{site.url}}/images/ml/supervisedlearning.png)

## Virtually every machine learning algorithm has this form, just specify

1. **What is the hypothesis function?**
2. **What is the loss function?**
3. **How do we solve the optimization problem?**

![ml1]({{site.url}}/images/ml/ml1.png)
![ml2]({{site.url}}/images/ml/ml2.png)


> The 3 attributes of a ML algorithm above and examples
![mlexample]({{site.url}}/images/ml/mlexample.png)



# Gradient

> The condition for finding a minimum of a scalar-valued function , that the **derivative must be equal to zero** (actually just true for functions with a single local minimum), can be generalized to functions of vectors

## `Gradient`: the multi-variate analog of the **derivative**
![gradient]({{site.url}}/images/ml/gradient.png)

## Minimizing a multi-variate function just involves finding a point where the entire gradient is zero
![gradient2]({{site.url}}/images/ml/gradient2.png)

### Example
Supposed the objective function is the least squares
![leastsquare]({{site.url}}/images/ml/leastsquare.png)

## Gradient descent algorithm
> The gradient provides more than a condition for optimality, it also gives the direction of “steepest increase” for the function: Provides an intuitive approach to **minimizing** objective function: take steps in the direction of the **negative gradient**

![gradient descent algorithm]({{site.url}}/images/ml/gradientdescent.png)


# Classification

Regression tasks: predicting **real-valued** quantity
Classification tasks: predicting **discrete-valued** quantity
![classification]({{site.url}}/images/ml/classification.png)

if it's mapping to discrete values, how will the loss/objective function be defined?
![classifierlossfnc]({{site.url}}/images/ml/classifierlossfnc.png)

From the functions above, you can see that there is not an analytical solution to the zero gradient condition for most classification losses. ==> Instead, we solve these optimization problems using **gradient descent**
![classfiergradientdescent]({{site.url}}/images/ml/classfiergradientdescent.png)


# Classification metrics

A confusion matrix explicitly lists the number of examples for each actual class and each prediction
![confusioinmatrix]({{site.url}}/images/ml/confusioinmatrix.png)
![metrics]({{site.url}}/images/ml/metrics.png)

##### ROC Curve
> Receiver operating characteristic curve

![roccurve]({{site.url}}/images/ml/roccurve.png)

#### Precision recall curves
![prcurve]({{site.url}}/images/ml/prcurve.png)
