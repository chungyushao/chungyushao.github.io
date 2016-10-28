---
layout: post
title: "Pandas basics"
excerpt: "Python Pandas"
categories: knownotes
tags: [python, pandas]
modified: 2016-10-24
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
* The axis labels are collectively referred to as the index.
* Series acts very similarly to a ndarray, and is a valid argument to most NumPy functions.
  * Series can be also be passed into most NumPy methods expecting an ndarray.
  * A key difference between Series and ndarray is that operations between Series automatically align the data based on label.

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

* Series is dict-like
  * A Series is like a fixed-size dict in that you can get and set values by index label.

##### DataFrame
* DataFrame is a 2-dimensional labeled data structure with **columns** of potentially different types.
* You can think of it like a spreadsheet or SQL table, or a dict of Series objects.


# Creation
##### Series
`s = pd.Series(data, index=index)`
  * `data` can be a Python dict, an ndarray or a scalar value.
  * `index` is a list of axis labels.

* From dict
  * If `data` is a dict, if `index` is passed the values in data corresponding to the labels in the index will be pulled out. Otherwise, an index will be constructed from the **sorted keys** of the dict, if possible.  

```python
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
  * If `data` is a scalar value, an index must be provided. The value will be repeated to match the length of index

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

##### DataFrame

DataFrame accepts many different kinds of input:
  * Dict of 1D ndarrays, lists, dicts, or Series
  * 2-D numpy.ndarray
  * Structured or record ndarray
  * A Series
  * Another DataFrame

* From dict of Series or dicts

```python
In [32]: d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
   ....:      'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
   ....:

In [33]: df = pd.DataFrame(d)

In [34]: df
Out[34]:
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0
```
* From dict of ndarrays / lists

```python
In [39]: d = {'one' : [1., 2., 3., 4.],
   ....:      'two' : [4., 3., 2., 1.]}
   ....:

In [40]: pd.DataFrame(d)
Out[40]:
   one  two
0  1.0  4.0
1  2.0  3.0
2  3.0  2.0
3  4.0  1.0

In [41]: pd.DataFrame(d, index=['a', 'b', 'c', 'd'])
Out[41]:
   one  two
a  1.0  4.0
b  2.0  3.0
c  3.0  2.0
d  4.0  1.0
```

# Attribute

```python
In [32]: d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
   ....:      'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
   ....:

In [37]: df.index
Out[37]: Index([u'a', u'b', u'c', u'd'], dtype='object')

In [38]: df.columns
Out[38]: Index([u'one', u'two'], dtype='object')
```
* From structured or record array

```python

```


# Access

```python
In [32]: d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
   ....:      'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
   ....:

In [35]: pd.DataFrame(d, index=['d', 'b', 'a'])
Out[35]:
   one  two
d  NaN  4.0
b  2.0  2.0
a  1.0  1.0

In [36]: pd.DataFrame(d, index=['d', 'b', 'a'], columns=['two', 'three'])
Out[36]:
   two three
d  4.0   NaN
b  2.0   NaN
a  1.0   NaN
```
