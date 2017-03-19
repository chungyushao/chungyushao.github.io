---
layout: post
title: "Moore Voting Algorithm"
excerpt: ""
categories: dsnalgo
tags: [algorithm]
comments: true
share: true
author: chungyu
---

# Boyer–Moore majority vote algorithm

## [Wiki-Boyer–Moore majority vote algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)

-  The majority vote problem is to determine in any given sequence of choices whether there is a choice with `more occurrences than all the others`, and if so, to determine this choice.

-  If a pair of elements from the list is not the same, then delete both, the last remaining element is the majority number

- [(E) 169 Majority Element](https://leetcode.com/problems/majority-element/)
- [(M) 229 Majority Element II](https://leetcode.com/problems/majority-element-ii/) //key: if it is the majority, both of them should be able to pair out other number special
