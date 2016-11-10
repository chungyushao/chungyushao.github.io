---
layout: post
title: "Sampling Distribution"
excerpt: "sampling distribution"
categories: knownotes
tags: [statistics]
comments: true
share: true
author: chungyu
---
> Mainly from the lecture notes of [ JB Statistics](http://www.jbstatistics.com/sampling-distributions/)

Statistical inference techniques are based on the concept of the sampling distribution of a statistic
  * The sampling distribution of a statistic is the **probability distribution** of that statistic.
  * Sampling distribution is the distribution of the statistic if we were to repeatedly draw samples from the population
  * If we get a sample (a value of the statistic), and draw different sample with the same sample size, and get the value of this statistic, this statistic will be vary from sample to sample, according to the sample distribution of that statistics.
    * In repeated sampling, the value of the sample mean would vary from sample to sample.

# example
> Suppose a class with 16 students, and someone wants to know the average age of the students.
![16true]({{site.url}}/images/stat/16true.png)
###### You don't know the exact 16 as the figure above
![16sample3]({{site.url}}/images/stat/16sample3.png)
###### You can only sample three data a time
![sample3stat]({{site.url}}/images/stat/sample3stat.png)
###### Every time you sample with 3 values, you can make a mean of these three values. Supposed you do this for 1 M times, you can draw a diagram for the "statistic of the sample mean."

---

# The Sampling Distribution of the Sample Mean
![samplemean1]({{site.url}}/images/stat/samplemean1.png)
###### Mathematically can be proved that
![samplemean2]({{site.url}}/images/stat/samplemean2.png)
###### AND
![samplemean3]({{site.url}}/images/stat/samplemean3.png)
![samplemean4]({{site.url}}/images/stat/samplemean4.png)
###### Scandalized to Z so that we can easily get the numeric value
![samplemean5]({{site.url}}/images/stat/samplemean5.png)
###### Example
![samplemean6]({{site.url}}/images/stat/samplemean6.png)

---
# Central Limit Theorem
![clt1]({{site.url}}/images/stat/clt1.png)
###### Regardless of the original distribution is the point!!
![clt2]({{site.url}}/images/stat/clt2.png)
![clt9]({{site.url}}/images/stat/clt9.png)
###### Example 1
![clt3]({{site.url}}/images/stat/clt3.png)
###### Supposed we only draw 2 samples a time, and calculate the 2-samples mean's histogram
![clt4]({{site.url}}/images/stat/clt4.png)
###### When we draw 50 samples a time, it's much different
![clt5]({{site.url}}/images/stat/clt5.png)
###### Conclusion 1
![clt6]({{site.url}}/images/stat/clt6.png)
###### Conclusion 2
![clt7]({{site.url}}/images/stat/clt7.png)
###### Conclusion 3
![clt8]({{site.url}}/images/stat/clt8.png)
###### Example 2
![clt10]({{site.url}}/images/stat/clt10.png)
