---
layout: post
title: "Machine Learning Basic 2: continue"
excerpt: "overfitting, cross-validation, regularization, and evaluation"
categories: knownotes
tags: [ML]
comments: true
share: true
author: chungyu
---
> Mainly from the lecture notes of [ Practical Data Analysis](http://www.datasciencecourse.org/)


# Overfitting

* We don’t really care about minimizing this objective on the given data set, What we really care about is how well our function will generalize to new examples that we didn’t use to train the system.
* The higher degree polynomials exhibited **overfitting**: they actually have very low loss on the training data, but create functions we do**n’t expect to generalize** well

![overfitting]({{site.url}}/images/ml/overfitting.png)

# Cross-validation

* Although it is difficult to quantify the true generalization error, we can approximate it by holdout cross-validation
* Basic idea of cross-validation: use training set to determine the parameters, use holdout set to determine the hyperparameters

###### hyperparameters
> * We refer to the \\( \theta \\) variables as the parameters of the machine learning algorithm
> * Other quantities that also affect the classifier: degree of polynomial, amount of regularization, etc; these are collectively referred to as the **hyperparameters** of the algorithm


# Regularization
* degree of the polynomial acts as a natural measure of the “complexity” of the model, higher degree polynomials are more complex
* But fitting these models also requires extremely large coefficients on these polynomials
* An alternative way to control model complexity: keep the weights small (regularization)
![regularization]({{site.url}}/images/ml/regularization.png)
* The formulation trades off loss on the training set with a penalty on high values of the parameters
* By varying \\( \lambda \\) from zero (no regularization) to infinity (infinite regularization, meaning parameters will all be zero), we can sweep out different sets of model complexity


# Evaluation

* Even though we used a training/holdout split to fit the parameters, we are still effectively fitting the hyperparameters to the holdout set
  * Imagine an algorithm that ignores the training set and makes random predictions; given a large enough hyperparameter search (e.g., over random seed), we could get perfect holdout performance
![evaluation]({{site.url}}/images/ml/evaluation.png)

The best solutions: evaluate your system “in the wild” (where it will see truly novel examples) as often a possible; recollect data if you suspect overfitting to test set; look at test set performance sparingly
