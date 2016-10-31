---
layout: post
title: "Pandas Series"
excerpt: "Series"
categories: knownotes
tags: [python, pandas]
comments: true
share: true
author: chungyu
---

> mainly from
> * [Intro to Data Structures](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro)

# Creation

`s = pd.Series(data, index=index)`
  * `data` can be a (1) Python dict, (2) an `ndarray` or (3) a scalar value.
  * `index` is a list of axis labels.

* From dict
  * If `data` is a dict, if `index` is passed the values in data corresponding to the labels in the index will be pulled out. Otherwise, an index will be constructed from the **sorted keys** of the dict, if possible.  

```python
# Python dict
In [7]: d = {'a' : 0., 'b' : 1., 'c' : 2.}

In [8]: pd.Series(d)
Out[8]:
a    0.0
b    1.0
c    2.0
dtype: float64

In [9]: pd.Series(d, index=['b', 'c', 'd', 'a'])
Out[9]:
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64
```  

* From scalar value
  * If `data` is a scalar value, **an index must be provided.** The value will be repeated to match the length of index

```python
In [10]: pd.Series(5., index=['a', 'b', 'c', 'd', 'e'])
Out[10]:
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64
```

* From `rename`
  * `s` and `s2` refer to different objects.

```python
s = pd.Series(np.random.randn(5), name='something')
s2 = s.rename("different")
```


# Histogramming
`.value_counts()`
```python
s = pd.Series(np.random.randint(0, 7, size=10))
# 0    4
# 1    2
# 2    1
# 3    2
# 4    6
# 5    4
# 6    4
# 7    6
# 8    4
# 9    4
# dtype: int64

s.value_counts()
# 4    5
# 6    2
# 2    2
# 1    1
# dtype: int64
```
