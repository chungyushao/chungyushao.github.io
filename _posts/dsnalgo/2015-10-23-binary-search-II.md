---
layout: post
title: "Binary Search II"
excerpt: ""
categories: dsnalgo
tags: [algorithm, binary search]
comments: true
share: true
author: chungyu
---

# Binary Search

[Notes 1 ](http://stackoverflow.com/questions/504335/what-are-the-pitfalls-in-implementing-binary-search)
[Notes 2](http://www.cppblog.com/converse/archive/2009/09/21/96893.aspx)
[Notes 3](http://coldattic.info/shvedsky/pro/blogs/a-foo-walks-into-a-bar/posts/95)
> "Although the basic idea of binary search is comparatively straightforward, the details can be surprisingly tricky" - Knuth


## How to write binary search?

### 1. Maintain an invariant.
- Find/decide and make explicit some invariant property that your "low" and "high" variables satisfy throughout the loop: before, during and after. Make sure it is never violated.

### 2. Think about the termination condition.
