---
layout: post
title: "Type of values"
excerpt: "lvalue, rvalue, glvalue, xvalue, prvalue"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [cppreference](http://en.cppreference.com/w/cpp/language/value_category)
> * [stackoverflow](http://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues)

# Why need?
* The C++ standard contains many rules that deal with the value category of an expression.
* Some rules make a distinction between lvalue and rvalue.
  * For example, when it comes to overload resolution.
* Other rules make a distinction between glvalue and prvalue.
  * For example, you can have a glvalue with an incomplete or abstract type but there is no prvalue with an incomplete or abstract type.
* ...


# General view

> This taxonomy went through significant changes with past C++ standard revisions

```
   ______ ______
  /      X      \
 /      / \      \
|   l  | x |  pr  |
 \      \ /      /
  \______X______/
      gl    r                   by sellibitze
```


![stackoverflow](https://i.stack.imgur.com/GNhBF.png)

# Explanation
* An **lvalue** (so called, historically, because lvalues could appear on the left-hand side of an assignment expression) **designates a function or an object**.
  * If `E` is an expression of pointer type, then `*E` is an lvalue expression referring to the object or function to which `E` points.
  * As another example, the result of calling a function whose return type is an lvalue reference is an lvalue.

* An **xvalue** (an “eXpiring” value) also refers to an object, usually near the end of its lifetime (so that its resources may be moved, for example).
  * An xvalue is the result of certain kinds of expressions involving rvalue references (8.3.2).
  * The result of calling a function whose return type is an rvalue reference is an xvalue.

* A **glvalue** (“generalized” lvalue) is an lvalue or an xvalue.

* An **rvalue** (so called, historically, because rvalues could appear on the right-hand side of an assignment expressions) is an xvalue, a temporary object (12.2) or subobject thereof, or a value that is not associated with an object.

* A **prvalue** (“pure” rvalue) is an rvalue that is not an xvalue.
  * The result of calling a function whose return type is not a reference is a prvalue.
  * The value of a literal such as 12, 7.3e5, or true is also a prvalue.
