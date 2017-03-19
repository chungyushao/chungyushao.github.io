---
layout: post
title: "Pre-order tree traversal"
excerpt: ""
categories: dsnalgo
tags: [algorithm, tree, data structure]
comments: true
share: true
author: chungyu
---

# Iterative pre-order tree traversal


### View Point 1
  - Since you will pop the root node out, you won't need to store it into Stack
  - Every time you see a new node, you simply push right node first then left node into stack
  - (Since the left node should be pop out earlier than right node, so you should pup right first)
{% highlight python %}
def preorderTraversal(self, root):
    if not root:
        return []
    stack, res = [root], []
    while stack:
        node = stack.pop()
        res.append(node.val)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    return res
{% endhighlight %}


### View Point 2
  - Since you will pop the root node out, you won't need to store it into Stack
  - What would be postpone is the right node, so I would put the right node into the stack
  - When to put the right node? Every time you see a new node/root!
{% highlight python %}
def preorderTraversal(self, root):
   stack, res = [root], []
   while stack:
       node = stack.pop()
       while node:
           res.append(node.val)
           if node.right:
               stack.append(node.right)
           node = node.left
   return  res
{% endhighlight %}
### [Morris Tree Traversal](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html)
