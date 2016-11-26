---
layout: post
title: "Pandas DataFrame Groupby"
excerpt: "split apply combine"
categories: knownotes
tags: [python, pandas]
comments: true
share: true
author: chungyu
---

> mainly from [Group By: split-apply-combine](http://pandas.pydata.org/pandas-docs/stable/groupby.html)


# Concept

###### Groupby
`groupby` often involving one or more of the following steps
* Splitting the data into groups based on some criteria
* Applying a function to each group independently
* Combining the results into a data structure

###### Apply
In the `apply` step, we might wish to one of the following:
* Aggregation
  * Computing a summary statistic (or statistics) about each group.
* Transformation
  * Perform some group-specific computations and return a like-indexed.
* Filtration
  * Discard some groups, according to a group-wise computation that evaluates True or False.
* Some combination of the above  



# Examples

##### Extract rows with certain columns being unique

```python

```
