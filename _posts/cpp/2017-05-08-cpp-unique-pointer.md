---
layout: post
title: "C++ unique pointer"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [cplusplus.com](http://www.cplusplus.com/reference/memory/unique_ptr/)
> * [stackoverflow](http://stackoverflow.com/questions/8114276/how-do-i-pass-a-unique-ptr-argument-to-a-constructor-or-a-function)


# Introduction

* Manages the storage of a pointer, providing a limited garbage-collection facility, with little to no overhead over built-in pointers **(depending on the deleter used)**.

* These objects have the ability of taking ownership of a pointer: once they take ownership they manage the pointed object **by becoming responsible for its deletion at some point**.

* `unique_ptr` objects automatically delete the object they manage (using a deleter)
  * **as soon as they themselves are destroyed**,
  * or as soon as their value changes either by an **assignment operation** or by an explicit call to `unique_ptr::reset`.

* `unique_ptr` objects own their pointer uniquely:
  * no other facility shall take care of deleting the object, and thus no other managed pointer should point to its managed object,
  * since as soon as they have to, `unique_ptr` objects delete their managed object without taking into account whether other pointers still point to the same object or not, and thus leaving any other pointers that point there as pointing to an invalid location.

* A unique_ptr object has two components:
  * a stored pointer: the pointer to the object it manages. This is set on construction, can be altered by an assignment operation or by calling member `reset`, and can be individually accessed for reading using members `get` or `release`.
  * a stored deleter: a callable object that takes an argument of the same type as the stored pointer and is called to delete the managed object. It is set on construction, can be altered by an assignment operation, and can be individually accessed using member `get_deleter`.

* unique_ptr objects replicate a limited pointer functionality by providing access to its managed object through operators `*` and `->` (for individual objects), or operator `[]` (for array objects).
* For safety reasons, they do not support pointer arithmetics, and **only support move assignment (disabling copy assignments)**.


# How do I pass a unique_ptr argument to a constructor or a function?

```cpp
#include <memory>
class Base {
public:
    Base(){}
    Base(unique_ptr<Base> n) : next(std::move(n)) {}
    virtual ~Base() {}
    void setNext(unique_ptr<Base> n) { next = std::move(n); }
protected :
    unique_ptr<Base> next;
};
```

### Pass by value

###### (1) `Base newBase(std::move(nextBase));`
* To take a unique pointer by value means that you are **transferring ownership** of the pointer to the function/object/etc in question.
* After newBase is constructed, nextBase is guaranteed to be empty.
  * You don't own the object, and you don't even have a pointer to it anymore. It's gone.
  * This is ensured because we take the parameter by value.
  * `std::move` doesn't actually move anything; it's just a fancy cast. `std::move(nextBase)` returns a `Base&&` that is an r-value reference to `nextBase`. That's all it does.

###### (2) `Base fromTemp(std::unique_ptr<Base>(new Base(...));`
* Because `Base::Base(std::unique_ptr<Base> n)` takes its argument by value rather than by r-value reference, C++ will automatically construct a temporary `std::unique_ptr<Base>` from the `Base&&` that we gave the function via `std::move(nextBase)`.
  * It is the construction of this temporary that actually moves the value from nextBase into the function argument n.

### Pass by non-const l-value reference

###### `Base newBase(nextBase);`
* The meaning of this is the same as the meaning of any other use of **non-const references: the function may or may not claim ownership of the pointer.**
* There is no guarantee that `nextBase` is empty. It may be empty; it may not.
* It really depends on what `Base::Base(std::unique_ptr<Base> &n)` wants to do.
* Because of that, it's not very evident just from the function signature what's going to happen; you have to read the implementation (or associated documentation).
* **Because of that, I wouldn't suggest this as an interface.**

### Pass by const l-value reference
###### `Base(std::unique_ptr<Base> const &n);`
* I don't show an implementation, because **you cannot move from a const&**.
* By passing a `const&`, you are saying that the function can access the Base via the pointer, but it cannot store it anywhere. It cannot claim ownership of it.
* it's always good to be able to hand someone a pointer and know that they cannot (without breaking rules of C++, like no casting away const) claim ownership of it. They can't store it. They can pass it to others, but those others have to abide by the same rules.

### Pass by r-value reference

###### `Base(std::unique_ptr<Base> &&n) : next(std::move(n)) {}`
* This is more or less identical to the "by non-const l-value reference" case. The differences are two things.
  * You can pass a temporary: `Base newBase(std::unique_ptr<Base>(new Base));`
* You must use `std::move` when passing non-temporary arguments.
  * The latter is really the problem. If you see this line: `Base newBase(std::move(nextBase));`
    * You have a reasonable expectation that, after this line completes, `nextBase` should be empty. It should have been moved from. After all, you have that std::move sitting there, telling you that movement has occurred.
    * **The problem is that it hasn't. It is not guaranteed to have been moved from. It may have been moved from, but you will only know by looking at the source code. You cannot tell just from the function signature.**
    * SINCE "`std::move` doesn't actually move anything; it's just a fancy cast!!"

### Recommendations

* **By Value**: If you mean for a function to claim ownership of a unique_ptr, take it by value.
* **By const l-value reference**: If you mean for a function to simply use the unique_ptr for the duration of that function's execution, take it by const&. Alternatively, pass a & or const& to the actual type pointed to, rather than using a unique_ptr.
* **By r-value reference**: If a function may or may not claim ownership (depending on internal code paths), then take it by &&. But I strongly advise **against** doing this whenever possible.


# Compatitble way for Legacy Code
* As your code base matures, you will need to say this less and less often.

```cpp
do_something( q.get() );          //retain ownership
do_something_else( q.release() ); //give up ownership
```

* Instead, when passing by reference, we don't have to worry about values being inadvertently copied and muddying ownership.
```cpp
void inc_baz( std::unique_ptr<foo> &p )
{
    p->baz++;
}
```
