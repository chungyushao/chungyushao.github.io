---
layout: post
title: "C++ Data Structures Comparison"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> Data Structures and Algorithm Analysis in C++ 4-th Edition, Mark Allen Weiss



# `vector` and `list`
##### `vector`
The vector provides a **growable array** implementation of the List ADT.
###### Pros
* The advantage of using the vector is that it is indexable in constant time.
###### Cons
* The disadvantage is that insertion of new items and removal of existing items is expensive, unless the changes are made at the end of the vector.

##### `list`
The list provides a **doubly linked list** implementation of the List ADT.
###### Pros
* The advantage of using the list is that insertion of new items and removal of existing items is cheap, provided that the position of the changes is known.
###### Cons
The disadvantage is that the list is not easily indexable.

> Both `vector` and `list` are inefficient for searches.
