---
layout: post
title: "In-order Tree Traversal"
excerpt: ""
categories: dsnalgo
tags: [algorithm, tree, data structure]
comments: true
share: true
author: chungyu
---

# Iterative in-order tree traversal

### View Point 1
  - You first go to the leftest node from root and store the node you traverse into a stack
  - Then you start pop nodes out.
  - Every time you pop a node out, you just check with whether it does have a right node
  - if it does, put the right node and the right node's subsiquent left node into the stack
{% highlight python %}
"""
    a
    /\
   b  c
  /\
 d  e
    /\
    f g
      /
      h

    insert     pop
   [a, b, d] => d
             => b
=> [a, e, f] => f
             => e
=> [a, g, h] => h
             => g
             => a
=> [c]       => c
"""
{% endhighlight %}

### View Point 2
  - Every time you see a new node, you append all it's left node into the stack
  - if nothing is in the stack, you already traverse all the tree
  - Otherwise, pop node from stack, and make node to be it's right node
  {% highlight python %}
def inorderTraversal(self, root):
    res, stack = [], []
    while True:
        while root:
            stack.append(root)
            root = root.left
        if not stack:
            return res
        node = stack.pop()
        res.append(node.val)
        root = node.right
  {% endhighlight %}

### [Morris Tree Traversal](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html)
