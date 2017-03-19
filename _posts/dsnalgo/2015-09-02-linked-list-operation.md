---
layout: post
title: "Linked List"
excerpt: ""
categories: dsnalgo
tags: [data structure, linked-list, algorithm]
comments: true
share: true
author: chungyu
---

# Linked List Operation
- Common techniques:
  - Add dummy head in the front

## Basic Operation:
### Reverse
{% highlight python linenos %}
"""
only need two
#      A ---> B ---> C      #
p <--- c
       p <--- c
              p <--- c
                     p <--- c
return p
"""
pre, cur = None, head
while cur:
    cur.next, pre, cur = pre, cur, cur.next
return pre
{% endhighlight %}


## Dummy head!
[(E) 19 Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

[(M) 86 Partition List](https://leetcode.com/problems/partition-list/)

## Two-pointer: Window style!
[(M) 61 Rotate List](https://leetcode.com/problems/rotate-list/)

## Special Case: Cycle-detection
- why slow and fast can find cycle?
    - if fast == slow => must have cycle
    - if fast is going to jump over the slow => fast must == slow in the next step!
- Simple proof for finding collision point in the cycle
    - when collide:
        - supposed slow travel m+k step
        - fast travel m + k + nc step
    - where:
        - m: starting pts
        - k: collide pts in the cycle
        - c: cycle length
        - n: integer constant >= 1
    - then:
        - m + k + nc = 2(m+k) #fast moves 2X faster than slow
        - => (m + k) = nc
        - => starting from k, if we go m steps further, we will be at the beginning of the cycle
        - make a new slow from the beginning, and travel with the slow, if they collide, it must be the starting point

[(E) 141 Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
```python
if not head or not head.next:
    return False
slow = fast = head # for cycle II, if you let fast = head.next, it might fail. do it for consistency
while fast and fast.next: # watch out during fast = fast.next.next, it might become None
    slow, fast = slow.next, fast.next.next
    if id(slow) == id(fast):
        return True
return False
```
[(E) 142 Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
```python
if not head or not head.next:
    return None
slow = fast = head # If you let fast to be head, it might cause an infinite loop on case like [1, 2] repeated
while fast and fast.next:
    slow, fast = slow.next, fast.next.next
    if slow == fast:
        fast = head
        while True:
            if slow == fast:
                return slow
            slow, fast = slow.next, fast.next
return None
```
