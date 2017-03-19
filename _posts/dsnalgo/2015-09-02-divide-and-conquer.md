---
layout: post
title: "Divide and Conquer"
excerpt: ""
categories: dsnalgo
tags: [algorithm, divideNConquer]
comments: true
share: true
author: chungyu
---

# Divide and Conquer
## Resource
[Divide and Conquer](http://goo.gl/Yd5tGK)
- General paradigm
  - Divide the problem (instance) into one or more subproblems
  - Conquer each subproblems recursively
  - Combine solutions
  - Example 1: Binary Search
    1. Divide: compare x with middle
    2. Conquer: recurse in one subarray
    3. Combine: trivial
  - Example 2: Powering a number
    1. Divide: divide to 2 cases: even and odd, and calculate pow(x, n/2)
    2. Conquer: pow(x, n/2)
    3. Combine: pow(x, n/2) * pow(x, n/2) or x * pow(x, n/2) * pow(x, n/2)
  - Example 3: Matrix Multiple
    1. Divide: n X n matrix = 2x2 block matrix of n/2 X n/2 sub-matrix
    2. Conquer: ...
    3. Combine: ...
- Running time
  - Need to know Master's theorem... [TBC]

## Leetcode Problems
### Awesome!
- [(M) 295 Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

- [(M) 105 Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) //105, 106 => see more on tree.md!

- [(M) 106 Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
