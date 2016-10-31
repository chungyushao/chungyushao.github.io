---
layout: post
title: "Pandas basics"
excerpt: "Python Pandas"
categories: knownotes
tags: [python, pandas]
comments: true
share: true
author: chungyu
---

> mainly from
> * [Intro to Data Structures](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro)


* Data alignment is intrinsic.
  * The link between labels and data will not be broken unless done so explicitly by you.
  * Series slicing also slice the index.
* `NaN` is the standard missing data marker used in pandas

# Data structure
##### Series
* Series is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.).
* Series is dict-like
  * A Series is like a fixed-size dict in that you can get and set values by index label.
* The axis labels are collectively referred to as the index.
* Series acts very similarly to a `ndarray`, and **is a valid argument to most NumPy functions.**
  * Series can be also be passed into most NumPy methods expecting an ndarray.
  * A key difference between Series and `ndarray` is that **operations between Series automatically align the data based on label.**

```python
In [35]: s = pd.Series(5., index=['a', 'b', 'c', 'd', 'e'])
In [37]: s[1:] + s[:-1]
Out[37]:
a     NaN
b    10.0
c    10.0
d    10.0
e     NaN
dtype: float64

```

##### DataFrame
* DataFrame is a 2-dimensional labeled data structure with **columns of potentially different types.**
* You can think of it like a spreadsheet or SQL table, or **a dict of Series objects.**
