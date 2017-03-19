---
layout: post
title: "Binary Search Tree"
excerpt: ""
categories: dsnalgo
tags: [data structure, algorithm, BST, tree]
comments: true
share: true
author: chungyu
---

# Binary Search Tree

# Basic Concepts:
- Each node has:
  - values (`key`s as text-book's terminology)
  - left pointer, right pointer, parent pointer
- BST properties:
  - all values for all nodes in left subtrees <= root's value
  - all values for all nodes in right subtrees >= root's value
- height, h, is length of longest path from root down to leaf.
  - `find_min`: O(h) <==> heap: O(1)
  - `next_larger(x)`: O(h)

## Balanced BST:
- If the height is theta(lg(n))
- AVL tree: the simplest and earliest of BST found to be balanced
- Intuitively, we want to keep height small, so we need to keep the information
  - height of a node: length of longest path from it down to leaf.
    - == max(height(left) + height(right)) + 1

## AVL Trees
- require heights of left and right children of **every node** to differ by at most +- 1
- Prove: AVL trees are balanced:
  - worst case: right subtree has height 1 more than left for every node
  - define N_h = min number of nodes in an AVL tree of height h
  - N_h = 1 + N_(h-1) + N_(h-2)
  ![Figure 1]({{ site.url }}/assets/imgs/BST1.png)
  - Like Fibonacci equation, prove mathematically through Fibonacci number
  - or N_h = 1 + N_(h-1) + N_(h-2) > 1 + 2*(N_(h-2)) ...

## AVL Trees insertion:
  - simple BST insertion
  - then fix AVL property, how?
  - Rotations!
  ![Figure 2]({{ site.url }}/assets/imgs/BST2.png)
  - Fix AVL by rotate from changed node up
  - Supposed x in lowest node violating AVL, assume right(x) higher than left(x)
  ![Figure 3]({{ site.url }}/assets/imgs/BST4.png)
    - if x's right child is right heavy or balanced
    => Right Rotate(x) would make it balanced
    - if x's right child is left heavy or balanced
    => Right Rotate(x) then Left Rotate would make it balanced!


## Leetcode questions

[(E) 235 Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
  - Common ancestor must satisfy: p.val <**=** root.val <**=** q.val
  - Equal sign is the KEY!!
  - If it's not, deep left if root.val > p.val and root.val > q.val, vice versa

[(M) 96	Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/) //DP

[(M) 95 Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)
