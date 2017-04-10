---
layout: post
title: "Header file inclusion rules"
excerpt: "cyclic dependency, incomplete type, forward declaration"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [C++ Header File Include Patterns](http://www.eventhelix.com/realtimemantra/HeaderFileIncludePatterns.htm#.WOG9cBLyscJ)
> * [stackoverflow by Luc Touraille](http://stackoverflow.com/questions/553682/when-can-i-use-a-forward-declaration)

# Header File Inclusion Rules
* A header file should be included only when a forward declaration would not do the job.
* The header file should be so designed that the order of header file inclusion is not important.
  * This is achieved by making sure that `x.h` is the first header file in `x.cpp`
* The header file inclusion mechanism should be tolerant to duplicate header file inclusions.

# Forward declaration and cycle dependency
* **Put yourself in the compiler's position: when you forward declare a type, all the compiler knows is that this type exists**;
  * it knows nothing about its size, members, or methods.
  * This is why it's called an incomplete type.

#### What you can do with an incomplete type

* Declare a member to be a pointer or a reference to the incomplete type

```cpp
class X; //forward declaration

class Foo {
    X * pt1;
    X & pt2;
};
```

* **Declare** functions or methods which accept/return incomplete types:

```cpp
void f1(X);
X    f2();
```

* **Define** functions or methods which accept/return pointers/references to the incomplete type **(but without using its members)**

```cpp
void f3(X*, X&) {}
X&   f4()       {}
X*   f5()       {}
```

#### What you canNOT do with an incomplete type

* Use it as a base class
  * `class Foo : X {} // compiler error!`

* Use it to declare a member:

```cpp
class Foo {
    X m; // compiler error!
};
```

* **Define** functions or methods using this type

```cpp
void f1(X x) {} // compiler error!
X    f2()    {} // compiler error!
```

* Trying to dereference a variable with incomplete type: ex: Use its methods or fields

```cpp
class Foo {
    X * m;            
    void method()            
    {
        m->someMethod();      // compiler error!
        int i = m->someField; // compiler error!
    }
};
```

# How about template?
* there is no absolute rule: whether you can use an incomplete type as a template parameter is **dependent on the way the type is used in the template.**
  * `std::vector<T>` requires its parameter to be a complete type
  * while `boost::container::vector<T>` does not.

# So what?
> The main rule is that **you can only forward-declare classes whose memory layout (and thus member functions and data members) do not need to be known in the file you forward-declare it**  --- Timo Geusch

* Image the [case by Roosh](http://stackoverflow.com/questions/625799/resolve-header-include-circular-dependencies):

```cpp
// file: A.h
class A {
  B b_;
};

// file: B.h
class B {
  A a_;
};

// file main.cc
#include "A.h"
#include "B.h"
int main(...) {
  A a;
}
```

* When you are compiling the .cc file, you need to allocate space for object `A`.
  * (remember that the .cc and not the .h is the unit of compilation)
* So, well, how much space then? Enough to store `B`!
* What's the size of `B` then? Enough to store `A`! ==> circular reference
* You can break it by allowing the compiler to instead reserve as much space as it knows about upfront - **pointers and references**
  * for example, will always be 32 or 64 bits (depending on the architecture)
* Solution:

```cpp
// file: A.h
class B;
class A {
  B* b_; // or any of the other variants.
};

// file: B.h
#include "A.h"
class B {
  A a_;
};

// main.cc
#include "A.h"
#include "B.h"
int main (...) {
  A a; B b;
}
```

# Example I met: two classes need each other

```cpp
//a.h
struct B;
struct A {
	A() {}
	void setB(B* b) { b_ = b; }
	void ACallB();
	void helloFromA();
	B* b_;
};

//a.cpp
#include <iostream>
#include "A.h"
#include "B.h"

void A::ACallB() { b_->helloFromB(); }
void A::helloFromA() { std::cout << "Hello form A\n"; }

//-----------------------------------------------------
//b.h
struct A;
struct B {
	B() {}
	void setA(A* a) { a_ = a; }
	void BCallA();
	void helloFromB();
	A* a_;
};

//b.cpp
#include <iostream>
#include "B.h"
#include "A.h"

void B::BCallA() { a_->helloFromA(); }
void B::helloFromB() { std::cout << "Hello form B\n"; }

//-----------------------------------------------------

//main.cpp
#include "a.h"
#include "b.h"

int main() {
	A* a = new A();
	B* b = new B();
	a->setB(b); b->setA(a);
	a->ACallB();
	b->BCallA();
	delete a; delete b;
}
```
