---
layout: post
title: "Recursion"
excerpt: ""
categories: dsnalgo
tags: [algorithm, recursion]
comments: true
share: true
author: chungyu
---

# Recursion

["An Introduction to Recursion: Part 1"@topcoder](https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-recursion-part-1/)

["An Introduction to Recursion: Part 2"@topcoder ](https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-recursion-part-2/)
## Key considerations in designing a recursive algorithm
- It handles a simple “base case” without using recursion.
- It avoids cycles.
  - You can avoid cycles by having each function call be for a problem that is somehow **smaller or simpler** than the original problem.
- Each call of the function represents a complete handling of the given task.
  -  We can pass a part of the job along to a recursive call, but the original function still has to account for all somehow.

## Overall Suggestions:
- In order to make recursion work, we must have a very clear specification of what each function call is doing or else we can end up with some very difficult to debug errors.
- Even if time is tight it’s often worth starting out by `writing a comment detailing exactly what the function is supposed to do`.
- In general, if you find a recursive solution for a problem, but find that the solution runs too slowly, then the solution is often memoization.

## When use Recursion
- Scenario #1: Hierarchies, Networks, or Graphs
- Scenario #2: Multiple Related Decisions
  - like searching in a Maze
- Scenario #3: Explicit Recursive Relationships



## iterative, recursive and backtracking:
    - When we encounter a problem that requires repetition, we often use "iteration" – i.e., some type of loop.
    - An alternative approach to problems that require repetition is to solve them using "recursion".
      A recursive method is a method that calls itself.
    - Backtracking is useful for constraint satisfaction problems
      that involve assigning values to variables according to a set of constraints.
      Backtracking reduces the number of possible value assignments that we consider, because it never considers invalid assignments.

## two key elements:
	- (1) identifying the base cases
    - When we use recursion, we solve a problem by reducing it to a simpler problem of the same kind.
    - We keep doing this until we reach a problem that is simple enough to be solved directly.
    - This simplest problem is known as the base case.
    - The base case stops the recursion, because it doesn't' make another call to the method.
	- (2) ensuring progress
    - The recursive case:
        - reduces the overall problem to one or more simpler problems of the same kind
        - makes recursive calls to solve the simpler problems
        - When we make the recursive call, we typically use parameters that bring us closer to a base case.

## divide-and-conquer v.s. recursive:
- DD:
		- (1) divide the problem into two or more independent smaller problems
		- (2) to reduce space complexity, DD usually being implemented as iteration, not recursion
- Re:
		- (1) maybe only single subproblem
		- (2) subproblems may not be independent
		- (3) might not be the same type as original
