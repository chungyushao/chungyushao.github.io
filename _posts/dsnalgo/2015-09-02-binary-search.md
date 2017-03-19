---
layout: post
title: "Binary Search I"
excerpt: ""
categories: dsnalgo
tags: [algorithm, binary search]
comments: true
share: true
author: chungyu
---

# Binary Search

[Noted mainly from Binary Search@topcoder By lovro ](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-search/)

## Quick Tips/Steps:

- Design a predicate which can be efficiently evaluated and so that binary search can be applied
- Decide on what you’re looking for and code so that the search space always contains that (if it exists)
- If the search space consists only of integers, test your algorithm on a two-element set to be sure it doesn’t lock up
- Verify that the lower and upper bounds are not overly constrained: it’s usually better to relax them as long as it doesn’t break the predicate


## Basic Concepts
- Binary search is used to quickly find a value in a sorted sequence.
- When we think a problem can be solved by binary search, we aim to design the predicate so that it satisfies the condition in [the main theorem](###the main theorem).

## Abstract
- If we replace an array lookup with a function evaluation: we are now looking for some x such that f(x) is equal to the target value.

## Terminology:
- We’ll call the sought value the `target` value.
- Binary search maintains a contiguous subsequence of the starting sequence where the target value is surely located. This is called the `search space`.
- Consider a `predicate p` defined over some `ordered set S` (the search space)
  - We use the predicate to verify if a candidate solution is legal (does not violate some constraint) according to the definition of the problem

## The Main Theorem
- Binary search can be used if and only if for all x in S, p(x) implies p(y) for all y > x.
  - p(x) implies p(y) for all y > x or that ¬p(x) implies ¬p(y) for all y < x
  - if you had a yes or no question (the predicate), getting a yes answer for some potential solution x means that you’d also get a yes answer for any element after x.
  - If the condition in the main theorem is satisfied, we can use binary search to find either the first x for which p(x) is true or the last x for which p(x) is false.

## Find first x for which p(x) is true
- [First Bad Version](https://leetcode.com/problems/first-bad-version/)
- “Given an array A and a target value, return the index of the first element in A equal to or greater than the target value.”
- Questions: What the two numbers you maintain (lower and upper bound) mean?
  - A likely answer is a closed interval which surely contains the first x for which p(x) is true.

{% highlight python linenos %}
binary_search(lo, hi, p):
   while lo < hi:
      mid = lo + (hi-lo)/2
      if p(mid) == true:
         hi = mid
      else:
         lo = mid + 1
   if p(lo) == False:
      return None
   return lo # lo is the least x for which p(x) is true, since you always round down for the `mid`
{% endhighlight %}

- Crucial Part-Maintain the invariant: hi = mid and lo = mid+1
  - When p(mid) is true, we can discard the second half of the search space,
    since the predicate is true for all elements in it (by the main theorem).
  - However, we can not discard mid itself, since it may well be the first element for which p is true.

{% highlight python linenos %}
## Find last x for which p(x) if false
binary_search(lo, hi, p):
   while lo < hi:
      mid = lo + (hi-lo)/2
      if p(mid) == true:
         hi = mid - 1
      else:
         lo = mid
   if p(lo) == True:
      return None
   return lo # lo is the least x for which p(x) is true, since you always round down for the `mid`
{% endhighlight %}

## Always test case p = [False, True]!
- For hi = mid in the first case and lo = mid in the second case, the loop might NOT terminate. (For Example: p = [False, True])
- The solution is to change mid = lo + (hi-lo)/2 to mid = lo + (hi-lo+1)/2, i.e. so that it **rounds up instead of down!**

## "mid" Bug
- mid = lo + (hi-lo)/2 instead of the usual mid = (lo+hi)/2
  - Avoid super large hi + lo overflow
  - Avoid "lo" being negative, it round up instead of round down
