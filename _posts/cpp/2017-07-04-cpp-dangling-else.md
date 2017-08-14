---
layout: post
title: "C++ Dangling else"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [Dangling else](https://en.wikipedia.org/wiki/Dangling_else)



# Happen when you write: (move-zeroes)[https://leetcode.com/problems/move-zeroes/#/description]

### Fail
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int firstZero = -1;

        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == 0)
                if (firstZero < 0) firstZero = i;
            else
                if (firstZero >= 0) std::swap(nums[i], nums[firstZero++]);
        }
    }
};
```


### Still fail

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int firstZero = -1;

        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == 0)
                if (firstZero < 0)
                    firstZero = i;
            else
                if (firstZero >= 0)
                    std::swap(nums[i], nums[firstZero++]);
        }
    }
};
```

# [Dr.Dobb's](http://www.drdobbs.com/cpp/dangling-else-yet-again/231602010)

```cpp
if (a)
if (b)
   s;
else
   t;
```
* should it be parsed as:

```cpp
if (a) {
  if (b)
    s;
  else
    t;
}
```
* or

```cpp
if (a) {
  if (b)
    s;
}
else
  t;
```

> * The usual (decades-old) solution for the compiler is to associate the else with the innermost if, and the ambiguity is resolved.

* But what if programmar write

```cpp
if (a)
  if (b)
    s;
else
  t;
```

* ==> it will still parse so that else is actually associated with the inner one, that's where your program has problem
