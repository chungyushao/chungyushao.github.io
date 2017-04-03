---
layout: post
title: "Perfect forwarding"
excerpt: "std::move, std::forward, reference collapsing rules"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [Thomas Becker](http://thbecker.net/articles/rvalue_references/section_07.html)
> * [stackoverflow 1](http://stackoverflow.com/questions/13725747/concise-explanation-of-reference-collapsing-rules-requested-1-a-a-2)
> * [stackoverflow 2](http://stackoverflow.com/questions/9671749/whats-the-difference-between-stdmove-and-stdforward)
> * [cppreference](http://en.cppreference.com/w/cpp/utility/forward)


# `std::move`
* `std::move` takes an object and allows you to treat it as a temporary (an rvalue).
* Although it isn't a semantic requirement, typically a function accepting a reference to an rvalue will invalidate it.
* When you see `std::move`, it indicates that the value of the object should not be used afterwards, but you can still assign a new value and continue using it.

# `std::forward`
* `std::forward` has a single use case: to cast a templated function parameter (inside the function) to the value category (lvalue or rvalue) the caller used to pass it.
* This allows rvalue arguments to be passed on as rvalues, and lvalues to be passed on as lvalues, a scheme called "perfect forwarding."

# Perfect forwarding
> Effectively forward parameters as if the user had called the function directly (minus elision, which is broken by forwarding)

* Consider

```cpp
template<class T>
void Fwd(T &&v) { Call(std::forward<T>(v)); }
```

# Types of passing
  * lvalues
  * xvalues
  * and prvalues

# Types of return/receiving
  * by value
  * by (possibly const) lvalue reference
  * and by (possibly const) rvalue reference
