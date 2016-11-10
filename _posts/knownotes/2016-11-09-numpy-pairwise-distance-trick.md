---
layout: post
title: "Pairwise Distance Trick in Numpy"
excerpt: "pairwise distance"
categories: knownotes
tags: [python, numpy]
comments: true
share: true
author: chungyu
---

> Mainly from PDS course recitation
```python
n,m,k = 4,6,2
X = np.random.randn(n,k)
Y = np.random.randn(m,k)
D = np.sum(X**2,axis=1)[:,None] + np.sum(Y**2, axis=1) + (-2 * X.dot(Y.T) + )
```

```python
X = [(a, b), (c, d), (e, f)] # 3 points
Y  = [(g, h)] #  1 points
```

###### Pairwise distance simple example

* \\( (a - g) ^ 2 + (b - h) ^ 2 = (a^2 + b^2) + (g^2 + h^2) -2(ag + bh) \\)
* \\( (c - g) ^ 2 + (d - h) ^ 2 = (c^2 + d^2) + (g^2 + h^2) -2(cg + dh) \\)
* \\( (e - g) ^ 2 + (f - h) ^ 2 = (e^2 + f^2) + (g^2 + h^2) -2(eg + fh) \\)
