---
layout: post
title: "LeetCode 437 Path Sum III"
excerpt: ""
categories: dsnalgo
tags: [tree, hashmap]
comments: true
share: true
author: chungyu
---

# Solution
* [nice explanation](https://discuss.leetcode.com/topic/64526/17-ms-o-n-java-prefix-sum-method/15)

### Some Notes
* Can view each **path** to be a prefix sum problem, and solve it using hashmap as two sum problem.
  * For example:

```
    10
   /  
  5   
 / \   
3   2
```
  * has two path "10->5->3" and "10->5->2"
  * the subpath "5->3" can be counted using path (10->5->3) minus path (10)
* Everytime the tree goes deeper recusion, it adds up the current node's value, which would be the total path sum from root to the current node.
* We store some previous path sum as the key of the hashmap, with frequency as value.

> If the difference between the current sum and the target value exists in the map, there must exist a node in the middle of the path, such that from this node to the current node, the sum is equal to the target value.
