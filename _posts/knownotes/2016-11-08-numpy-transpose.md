---
layout: post
title: "1-D Array Transpose"
excerpt: "trnaspose 1-D array, newaxis"
categories: knownotes
tags: [python, numpy]
comments: true
share: true
author: chungyu
---

> Mainly from [thread](http://stackoverflow.com/questions/5954603/transposing-a-numpy-array)

* The transpose of a 1D array is still a 1D array
* If you want to turn your 1D vector into a 2D array and then transpose it, just slice it with `np.newaxis` (or `None`)
	* they're the same, newaxis is just more readable

```python

print np.newaxis
None

a = np.array([5, 4])
# array([5, 4])

a.T
array([5, 4])

a[None]
# array([[5, 4]])

a[np.newaxis]
# array([[5, 4]])

a[None, :]
# array([[5, 4]])

a[:, np.newaxis]
# array([[5],
#        [4]])

```

> Generally speaking though, you don't ever need to worry about this. Adding the extra dimension is usually not what you want, if you're just doing it out of habit. Numpy will automatically broadcast a 1D array when doing various calculations. There's usually no need to distinguish between a row vector and a column vector (neither of which are vectors. They're both 2D!) when you just want a vector.


