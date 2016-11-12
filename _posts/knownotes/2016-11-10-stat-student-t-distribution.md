---
layout: post
title: "Student's T Distribution"
excerpt: "t distribution"
categories: knownotes
tags: [statistics]
comments: true
share: true
author: chungyu
---
> Mainly from the lecture notes of
* [JB Statistics](http://www.jbstatistics.com/)


# Concept
![tdist1]({{site.url}}/images/stat/tdist1.png)
![tdist2]({{site.url}}/images/stat/tdist2.png)
![tdist3]({{site.url}}/images/stat/tdist3.png)
![tdist4]({{site.url}}/images/stat/tdist4.png)

* The population standard deviation \\( \sigma \\) (regardless known or unknown) should be a **constant**
* Whereas sample standard deviation \\( S \\) is a **statistics**, it's varied from samples to samples


# Distribution
![tdist5]({{site.url}}/images/stat/tdist5.png)
![tdist6]({{site.url}}/images/stat/tdist6.png)


# Application 1: calculating confidence interval
![tdist9]({{site.url}}/images/stat/tdist9.png)
* If the population standard deviation \\( \sigma \\), we can get the constant 1.96 from the normal distribution.
* However, usually population standard deviation is unknown. We can apply the same logic on T-Distribution.
* The constant will be varied with the degree of freedom.
![tdist10]({{site.url}}/images/stat/tdist10.png)

# Application 2: T-testing
* See the specific article.

---

# Formally
![tdist7]({{site.url}}/images/stat/tdist7.png)
![tdist8]({{site.url}}/images/stat/tdist8.png)
