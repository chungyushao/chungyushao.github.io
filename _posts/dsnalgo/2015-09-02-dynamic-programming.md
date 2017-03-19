---
layout: post
title: "Dynamic Programming"
excerpt: ""
categories: dsnalgo
tags: [algorithm, DP]
comments: true
share: true
author: chungyu
---

> Guess is a try and test method for solving any problem!!- [Erik Demaine](http://erikdemaine.org)

## Resource
[Dynamic Programming I](http://goo.gl/wj4kud)

## Five easy steps to DP
- Define subproblems
- Guess (part of solutions/ features of solutions)
- Relate subproblems solutions
- Recurse and memorize OR build DP table bottom-up
- Solve original problem


## Notes
- DP ~ **careful** brute force
- DP ~ subproblems + **reuse/memorize** ![Figure 1]({{ site.url }}/assets/imgs/dp1.png)
- Generally, DP means memorize and reuse solutions to subproblems
   that help solve the problem
   - **DP~ recursion + memorization**
   - **time = #subproblems * time per subproblems**
      - don't count recursion time, since I only need to count once
- What u often use, dp-array solution is actually **Bottom-up DP algorithm** ![Figure 2]({{ site.url }}/assets/imgs/dp2.png)
- **DP ~ recursion + memorization + GUESS!!**
  - try all the guess *carefully!* and take the *best*
- Subproblems dependencies should be acyclic, or u get infinite time algorithm!
  - What if it's cyclic? ![Figure 3]({{ site.url }}/assets/imgs/dp3.png)

## Leetcode Problems

### State now is affected by previous state:
- [(M) 53 Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- [(M) 121 Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### State now is affected by previous 2 states (Hanoi-like):
- [(E) 70 Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- [(M) 62 Unique Paths](https://leetcode.com/problems/unique-paths/)
- [(M) 63 Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)
- [(M) 64 Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
- [(M) 120 Triangle](https://leetcode.com/problems/triangle/)
- [(M) 256 Paint House](https://leetcode.com/problems/paint-house/)
- [(M) 152 Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

### State now is affected by all previous states
- [(M) 139 Word Break](https://leetcode.com/problems/word-break/)
- [(M) 279 Perfect Squares](https://leetcode.com/problems/perfect-squares/)

### 2 separate situations from *previous state*:
- [(E) 276 Paint Fence](https://leetcode.com/problems/paint-fence/)
- [(E) 198 House Robber](https://leetcode.com/problems/house-robber/)
- [(M) 91 Decode Ways](https://leetcode.com/problems/decode-ways/)
### Cool DP!
- [(M) 96 Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

### WTF?
- [(M) 264 Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)

###### Log:

###### 20150927: Finish Medium problems so far at Leetcode!

###### 20150926: This is my first .md Note! Seems NICE...^\_^
