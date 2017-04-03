---
layout: post
title: "C++ OO1: class"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel

# Struct and Class

The only differences between structures and classes in C++ is that structure members default to `public` access and class members default to `private` access; and structure default to `public` inheritance, whereas classed to `private` inheritance.

# Class basics
* Data member typically should be initialize by the class's constructor as **there is no default initialization for fundamental-type data members**
* Even though a member function declared in a class definition may be defined outside that class definition using `::` (scope resolution operator), that member function is still within that class's scope.
* If a member function is defined in a class's body, the member function is implicitly declared `inline`.
* Object didn't contain member functions, instead, only data. The compiler creates one copy of the member functions separate from all object of the class. All object of the class share this one copy. The function code is non-modifiable and can be shared among all objects of the class.
* Variables declared in a member function have `block scope` and are only known to the function.
  * If a member function defines a variable with the same name as a variable with `class scope`, the `class scope` variables is hidden in the function by the block-scope variable. (Can still accesses through scope resolution operator `::`)

# Constructor
* A constructor that defaults all its argument is also a default constructor -- that is, a constructor that can be invoked with no arguments. (At most one default constructor per class).

##### [C++11]List initializer
```cpp
//original
Time t1; // all arguments defaulted
Time t2(2); // hour specified; minute and second defaulted
Time t3(21,34); // hour and minute specified; second defaulted
Time t4(12,25,42); // hour, minute and second specified

//C++11 style 1 (Preferred)
Time t2{2}; // hour specified; minute and second defaulted
Time t3{21,34}; // hour and minute specified; second defaulted
Time t4{12,25,42}; // hour, minute and second specified

//C++11 style 2
Time t2 = {2}; // hour specified; minute and second defaulted
Time t3 = {21,34}; // hour and minute specified; second defaulted
Time t4 = {12,25,42}; // hour, minute and second specified
```
##### [C++11]Overloaded constructor
* Overloaded constructor typically allow objects to be initialize with different types and/or numbers of arguments,
* To overload a constructor, provide in the class definition a prototype for each version of the constructor. (Also applies to the class's member functions)

##### [C++11]Delegating constructor
* Constructor can call other constructors in the same class. The calling constructor is known as delegating constructor.

```cpp
Time(); // default hour, minute and second to 0
Time(int); // initialize hour; default minute and second to 0
Time(int,int); // initialize hour and minute; default second to 0
Time(int,int,int); // initialize hour, minute and second
```

```cpp
Time::Time() : Time(0, 0, 0) {}
// delegate to Time( int, int, int )
Time::Time(int hour) : Time(hour, 0, 0) {}
// delegate to Time( int, int, int )
Time::Time(int hour, int minute) : Time(hour, minute, 0) {}
// delegate to Time( int, int, int )
```
* A constructor must be a non-const membeer function, but it can still be used to initialize a `const` object.



# Destructor
* The destructor itself does not actually release the object's memory--it performs termination housekeeping before the object's memory is reclaimed, so the memory may be reused to hold new objects.
* Every class has one destructor. If you do not explicitly define, the compiler defines an empty destructor.
* Generally destructor calls are made in the reverse order of the corresponding constructor calls, but the storage classes of objects can alter the order in which destructors are called.

##### Objects in global scope
* Constructors are called (the order not guaranteed) before any other function (including `main`).
* Destructors are called when `main` terminates.
* `exit`forces a program to terminate immediately and does not execute the destructors of local objects.
  * indicate a fatal unrecoverable error
* `abort` forces the program to terminate immediately, without allowing the destructors of **any objects** to be called.
  * indicate *abnormal termination*

##### Local objects
* Constructors and destructors are called each time execution enters and leaves **the scope of the object**
* Destructors are NOT called for local objects if the program terminates with `exit` or `abort`.

##### Static local objects
* Constructor is called only once, when execution first reaches the point where the object is defined.
* Destructor is called when `main` terminates or the program calls `exit`
* Destructors are not called when program terminates with `abort`

* Declare a constructor or destructor `const` is a compilation error.

# Composition
* Object as members of classes
* "has-a" relationship--a class can have objects of other classes as members

# `friend` functions and `friend` class
* A `friend` function of a class is a **non-member** function that has the right to access the public and non-public class members.
* standalone functions, entire classes or member function of other classes may be declared to be `friend`s of another class.
* `friend` is not symmetric--if class A is a friend of class B, B might not be a friend of A.
* `friend` is not transitive--if class A is friend of class B and class B is friend of class C, class A might not be a friend of class C!

```cpp
// Fig. 9.22: fig09_22.cpp  
// Friends can access private members of a class.
#include <iostream>
using namespace std;

// Count class definition
class Count {
   friend void setX( Count &, int ); // friend declaration
public:
   // constructor
   Count() : x(0) {} // initialize x to 0
   void print() const {
      cout << x << endl;
   } // end function print
private:
   int x; // data member
}; // end class Count

// function setX can modify private data of Count
// because setX is declared as a friend of Count (line 9)
void setX(Count &c, int val) {
   c.x = val; // allowed because setX is a friend of Count
} // end function setX

int main() {
   Count counter; // create Count object
   cout << "counter.x after instantiation: ";
   counter.print();
   setX( counter, 8 ); // set x using a friend function
   cout << "counter.x after call to setX friend function: ";
   counter.print();
} // end main
```

##### Declaring
* To declare all member function of `ClassTwo` as friends of `ClassOne`, place `friend class ClassTwo;` in the definition of `ClassOne`.
* Even though the prototype for `friend` functions appear in the class definition, friends are not member functions.
* access notation (`public`, `protected`, `private`) are not relevant to `friend` declaration, so `friend` can be placed anywhere in a class definition.

# `this`
* Every `object` has access to its own address through `this` pointer.
* The `this` pointer is not part of the object itself--i.e., the memory occupied by the `this` pointer is not reflected in the `sizeof` operation.
* `this` is passed by compiler as an implicit argument to each of the object's non-static member functions.
* In `const` object/member function, `this` will be `const` pointer, whereas in non-const object/member function, `this` will be normal pointer.
* A static member function does not have a `this` pointer, because `static` data members and `static` member functions exist independently of **any objects of a class**. `this` must refer to a specific object of the class, which might not exist when a `static` member functions is called.

##### When to use `this`?
1. Avoid naming collisions

```cpp
void Time::setHour(int hour) {
  if (hour >= 0 && hour < 24) {
    this->hour = hour;
  } else {
    throw invalid_argument("wrong hour value");
  }
}
```
> Never hide data members with local variable names will be better practice.

2. Enable cascaded function calls
* through returning `this`!
