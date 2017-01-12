---
layout: post
title: "C++ OO2: Inheritance"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---

> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel


# Multiple inheritance

* Great care is required in the design of a system to use multiple inheritance properly; it should not be used when single inheritance and/or composition will do the job.
* The base-class constructors are called in **the order that the inheritance is specified**, not the order in which their constructors are mentioned.
* If the base-class constructors are not explicitly called in the member-initializer list, their default constructors will be called implicitly.

###### "diamond inheritance"
```
            basic_istream
basic_ios /               \basic_iostream
          \ basic_ostream /          
```

* A potential problem exists in diamond inheritance: `basic_iostream` could contain two copies of themembers of class `basic_ios` -- one inherited via `basic_istream` and one inherited via `basic_ostream`. Compiler would not know which version of the members to use.
* Resolve: `virtual`

# Eliminating Duplicate Subobjects with `virtual` base-class inheritance
* When a base class is inherited as `virtual`, only one subobject will appear in the derived class -- a process called `virtual` base-class inheritance.

```cpp
// Fig. 23.21: fig23_21.cpp
// Using virtual base classes.
#include <iostream>
using namespace std;

// class Base definition
class Base {
public:
   virtual void print() const = 0; // pure virtual
}; // end class Base

// class DerivedOne definition **virtual inheritance!!
class DerivedOne : virtual public Base {
public:
   // override print function
   void print() const {
      cout << "DerivedOne\n";
   } // end function print
}; // end DerivedOne class

// class DerivedTwo definition **virtual inheritance!!
class DerivedTwo : virtual public Base {
public:
   // override print function
   void print() const {
      cout << "DerivedTwo\n";
   } // end function print
}; // end DerivedTwo class

// class Multiple definition
class Multiple : public DerivedOne, public DerivedTw {
public:
   // qualify which version of function print
   void print() const {
      DerivedTwo::print();
   } // end function print
}; // end Multiple class

int main() {
   Multiple both; // instantiate Multiple object
   DerivedOne one; // instantiate DerivedOne object
   DerivedTwo two; // instantiate DerivedTwo object

   // declare array of base-class pointers and initialize
   // each element to a derived-class type
   Base *array[ 3 ];
   array[ 0 ] = &both;
   array[ 1 ] = &one;
   array[ 2 ] = &two;

   // polymorphically invoke function print
   for ( int i = 0; i < 3; ++i )
      array[ i ]->print();
} // end main
```
