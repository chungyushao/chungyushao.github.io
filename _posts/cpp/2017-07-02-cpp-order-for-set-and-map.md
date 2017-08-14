---
layout: post
title: "C++ Order for `std::set` and `std::map`"
excerpt: ""
categories: cpp
tags: [C++, lambda]
comments: true
share: true
author: chungyu
---

> * [Stackoverflow](https://stackoverflow.com/questions/8833938/is-the-stdset-iteration-order-always-ascending-according-to-the-c-specificat)
> * [cppreference](http://en.cppreference.com/w/cpp/container/set)

* `std::set` is an associative container that contains a **sorted** set of unique objects of type Key.
  * Sorting is done using the key comparison function Compare.
  * Search, removal, and insertion operations have **logarithmic complexity**.
  * Sets are usually implemented as red-black trees
