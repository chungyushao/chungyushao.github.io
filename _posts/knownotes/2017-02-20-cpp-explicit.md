---
layout: post
title: "C++ `explicit`"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---
> [cppreference.com](http://en.cppreference.com/w/cpp/language/explicit)

* The `explicit` specifier specifies that a constructor or conversion function (since C++11) doesn't allow implicit conversions or copy-initialization.

##### Constructor example:
* `B b1 = 1;`: error: copy-initialization CANNOT select `B::B(int)` since `explicit`
* `B b4 = {4, 5};`: // error: copy-list-initialization CANNOT select `B::B(int,int)` since `explicit`

##### Conversion example: 
* `bool nb1 = b2;`: // error: copy-initialization CANNOT select `B::operator bool()` since `explicit`


```cpp
struct A
{
    A(int) { }      // converting constructor
    A(int, int) { } // converting constructor (C++11)
    operator bool() const { return true; }
};

struct B
{
    explicit B(int) { }
    explicit B(int, int) { }
    explicit operator bool() const { return true; }
};

int main()
{
    A a1 = 1;      // OK: copy-initialization selects A::A(int)
    A a2(2);       // OK: direct-initialization selects A::A(int)
    A a3 {4, 5};   // OK: direct-list-initialization selects A::A(int, int)
    A a4 = {4, 5}; // OK: copy-list-initialization selects A::A(int, int)
    A a5 = (A)1;   // OK: explicit cast performs static_cast
    if (a1) ;      // OK: A::operator bool()
    bool na1 = a1; // OK: copy-initialization selects A::operator bool()
    bool na2 = static_cast<bool>(a1); // OK: static_cast performs direct-initialization

//  B b1 = 1;      // error: copy-initialization CANNOT select B::B(int) since explicit
    B b2(2);       // OK: direct-initialization selects B::B(int)
    B b3 {4, 5};   // OK: direct-list-initialization selects B::B(int, int)
//  B b4 = {4, 5}; // error: copy-list-initialization does not consider B::B(int,int)
    B b5 = (B)1;   // OK: explicit cast performs static_cast
    if (b2) ;      // OK: B::operator bool()
//  bool nb1 = b2; // error: copy-initialization does not consider B::operator bool()
    bool nb2 = static_cast<bool>(b2); // OK: static_cast performs direct-initialization
}
```
