---
layout: post
title: "Two Pointer"
excerpt: ""
categories: dsnalgo
tags: [algorithm, two-pointer]
comments: true
share: true
author: chungyu
---

# Two Pointers

## Basic concepts
- left and right,
- keep left < right all the time
- and check left == right when checking


## k-Sum ordinary form
[(M) 167 Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

[(M) 15 3Sum](https://leetcode.com/problems/3sum/) //think about how to avoid duplicates!

[(M) 16 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

[(M) 259	3Sum Smaller](https://leetcode.com/problems/3sum-smaller/) // cool mutant!! fun to think!

[(M) 18 4Sum](https://leetcode.com/problems/4sum/) // any k-sum can reduce to 2 sum!




## Take care of the buggy index!

[(E) 26 Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) //fairly easy one

[(M) 80 Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/) //small detail that could go wrong

[(E) 28 Implement strStr()](https://leetcode.com/problems/implement-strstr/) // fairly easy one

[(E) 125 Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) // also beautiful build-in solution!

[(E) 27 Remove Element](https://leetcode.com/problems/remove-element/) // seems easy but damnnnn

[(E) 283 Move Zeroes](https://leetcode.com/problems/move-zeroes/) // alike (27), slightly easier

[(M) 209 Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) // u fck up last time~ calculate on int is far faster than calculate on array! Even if it only take O(1) time? weird performance for the array version

[(M) 3 Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) //how to change begin is a problem!


## Think which pointer to move!!
[(E) 88 Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) //trick: do from the last to be inplace!

[(M) 11 Container With Most Water](https://leetcode.com/problems/container-with-most-water/) //fun to think!

[(M) 75 Sort Colors](https://leetcode.com/problems/sort-colors/) //beautiful 3 pointers method to solve!!

{% highlight python linenos %}
def Sort Colors:
    """
    201->102->012
    """
    if not nums:
        return
    cur, zero, two = 0, 0, len(nums)-1
    while cur <= two:
        if nums[cur] == 0:
            nums[cur], nums[zero] = nums[zero], nums[cur]
            zero += 1
            cur += 1
        elif nums[cur] == 2:
            nums[two], nums[cur] = nums[cur], nums[two]
            two -= 1
        else:
            cur += 1
{% endhighlight %}

{% highlight python linenos %}
def sqrt:
    """
    left or right +1/ -1 ==> must be overlapped some time
    ==> while l != r: would also pass!!
    """
    l, r = 0, x
    while l < r:
        mid = (l + r) / 2
        if mid * mid <= x < (mid + 1) * (mid + 1):
            return mid
        elif mid * mid < x:
            l = mid + 1
        else:
            r = mid - 1
    return l
{% endhighlight %}
