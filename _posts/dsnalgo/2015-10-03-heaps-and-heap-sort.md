---
layout: post
title: "Heaps"
excerpt: ""
categories: dsnalgo
tags: [data structure, algorithm]
comments: true
share: true
author: chungyu
---

# Heaps and Heap Sort


## Resource
[Heaps and Heap Sort](http://goo.gl/wJqjmW)

## Motivation: Priority Queue
  - Expected manipulation:
    - `insert`
    - `max`
    - `pop_max`
    - `increase_key`: increase key of certain value to another value
  - heap is an implementation for priority queue

## Heap
  - **An array visualized to a nearly complete binary tree is call a heap**
    - root of tree is the first element in array
    - #### For node_i
      - ##### parent(node_i) @ i/2
      - ##### left(node_i)   @ 2*i
      - ##### right(node_i)  @ 2*i+1
    - Good news assumed the array has n elements:
      - The height would be bounded as log(n)


  - Max-heap property:
    - The keys of a node >= keys of its children for every node in the tree
    - <= for min-heap property
  - Then, `max` is trivially performed for max-heap, but NOT `pop_max`
## Heap operations
  - `build_max_heap`: produce a max-heap from an unordered array
  - `max_heapify`: correct a single violation of a heap property in a subtree's root

## max-heapify(i):
  - Assume that the tree rooted, left(i) and right(i) are max-heaps
  - Complexity: O(log(n))

## build-max-heap: Convert A[1, ..., n] into a max-heap
  - for i = n/2 down to 1:
      max_heapify(A, i)
  - why down to, why start from n/2
    - [n/2 +1, ..., n] are all leafs!!
    - Leaf must satisfy the assumption for max-heapify
  - Complexity: O(nlog(n))? No, actually O(n)
    - Actually, though when the level are high, the operations would be more, the node need to operation would decrease!
    - Observe that max_heapify taken O(1) for nodes that are one level above the leafs
    - and in general O(l) times for nodes that are l levels above the leafs
    - Could be proved through Mathmatics!  
