---
layout: post
title: "Back tracking"
excerpt: ""
categories: dsnalgo
tags: [algorithm, backtracking]
comments: true
share: true
author: chungyu
---

# backtracking

## Concepts:

- Problems solved with backtracking usually can only be solved by trying **every** possible configuration and each configuration is **tried only once**.

-  Backtracking is an optimization over this naive approach and works in an **incremental** way.
  - A naive solution for these problems is to try all configurations and output a configuration that works.
  - Name *backtracking* comes from the fact that we go back one level and remove the choice made at that level if that choice did not lead to a solution.


## Leetcode Problems:

### DFS + Branch Pruning:
[(M) 79 Word Search](https://leetcode.com/problems/word-search/)

[(M) 93 Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

[(M) 22 Generate Parentheses
](https://leetcode.com/problems/generate-parentheses/)

### DFS + Special Arrange

[(M) 46 Permutations](https://leetcode.com/problems/permutations/)
[Keys: Pushing the leaf instead of all the nodes!](http://d1gjlxt8vb0knt.cloudfront.net//wp-content/uploads/NewPermutation.gif)
