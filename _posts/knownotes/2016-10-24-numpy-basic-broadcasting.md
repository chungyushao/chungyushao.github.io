---
layout: post
title: "Numpy broadcasting"
excerpt: "numpy broadcasting"
categories: knownotes
tags: [python, numpy]
comments: true
share: true
author: chungyu
---

> Mainly from [Array Broadcasting in numpy](http://scipy.github.io/old-wiki/pages/EricsBroadcastingDoc)
[Broadcasting](https://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)

# Broadcasting
> * The term broadcasting describes how numpy treats **arrays with different shapes during arithmetic operations.**
> * the smaller array is "broadcast" across the larger array so that they have compatible shapes.


# Advantage?
* Broadcasting provides a means of vectorizing array operations so that looping occurs in C instead of Python.
* It does this without making needless copies of data and usually leads to efficient algorithm implementations.

# The Broadcasting Rule
> numpy operations are usually done element-by-element which requires two arrays to have exactly the same shape; numpy's broadcasting rule relaxes this constraint when the arrays' shapes meet certain constraints.

**In order to broadcast, the size of the trailing axes for both arrays in an operation must either be the same size or one of them must be one.**
*  The size of the result array created by broadcast operations is the **maximum size along each dimension from the input arrays**.

```python
Image  (3d array): 256 x 256 x 3
Scale  (1d array):             3
Result (3d array): 256 x 256 x 3
```

* The "trailing" axes above means you are matching the dimension with the rules from the last dimension
* When either of the dimensions compared is one, the other is used. In other words, dimensions with size 1 are stretched or “copied” to match the other.

```python
A      (4d array):  8 x 1 x 6 x 1
B      (3d array):      7 x 1 x 5
Result (4d array):  8 x 7 x 6 x 5
```

Here are examples of shapes that do not broadcast:

```python
A      (1d array):  3
B      (1d array):  4 # trailing dimensions do not match

A      (2d array):      2 x 1
B      (3d array):  8 x 4 x 3 # second from last dimensions mismatched
```


# Base Case: an array and a scalar value are combined in an operation:
```python
a = np.array([1.0,2.0,3.0])
b = 2.0
a * b #array([ 2.,  4.,  6.])
```
* `[1.0,2.0,3.0] * 2` will be 10 times faster than `[1.0,2.0,3.0] * [2.0,2.0,2.0]` in numpy

# Pitfall! Row vector's shape (N,) is actually a row vector with dimension (1, N)
```python
a = np.array([1, 2, 3, 4])
a.shape #(4,)


x = np.arange(4)
xx = x.reshape(4,1)
y = np.ones(5) # (5,) or (1, 5)
x + y #shape mismatch
xx + y # (4, 5)
# y
# array([ 1.,  1.,  1.,  1.,  1.])
#
# xx
# array([[0],
#        [1],
#        [2],
#        [3]])
#
# xx + y
# array([[ 1.,  1.,  1.,  1.,  1.],
#        [ 2.,  2.,  2.,  2.,  2.],
#        [ 3.,  3.,  3.,  3.,  3.],
#        [ 4.,  4.,  4.,  4.,  4.]])
```
