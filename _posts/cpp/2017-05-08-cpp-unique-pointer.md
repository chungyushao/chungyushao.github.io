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
> * [C++11: unique_ptr](http://www.drdobbs.com/cpp/c11-uniqueptr/240002708)
> * [stackoverflow](http://stackoverflow.com/questions/8114276/how-do-i-pass-a-unique-ptr-argument-to-a-constructor-or-a-function)

#
* The class template `unique_ptr<T>` manages a pointer to an object of type `T`.
* You will usually construct an object of this type by calling new to create an object in the `unique_ptr` constructor: `std::unique_ptr<foo> p( new foo(42) );` or `std::make_unique<foo>(42)` as in C++14.
* After calling the constructor, you can use the object very much like a raw pointer.
* The `*` and `->` operators work exactly like you would expect, and are very efficient — usually generating nearly the same assembly code as raw pointer access.

# Benefit
* automatic destruction of the contained object when the pointer goes out of scope.
* You don't have to track every possible exit point from a routine to make sure the object is freed properly — it is done automatically.
* And more importantly, it will be destroyed if your function exits via an exception.
* With the addition of rvalue references and move semantics `unique_ptr` objects can be stored in containers, work properly when containers are resized or moved, and will still be destroyed when the container is destroyed.

# Ownership

```cpp
std::unique_ptr<foo> q( new foo(42) );
v.push_back( q );
```
* Who owns the pointer? Will the container destroy it at some point in its lifetime? Or is it still my job do so?
* Actually you won't be able to compile here.
* we are only allowed to have one copy of the pointer — unique ownership rules apply.

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
* By passing a const&, you are saying that the function can access the Base via the pointer, but it cannot store it anywhere. It cannot claim ownership of it.
* it's always good to be able to hand someone a pointer and know that they cannot (without breaking rules of C++, like no casting away const) claim ownership of it. They can't store it. They can pass it to others, but those others have to abide by the same rules.

### Pass by r-value reference

###### `Base(std::unique_ptr<Base> &&n) : next(std::move(n)) {}`
* This is more or less identical to the "by non-const l-value reference" case. The differences are two things.
  * You can pass a temporary: `Base newBase(std::unique_ptr<Base>(new Base));`
* You must use `std::move` when passing non-temporary arguments.
  * The latter is really the problem. If you see this line: `Base newBase(std::move(nextBase));`
    * You have a reasonable expectation that, after this line completes, `nextBase` should be empty. It should have been moved from. After all, you have that std::move sitting there, telling you that movement has occurred.
    * **The problem is that it hasn't. It is not guaranteed to have been moved from. It may have been moved from, but you will only know by looking at the source code. You cannot tell just from the function signature.**

### Recommendations

* **By Value**: If you mean for a function to claim ownership of a unique_ptr, take it by value.
* **By const l-value reference**: If you mean for a function to simply use the unique_ptr for the duration of that function's execution, take it by const&. Alternatively, pass a & or const& to the actual type pointed to, rather than using a unique_ptr.
* **By r-value reference**: If a function may or may not claim ownership (depending on internal code paths), then take it by &&. But I strongly advise against doing this whenever possible.


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
