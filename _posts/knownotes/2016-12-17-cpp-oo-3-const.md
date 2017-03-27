---
layout: post
title: "C++ OO3: const"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---


> * Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel
> * [](http://duramecho.com/ComputerInformation/WhyHowCppConst.html)

# `const` objects
* `const` specify an object is NOT modifiable and any attempt to modify the object should result compilation error.
* Compiler can perform optimizations on constants that can't be performed on non-const variables.

# `const` member functions
* C++ disallows member function calls for `const` object unless the member functions themselves are declared `const`
* Declare a constructor or destructor `const` is a compilation error.

# Position of `const`
> Rules of thumbs
> * `const` applies to whatever is on its immediate left
> * If there is nothing there in the left, it applies to whatever is its immediate right.

* `const int * Constant2`: Constant2 is a variable pointer to a constant integer.
  * If there is nothing there in the left, it applies to whatever is its immediate right.
* `int const * Constant2`: Constant2 is a variable pointer to a constant integer.
* `int * const Constant3`: Constant3 is constant pointer to a variable integer
* `int const * const Constant4`: Constant4 is constant pointer to a constant integer.


# `const static`?
* Declaring a `static` member function `const` is a compilation error.
* The `const` qualifier indicates that a function can't modify the contents of the object on which it operates, but `static` member functions exist and operate independently of any objects of the class.

# `const_cast` Operator
* In general, a `const_cast` should be used **only when it's known in advance** that the original data is not constant.
* `const_cast` cast away `const` or `volatile` qualification.
  * You declare a variable with `volatile` when you expect the variable to be modified by hardware or other programs not known to compilers.
  * Prevent compilers from optimizing the variable.
* Operations involving `const_cast` are typically hidden in a member function's implementation. The user might not be aware that a member is being modified underneath.
  * Example:
```cpp
#include <iostream>
#include <cstring> // contains prototypes for functions strcmp and strlen
#include <cctype> // contains prototype for function toupper
using namespace std;

// returns the larger of two C strings
const char *maximum( const char * first, const char * second )
{
   return ( strcmp( first, second ) >= 0 ? first : second );
} // end function maximum

int main()
{
   char s1[] = "hello"; // modifiable array of characters
   char s2[] = "goodbye"; // modifiable array of characters

   // const_cast required to allow the const char * returned by maximum
   // to be assigned to the char * variable maxPtr
   char * maxPtr = const_cast< char * >( maximum( s1, s2 ) );

   cout << "The larger string is: " << maxPtr << endl;

   for ( size_t i = 0; i < strlen( maxPtr ); ++i )
      maxPtr[ i ] = toupper( maxPtr[ i ] );

   cout << "The larger string capitalized is: " << maxPtr << endl;
} // end main
```

# `mutable` class members

* If a data member should always be modifiable, C++ provides the storage-class specifier `mutable` as an alternative to `const_cast`.
* A `mutable` data member is always modifiable, **even in a `const` member function or `const` object**
