---
layout: post
title: "Iterative"
excerpt: ""
categories: dsnalgo
tags: [algorithm, iterative]
comments: true
share: true
author: chungyu
---

## Damnn

- [(M) 78 Subsets](https://leetcode.com/problems/subsets/)
- [(M) 90 Subsets II](https://leetcode.com/problems/subsets-ii/)
  - 78, 90 originally tag as backtracking, but iterative solution is like using DP??
  - No, you don't need to use DP to memorize, see followings:

{% highlight python linenos %}
    def subsets_I(self, nums):
        nums.sort()
        res = [[]]
        for i in range(len(nums)):
            n, l = nums[i], len(res)
             # This is the key, the l element is just the whole subset generated before
            for j in range(l):
                res.append(res[j] + [n])
        return res
    def subsets_II(self, S):
          res, preCount = [[]], 1
          S.sort()
          for i in range(len(S)):
              if i == 0 or S[i] != S[i - 1]:
                  preCount = len(res)
              for j in range(len(res) - preCount, len(res)):
                  res.append(res[j] + [S[i]])
          return res
{% endhighlight %}
