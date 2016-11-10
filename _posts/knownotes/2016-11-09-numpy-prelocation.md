---
layout: post
title: "Prelocation in Numpy"
excerpt: "different between numpy array and list"
categories: knownotes
tags: [python, numpy]
comments: true
share: true
author: chungyu
---

> Mainly from PDS course recitation

# Difference between Numpy array and List

* Lists are implemented as unbounded arrays and can be resized somewhat efficiently
* numpy arrays are optimized for fixed shape operations.

* Pre-allocate any matrices that you'll use over and over again that don't change (like identity matrices), instead of recreating them every iteration

###### iteratively append rows to a matrix

```python
X = np.ones(1000)
# (1000,)
for _ in range(200):
    X = np.vstack([X, np.ones(1000)])
# (201, 10000)

# 10 loops, best of 3: 46.4 ms per loop
```

###### preallocate and assign values

```python
X = np.zeros((200, 1000))
for i in range(200):
    X[i] = np.ones(n)
# 1000 loops, best of 3: 1.3 ms per loop		
```

###### preallocate matrices that don't change

```python
#
X = np.random.randn(1000, 1000) #(1000, 1000)
D = np.eye(1000) * 1e-4 #(1000, 1000)

[X + D for _ in range(10)]
# list with 10 elements, each one is a (1000, 1000) matrix
# 10 loops, best of 3: 40.6 ms per loop

[X + np.eye(1000) * 1e-4 for _ in range(10)]
# 10 loops, best of 3: 96.4 ms per loop

```
