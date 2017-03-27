---
layout: post
title: "Bit Manipulation"
excerpt: ""
categories: dsnalgo
tags: [algorithm, bitManipulation]
comments: true
share: true
author: chungyu
---

# General Bit Operation
* `&`: 通常用来将某变量中的某些位清0且同时保留其他位不变。也可以用来获取某变量中的某一位。
* `|`: 通常用来将某变量中的某些位置1且保留其他位不变。
* `^`: 通常用来将某变量中的某些位取反, 且保留其他位不变。
* `>>`: 右移n位,就相当于左操作数除以 \$ 2^n \$

## Stephen's Trick
- `nums[0]` : `nums[~0] == nums[-1]`
- `nums[1]` : `nums[~1] == nums[-2]`
- `nums[i]` : `nums[~i] == nums[-i]`
- 2's complement can give u the corresponding index from the end!

## [The Magic of XOR](http://www.cs.umd.edu/class/sum2003/cmsc311/Notes/BitOp/xor.html)
- For plain XOR, not bitwise XOR
	- p ^ q is true if exactly one of p and q is true.
	- p1 ^ p2 ^ ... ^ pn is true if the number of variables with the value true is odd
		- If pi is true, then treat it as a 1, otherwise treat it as a 0.
		- p1 ^ p2 ^ ... ^ pn == ( p1 + p2 + ... + pn ) % 2
- Properties of XOR that appliy to plain XOR and bitwise XOR
	- x ^ 0 = x
		- XORing with 0 gives you back the same number.
	- x ^ ~0 = ~x  
		- XORing with all 1 gives you back the bitwise negation of x.
	- x ^ x = 0
	- (x ^ y) ^ z = x ^ (y ^ z)
		- XOR is associative.
	- x ^ y = y ^ x
		- XOR is commutative.

```python
def swap(x, y):
	x = x ^ y ;
	y = x ^ y ;
	x = x ^ y ;
```


## LeetCode Questions for XOR

- [(M) 136 Single Number](https://leetcode.com/problems/single-number/)
- [(M) 268 Missing Number](https://leetcode.com/problems/missing-number/)
- [(M) 137 Single Number II](https://leetcode.com/problems/single-number-ii/)
- [(M) 260 Single Number III](https://leetcode.com/problems/single-number-iii/)

TODO: Take a look at [this thread](https://leetcode.com/discuss/54970/an-general-way-to-handle-all-this-sort-of-questions)

## Binary-Based Approaches to Checking for Powers of Two
{% highlight python %}
def isPowerOfTwo(x):
	# x is positive
	return x != 0 and x & (x-1)
{% endhighlight %}
- from [Ten Ways to Check if an Integer Is a Power Of Two in C](http://www.exploringbinary.com/ten-ways-to-check-if-an-integer-is-a-power-of-two-in-c/)

> Let n be the position of the leftmost 1 bit if x. If x is a power of two, its lone 1 bit is in position n. This means x – 1 has a 0 in position n. To see why, recall how binary subtraction works. When subtracting 1 from x, the borrow propagates all the way to position n; bit n becomes 0 and all lower bits become 1. Now, since x has no 1 bits in common with x – 1, x & (x – 1) is 0, and !(x & (x – 1)) is true.


> If x is not a power of two, x – 1 has a 1 in position n. This is because the borrow will not propagate to position n. Subtraction borrows from the lowest 1 bit, which by virtue of x not being a power of two, is before position n. The lowest 1 bit is like a firewall that prevents the borrow from reaching position n. Since x and x – 1 have at least one 1 bit in common — at position n — x & (x – 1) is nonzero, and !(x & (x – 1)) is false.

## Hamming weight in O(1):
	        x = abcdefgh  | a =     x  & mask1 = 0b0d0f0h
	==> mask1 = 01010101  | b = (x>>1) & mask1 = 0a0c0e0g
---------------------------------------------------------
	==>  a + b =





## Build-in Functions
	- int('00100001', 2) ==> 33
	- bin(33) ==> '0b100001'
