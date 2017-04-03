---
layout: post
title: "C++ exception handling"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel


# Basics

* The fundamental idea is to separate **detection** of an error (which should be done in a called function) from the **handling** of an error (which should be done in the calling function) while **ensuring that a detected error cannot be ignored**

* The basic idea is that if a function finds an error that it cannot handle, it does not **return** normally; instead, it **throws** an exception indicating what went wrong. Any direct or indirect caller can **catch** the exception.

* `error()` is supposed to terminate the program after getting its message written.

* A typical exception class that derives from the runtime_error class **defines only a constructor** that passes an error-message string to the base-class runtime_error constructor.

* Exceptions may surface through explicitly mentioned code in a try block,
  through calls to other functions and through deeply nested function calls
  initiated by code in a try block.
* An exception parameter should always be declared as **a reference to the type of `exception`** the catch handler can process
  * prevents copying the exception object when it’s caught
  * allows a catch handler to properly catch derived-class exceptions.
* The operand of a `throw` can be of any type (but it must be copy constructable)
  * `throw x > 5`, `throw 5` is valid!
* Exception handling is not designed to process errors associated with asynchronous events


```cpp
// DivideByZeroException.h
#include <stdexcept> // stdexcept header contains runtime_error

class DivideByZeroException : public std::runtime_error {
public:
   // constructor specifies default error message
   DivideByZeroException()
      : std::runtime_error("attempted to divide by zero") {}
};
```



# Stack unwinding
Unwinding the function call stack means that the function in which the exception was not caught terminates, all local variables that have completed intitialization in that function are destroyed and control returns to the statement that originally invoked that function. If a try block encloses that statement, an attempt is made to catch the exception. If a try block does not enclose that statement, stack unwinding occurs again.


# Rethrowing
 An exception handler, upon receiving an exception, can release the resource then notify its caller than an exception occurred by rethrowing the exception via the statement `throw;`

```cpp
try
{
   cout << "  Function throwException throws an exception\n";
   throw exception(); // generate exception
} // end try
catch ( exception & ) // handle exception
{
   cout << "  Exception handled in function throwException"
      << "\n  Function throwException rethrows exception";
   throw; // rethrow exception for further processing
} // end catch
```

# Exceptions and inheritance
* If a catch handler catches a reference to an exception object of a base-class type, it also can catch a reference to all objects of classes publicly derived from that base class
* catch pointers or references to **base-class** exception objects
* catching pointers or references to de- rived-class exception objects individually is error prone

# [c++11] `noexcept`
* if a function does not throw any exceptions and does not call any functions that throw exceptions, you should explicitly state that.
  * add `noexcept` to the right of the function’s parameter list in both the prototype and the definition.
  * For a const member function, place `noexcept` after `const`.
* If a function that’s declared noexcept calls another function that throws an exception or executes a throw statement, the program terminates.

# Constructor, destructor
* Before an exception is thrown by a constructor, destructors are called for any member objects whose constructors have run to completion as part of the object being constructed.
* When an exception is thrown from the constructor for an object that’s created in a new expression, the dynamically allocated memory for that object is released.

# Potential resource leak
* An exception could preclude the operation of code that would normally release a resource (such as memory or a file), thus causing a resource leak that prevents other programs from acquiring the resource.
* initialize a local object to acquire the resource. When an exception occurs, the destructor for that object will be invoked and can free the resource.


# Handling `new` Failures Using Function set_new_handler

```cpp
#include <iostream>
#include <new> // set_new_handler function prototype
#include <cstdlib> // abort function prototype
using namespace std;

// handle memory allocation failure
void customNewHandler()                  
{                                        
   cerr << "customNewHandler was called";
   abort();                              
} // end function customNewHandler       

// using set_new_handler to handle failed memory allocation
int main()
{
   double *ptr[ 50 ];

   // specify that customNewHandler should be called on
   // memory allocation failure                         
   set_new_handler( customNewHandler );                 

   // aim each ptr[i] at a big block of memory; customNewHandler will be
   // called on failed memory allocation
   for ( int i = 0; i < 50; ++i )
   {
      ptr[ i ] = new double[ 50000000 ]; // may throw exception
      cout << "ptr[" << i << "] points to 50,000,000 new doubles\n";
   } // end for
}  // end main
```

# [c++11] `unique_ptr`
* If an exception occurs after successful memory allocation but before the delete statement executes, a memory leak could occur.
* Class template `unique_pt`r provides overloaded operators `*` and `->` so that a `unique_ptr` object can be used just as a regular pointer variable is.
* The class is called unique_ptr because only one unique_ptr at a time can own a dynami- cally allocated object. By using its overloaded assignment operator or copy constructor, a unique_ptr can transfer ownership of the dynamic memory it manages.

```cpp
// Demonstrating unique_ptr.
#include <iostream>
#include <memory> // for unique_ptr
#include "Integer.h"
using namespace std;

// use unique_ptr to manipulate Integer object
int main()
{
   cout << "Creating a unique_ptr object that points to an Integer\n";

   // "aim" unique_ptr at Integer object
   unique_ptr< Integer > ptrToInteger( new Integer( 7 ) );

   cout << "\nUsing the unique_ptr to manipulate the Integer\n";
   ptrToInteger->setInteger( 99 ); // use unique_ptr to set Integer value

   // use unique_ptr to get Integer value
   cout << "Integer after setInteger: " << ( *ptrToInteger ).getInteger()
      << "\n\nTerminating program" << endl;
}  // end main

```

# Standard Library Exception Hierarchy
![stdexcept]({{site.url}}/images/cpp/stdexcept.png)
* The standard exception hierarchy is a good starting point for creating exceptions. You can build programs that can throw standard exceptions, throw exceptions derived from the standard exceptions or throw your own exceptions not derived from the standard exceptions.
