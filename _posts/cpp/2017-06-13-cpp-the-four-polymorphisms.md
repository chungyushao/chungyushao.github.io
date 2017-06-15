---
layout: post
title: "C++ The Four Polymorphisms"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [The Four Polymorphisms](http://www.catonmat.net/blog/cpp-polymorphism/)


These polymorphisms also go by different names in C++,

* **Subtype polymorphism** is also known as runtime polymorphism.
* **Parametric polymorphism** is also known as compile-time polymorphism.
* **Ad-hoc polymorphism** is also known as overloading.
* **Coercion** is also known as (implicit or explicit) casting.

# Subtype polymorphis
> the ability to use derived classes through base class pointers and references.

* Subtype polymorphism is also called runtime polymorphism for a good reason.
* The resolution of polymorphic function calls **happens at runtime** through an indirection via the **virtual table**.
  * Another way of explaining this is that compiler does not locate the address of the function to be called at compile-time, instead when the program is run, the function is called by dereferencing the right pointer in the virtual table.

# Parametric Polymorphism (Compile-Time Polymorphism)
> Parametric polymorphism provides a means to execute the same code for any type.

* In C++ parametric polymorphism is implemented via templates.
* Since parametric polymorphism happens at compile time, it's also called compile-time polymorphism.

# Ad-hoc Polymorphism (Overloading)
> Ad-hoc polymorphism allows functions with the same name act differently for each type.

# Coercion Polymorphism (Casting)
> Coercion happens when an object or a primitive is cast into another object type or primitive type.

* Explicit casting happens when you use C's type-casting expressions, such as `(unsigned int *)` or `(int)` or C++'s `static_cast`, `const_cast`, `reinterpret_cast`, or `dynamic_cast`.
* Coercion also happens if the constructor of a class isn't explicit. If you made the constructor of A explicit, that would no longer be possible.

```cpp
include <iostream>

class A {
 int foo;
public:
 A(int ffoo) : foo(ffoo) {}
 void giggidy() { std::cout << foo << std::endl; }
};

void moo(A a) {
 a.giggidy();
}

int main() {
 moo(55);     // prints 55  ==> 55 got cast to A
}
```

> It's always a good idea to make your constructors explicit to avoid accidental conversions.

* If a class defines **conversion operator** for type `T`, then it can be used anywhere where type `T` is expected.

```cpp
#include <iostream>


class CrazyInt {
 int v;
public:
 CrazyInt(int i) : v(i) {}
 operator int() const { return v; } // conversion from CrazyInt to int
};


void print_int(int a) {
 std::cout << a << std::endl;
}

int main() {
 CrazyInt b = 55;
 print_int(999);    // prints 999
 print_int(b);      // prints 55
}
```

* Subtype polymorphism is actually also coercion polymorphism because the derived class gets converted into base class type.
