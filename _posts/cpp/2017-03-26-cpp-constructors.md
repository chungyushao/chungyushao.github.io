---
layout: post
title: "C++ Constructors"
excerpt: "Default, Copy, Converting"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [Coursera](https://www.coursera.org/learn/cpp-chengxu-sheji)
> * [cppreference.com](http://en.cppreference.com/w/cpp/language/copy_constructor)


# Constructor
* A default constructor is a constructor which can be called with no arguments (either defined with an empty parameter list, or with default arguments provided for every parameter)
* Behavior of inheritance:

```cpp
struct A {
	int x_;
	A(int x = 1) : x_(x) {}; // user-defined default constructor
};
struct B : A {}; //B::B() is implicitly-defined, calls A::A()
struct C { A a; }; //C::C() is implicitly-defined, calls A::A()
struct D : A {
	D(int y) : A(y) {};
};
struct E : A {
	E(int y) : A(y) {};
	E() = default; // explicitly defaulted, calls A::A()
};

int main() {
	A a; B b; C c;
	//D d; //D::D() is not declared because another constructor exists
	D(5); E e;
}
```

# Copy Constructor
* A copy constructor of class `T` is a non-template constructor whose first parameter is `T&`, `const T&`, `volatile T&`, or `const volatile T&`

* The copy constructor is called whenever an object is initialized **from another object of the same type**
  * (unless overload resolution selects a better match or the call is elided), which includes
  * initialization: `T a = b;` or `T a(b);`, where `b` is of type `T`;
  * function argument passing: `f(a)`;, where `a` is of type `T` and `f` is `void f(T t)`;
  * function return: `return a`; inside a function such as `T f()`, where `a` is of type `T`, **which has no move constructor.**
* If no user-defined copy constructors are provided for a class type (struct, class, or union), the compiler will always declare a copy constructor as a non-explicit inline public member of its class.
* A class can have multiple copy constructors, e.g. both `T::T(const T&)` and `T::T(T&)`
* If some user-defined copy constructors are present, the user **may** still force the generation of the implicitly declared copy constructor with the keyword `default`.
  * `class_name ( const class_name & ) = default;`
* 不允许有形如 `T::T( T )`的构造函数。

```cpp
#include <stdio.h>
using namespace std;

struct Dummy {
	Dummy(int v = 0) : val_(v) { printf("Constructor\n"); }
	Dummy(Dummy& d) : val_(d.val_){ printf("Copy Constructor\n"); }
	Dummy(const Dummy& d) : val_(d.val_){ printf("Const Copy Constructor\n"); }
	int val_;
};

Dummy returnDummy() {
	printf("Inside returnDummy\n");
	Dummy d4(4);
	return d4;
}

void passDummy(Dummy d) {
	printf("Inside passDummy\n");
}

int main () {
	Dummy d1(1); //Constructor
	Dummy d2(d1); //Copy Constructor
	Dummy d3 = d2; //Copy Constructor
	printf("%d\n", returnDummy().val_); //Constructor, move constructor opt out copy constructor here?
	passDummy(d3); //Copy Constructor
}
```

# Converting Constructor
* It is said that a converting constructor specifies an implicit conversion from the types of its arguments (if any) to the type of its class.
* A constructor that is not declared with the specifier explicit and which can be called **with a single parameter** (until C++11) is called a converting constructor.
* Converting constructors are also considered during copy initialization, as part of user-defined conversion sequence.
* Implicitly-declared and user-defined non-explicit copy constructors and move constructors are converting constructors.

```cpp
#include <stdio.h>
using namespace std;

struct A {
    A() { printf("A()\n"); }         // converting constructor (since C++11)
    A(int) { printf("A(int)\n"); }      // converting constructor
    A(int, int) { printf("A(int, int)\n"); } // converting constructor (since C++11)
};

struct B {
    explicit B() { printf("B()\n"); }
    explicit B(int) { printf("B(int)\n"); }
    explicit B(int, int) { printf("B(int, int)"); }
};

int main()
{
    A a1 = 1;      // OK: copy-initialization selects A::A(int)
    A a2(2);       // OK: direct-initialization selects A::A(int)
    A a3{4, 5};    // OK: direct-list-initialization selects A::A(int, int)
    A a4 = {4, 5}; // OK: copy-list-initialization selects A::A(int, int)
    A a5 = (A)1;   // OK: explicit cast performs static_cast, direct-initialization

//  B b1 = 1;      // error: copy-initialization does not consider B::B(int)
    B b2(2);       // OK: direct-initialization selects B::B(int)
    B b3{4, 5};    // OK: direct-list-initialization selects B::B(int, int)
//  B b4 = {4, 5}; // error: copy-list-initialization selected an explicit constructor
                   //        B::B(int, int)
    B b5 = (B)1;   // OK: explicit cast performs static_cast, direct-initialization
    B b6;          // OK, default-initialization
    B b7{};        // OK, direct-list-initialization
//  B b8 = {};     // error: copy-list-initialization selected an explicit constructor
                   //        B::B()
}
```
