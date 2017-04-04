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
> * [Effective Morder C++](http://shop.oreilly.com/product/0636920033707.do)

# `std::move`
* `std::move` takes an object and allows you to treat it as a temporary (an rvalue).
* Although it isn't a semantic requirement, typically a function accepting a reference to an rvalue will invalidate it.
* When you see `std::move`, it indicates that the value of the object should not be used afterwards, but you can still assign a new value and continue using it.
* Applying `std::move` to an object tells the compiler that the object is eligible to be moved from. That’s why `std::move` has the name it does: to make it easy to designate **objects that may be moved from**.
* `std::move` not only doesn’t actually move anything, it doesn’t even guarantee that the object it’s casting will be eligible to be moved.
  * The only thing you know for sure about the result of applying `std::move` to an object is that it’s an rvalue.
  * See the session "Caveat from const" below

# `std::forward`
* `std::forward` has a single use case: to cast a templated function parameter (inside the function) to the value category (lvalue or rvalue) the caller used to pass it.
* This allows rvalue arguments to be passed on as rvalues, and lvalues to be passed on as lvalues, a scheme called "perfect forwarding."

# Summary of the two
* `std::move` doesn’t move anything. `std::forward` doesn’t forward anything. At run‐ time, neither does anything at all. They generate no executable code.
* `std::move` and `std::forward` are merely functions (actually function templates) that perform casts.
  * `std::move` unconditionally casts its argument to an rvalue, while
  * `std::forward` performs this cast only if a particular condition is fulfilled.
* The story for `std::forward` is similar to that for `std::move`, but whereas `std::move` unconditionally casts its argument to an rvalue, `std::forward` does it only under certain conditions. `std::forward` is a conditional cast.

# Caveat from const

```cpp
class Annotation {
public:
  explicit Annotation(const std::string text)
  : value(std::move(text)) // "move" text into value; this code { ... } // doesn't do what it seems to!
  ...
private:
  std::string value;
};

```

* the result of `std::move(text)` is an rvalue of type `const std::string`.
  * That rvalue can’t be passed to `std::string`’s move constructor, because the move constructor takes an rvalue reference to a non-const `std::string`.
* The rvalue can, however, be passed to the copy constructor, because an **lvalue-reference-to-const is permitted to bind to a const rvalue.**
* The member initialization therefore invokes the copy constructor in `std::string`, even though text has been cast to an rvalue!

> Don’t declare objects const if you want to be able to move from them. Move requests on const objects are
silently transformed into copy operations.



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
