---
layout: post
title: "Machine Learning Basic 3: Probabilistic ML"
excerpt: "maximum likelihood, naive bayes"
categories: knownotes
tags: [ML]
comments: true
share: true
author: chungyu
---
> Mainly from the lecture notes of [ Practical Data Analysis](http://www.datasciencecourse.org/)

# Transform the problem

The basic question: given some data \\( x^{(1)}, ..., x^{(m)} \\)  , how do I find a distribution that captures this data “well”?
  * In general (if we can pick from the space of all distributions), this is a hard question, but if we pick from a **particular parameterized family of distributions** \\( p(X; \theta) \\) , the question is (at least a little bit) easier
Question becomes: how do I find parameters \\( \theta \\) of this distribution that fit the data?

---

# Maximum likelihood estimation
> Basic idea of maximum likelihood estimation (MLE): find the parameters that maximize the probability of the observed data

![mle1]({{site.url}}/images/ml/mle1.png)
![mle2]({{site.url}}/images/ml/mle2.png)

---

#### Example: MLE for Bernoulli
![bernoulli1]({{site.url}}/images/ml/bernoulli1.png)
![bernoulli2]({{site.url}}/images/ml/bernoulli2.png)

#### So the same logic applied for other distribution:
* (1) Derive the log likelihood form \\( l(\theta) \\)
* (2) Making partial derivatives for each parameter \\( \theta_i \\) and make it to zero, which will give you the equation of getting the MLE for each parameter!
![gaussianmle]({{site.url}}/images/ml/gaussianmle.png)


# Naive Bayes modeling

Basic idea is that we model
* input as random variables: \\( X = (X_1, X_2, ..., X_n) \\)
  * each \\( X_i \\) might be Bernoulli, categorical, or Gaussian random variables
* output as random variables: \\( Y \\)
  * could be a Bernoulli, a categorical, or a Gaussian random variables

Then the goal is find \\( P(Y\|X) \\). How? Bayes’ rule
![bayes]({{site.url}}/images/ml/bayes.png)
![bayes2]({{site.url}}/images/ml/bayes2.png)

Then we can make predictions
![bayes3]({{site.url}}/images/ml/bayes3.png)

Pitfalls
![bayesissue]({{site.url}}/images/ml/bayesissue.png)
