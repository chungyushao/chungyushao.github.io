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
> * [stackoverflow 1](http://stackoverflow.com/questions/8114276/how-do-i-pass-a-unique-ptr-argument-to-a-constructor-or-a-function)
> * [stackoverflow 2](http://stackoverflow.com/questions/18282204/proper-way-of-transferring-ownership-of-a-stdvector-stdunique-ptr-int-t)

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


# [How do I pass a unique_ptr argument to a constructor or a function?](http://stackoverflow.com/questions/8114276/how-do-i-pass-a-unique-ptr-argument-to-a-constructor-or-a-function)

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

### (1) Pass by value

###### (1) `Base newBase(std::move(nextBase));`
* To take a unique pointer by value means that you are **transferring ownership** of the pointer to the function/object/etc in question.
* After newBase is constructed, nextBase is guaranteed to be empty.
  * You don't own the object, and you don't even have a pointer to it anymore. It's gone.
  * This is ensured because we take the parameter by value.
  * `std::move` doesn't actually move anything; it's just a fancy cast. `std::move(nextBase)` returns a `Base&&` that is an r-value reference to `nextBase`. That's all it does.

###### (2) `Base fromTemp(std::unique_ptr<Base>(new Base(...));`
* Because `Base::Base(std::unique_ptr<Base> n)` takes its argument by value rather than by r-value reference, C++ will automatically construct a temporary `std::unique_ptr<Base>` from the `Base&&` that we gave the function via `std::move(nextBase)`.
  * It is the construction of this temporary that actually moves the value from nextBase into the function argument n.

### (2) Pass by non-const l-value reference

###### `Base newBase(nextBase);`
* The meaning of this is the same as the meaning of any other use of **non-const references: the function may or may not claim ownership of the pointer.**
* There is no guarantee that `nextBase` is empty. It may be empty; it may not.
* It really depends on what `Base::Base(std::unique_ptr<Base> &n)` wants to do.
* Because of that, it's not very evident just from the function signature what's going to happen; you have to read the implementation (or associated documentation).
* **Because of that, I wouldn't suggest this as an interface.**

### (3) Pass by const l-value reference
###### `Base(std::unique_ptr<Base> const &n);`
* I don't show an implementation, because **you cannot move from a const&**.
* By passing a `const&`, you are saying that the function can access the Base via the pointer, but it cannot store it anywhere. It cannot claim ownership of it.
* it's always good to be able to hand someone a pointer and know that they cannot (without breaking rules of C++, like no casting away const) claim ownership of it. They can't store it. They can pass it to others, but those others have to abide by the same rules.

### (4) Pass by r-value reference

###### `Base(std::unique_ptr<Base> &&n) : next(std::move(n)) {}`
* This is more or less identical to the "by non-const l-value reference" case. The differences are two things.
  * You can pass a temporary: `Base newBase(std::unique_ptr<Base>(new Base));`
* You must use `std::move` when passing non-temporary arguments.
  * The latter is really the problem. If you see this line: `Base newBase(std::move(nextBase));`
    * You have a reasonable expectation that, after this line completes, `nextBase` should be empty. It should have been moved from. After all, you have that std::move sitting there, telling you that movement has occurred.
    * **The problem is that it hasn't. It is not guaranteed to have been moved from. It may have been moved from, but you will only know by looking at the source code. You cannot tell just from the function signature.**
    * SINCE "`std::move` doesn't actually move anything; it's just a fancy cast!!"

> Recommendations
> * **By Value**: If you mean for a function to claim ownership of a unique_ptr, take it by value.
> * **By const l-value reference**: If you mean for a function to simply use the unique_ptr for the duration of that function's execution, take it by const&. Alternatively, pass a & or const& to the actual type pointed to, rather than using a unique_ptr.
> * **By r-value reference**: If a function may or may not claim ownership (depending on internal code paths), then take it by `&&`. But I strongly advise **against** doing this whenever possible.

```cpp
#include <iostream>
#include <memory>
#include <cassert>

struct D {
	D() { std::cout << "D::D\n"; }
	~D() { std::cout << "D::~D\n"; }
	void bar() { std::cout << "D::bar\n"; }
};

std::unique_ptr<D> pass_through(std::unique_ptr<D> p)
{
	// a function consuming a unique_ptr
	// can take it by value or by rvalue reference
	p->bar();
	return p;
}

int main()
{
	std::cout << "ownership semantics\n";
	{
		auto p = std::make_unique<D>();
		auto q = pass_through(std::move(p));
		assert(!p);
		q->bar();
	}
}
```


# [Proper way of transferring ownership of a `vector<unique_ptr<int>>`](http://stackoverflow.com/questions/18282204/proper-way-of-transferring-ownership-of-a-stdvector-stdunique-ptr-int-t)

```cpp
class Foo {
public:
  Foo(vector<std::unique_ptr<int>> vec_);

private:
  vector<std::unique_ptr<int>> vec_;
};
```

* `std::unique_ptr<T>` is a non-copyable but movable type. Having a move-only type in a `std:vector<T>` make the `std::vector<T>` move-only, too.
* To have the compiler automatically move objects, you need to have an r-value for move-construction or move-assignment.
* class member `vec_` is an l-value. So the preferable way is

```cpp
Foo::Foo(std::vector<std::unique_ptr<int>> vec)
    : vec_(std::move(vec))
{
}
```

* Initialize list avoid first default-constructing the member and then move-assigning to it and, instead, move-constructs the member directly.
* Note, that you can construct objects of type Foo only from something which can be bound to an r-value, e.g

```cpp
int main() {
    Foo f0(std::vector<std::unique_ptr<int>>()); // OK
    std::vector<std::unique_ptr<int>> v;
    Foo f1(v); v// ERROR: using with an l-value
    Foo f2{v}; v// ERROR: using with an l-value
    Foo f3 = v; // ERROR: using with an l-value
    Foo f4(std::move(v)); // OK: pretend that v is an r-value
}
```

# Unique pointer and polymorphism

```cpp
#include <iostream>
#include <vector>
#include <memory>

struct B {
	virtual void bar() { std::cout << "B::bar\n"; }
	virtual ~B() = default;
};

struct D : B {
	D() { std::cout << "D::D\n"; }
	~D() { std::cout << "D::~D\n"; }
	void bar() override { std::cout << "D::bar\n"; }
};

int main()
{
	std::cout << "runtime polymorphism\n";
	{
		std::unique_ptr<B> p = std::make_unique<D>();
		p->bar();
	}

	std::cout << "runtime polymorphism -- in container\n";
	{
		std::unique_ptr<B> p = std::make_unique<D>();
		std::vector<std::unique_ptr<B>> v;
		v.push_back(std::make_unique<D>());
		v.push_back(std::move(p));
		v.emplace_back(new D);
		for(auto& e: v) e->bar();
	}
}
```
