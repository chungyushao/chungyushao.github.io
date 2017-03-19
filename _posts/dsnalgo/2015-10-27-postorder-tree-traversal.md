---
layout: post
title: "Post-order Tree Traversal"
excerpt: ""
categories: dsnalgo
tags: [algorithm, tree, data structure]
comments: true
share: true
author: chungyu
---

# Iterative post-order tree traversal

### View Point 1
  - Basically, if you change preorder traversal to be Root=>Right=>left
  - Then the reversed of the result would be like post order
  - Take a look at View Point 1 of preorder traversal
{% highlight python %}
def postorderTraversal(self, root):
    if not root:
        return []
    stack, res = [root], []
    while stack:
        node = stack.pop()
        res.append(node.val)
        if node.left:
            stack.append(node.left)
        if node.right:
            stack.append(node.right)
    return res[::-1]
{% endhighlight %}


### View Point 2
  －
{% highlight python %}

{% endhighlight %}
### [Morris Tree Traversal](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html)
