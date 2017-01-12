---
layout: post
title: "C++ pointer"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---


> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel




# Basics
* `double *xPtr, *yPtr;` Each pointer must be declared with the prefix `*`
* A pointer contains the memory address of a variable that in turn contains a specific value.
* Referencing a value through a pointer is called indirection.


# `nullptr` and `NULL`
* Pointer should be initialized as `nullptr` (c++11) to prevent pointing to unknown area of memory.
* `NULL` is defined in several standard library headers to represents value 0.
* Initialize pointer to `NULL` is actually making the pointer to integer 0.
  * `0` is the only integer that can be assigned to a pointer without casting.


# `&`, Address operator
* `&` is a unary operator that **obtains the memory address of its operand**

```cpp
int y = 5;
int *yptr = nullptr;
yptr = &y;
```

* When declaring a reference, the `&` becomes part of the type, not operand.
* The operand of the address operator must be an lvalue -- the address operator cannot be applied to constants or to expressions that result in temporary values (like the results of calculation)

# `*`, Indirection operator
* or called dereference operator, returns an lvalue representing the object to which its pointer operand points
* The dereferencing pointer may also be used to receive an input values as `cin >> *yPtr`


# Passing arguments
##### pass-by-value
##### pass-by-reference
* Enable a programes to pass large data objects to a function and avoid the overhead of passing the objects by value.

##### pass-by-reference with pointers
* When calling a function with an argument that should be modifed, the **address** of the argument is passed.
* A function receiving an address as an argument must define a pointer parameter to receive the address.
* In C++, all arguments are **always** pssed by value. Passing a variable by reference with a pointer **does not actually pass anything by reference**, instead, a pointer to that variable is passed by value and is copied into the function's corresponding pointer parameter, and being dereferencing for further use.

```cpp
#include <iostream>
using namespace std;

void cubeByReference( int * ); // prototype

int main()
{
   int number = 5;

   cout << "The original value of number is " << number;

   cubeByReference( &number ); // pass number address to cubeByReference

   cout << "\nThe new value of number is " << number << endl;
} // end main

// calculate cube of *nPtr; modifies variable number in main
void cubeByReference( int *nPtr )
{
   *nPtr = *nPtr * *nPtr * *nPtr; // cube *nPtr
} // end function cubeByReference
```

# Declaring Built-in array parameters
* The compiler does not diffrentiate between a function that receives a pointer and a function that receives a built-in array.
* For clarity, you should use the `[]` notation when the function expects a built-in array argument.

```cpp
int sumElements(const int values[], const size_t numberOfElements)
int sumElements(const int* values, const size_t numberOfElements)
```
# `const`
> **Principle of least privilege**: Always give a function enough acess to the data in its parameters to accomplish is specified task, but not more.

* `int * ptr`, `const int* ptr`, `int* const ptr`, `const int* const ptr`: Non/constant pointer to non/constant data
##### Nonconstant pointer to nonconstant data
* just `int * ptr`

##### Nonconstant pointer to constant data
* `const int* ptr`, read from right to left: a pointer to an integer constant

```cpp
int x = 5, y;
const int * ptr = &x;
cout << *ptr << endl;
*ptr = 7; // error: *ptr is const; cannot assign new value
ptr = &y; // pass! pointer not constant, can assign to other address
```


##### Constant pointer to nonconstant data
* `int* const ptr`, read from right to left: ptr is a constant pointer to nonconstant integer.

```cpp
int x = 5, y;
int *const ptr = &x;
cout << *ptr << endl;
*ptr = 7; // pass! data not constant, can be modified to other value!
ptr = &y; // error: ptr is const; cannot assign new address
```


##### Constant pointer to constant data
* `const int *const ptr`

```cpp
int x = 5, y;
const int *const ptr = &x;
cout << *ptr << endl;
*ptr = 7; // error: *ptr is const; cannot assign new value
ptr = &y; // error: ptr is const; cannot assign new address
```

# `sizeof`
* When applied to a built-in array's name, the `sizeof` operator returns the total number of bytes in the array as a valuel of `size_t`.
* When applied to a pointer parameter in a function that receives built-in array as an argument, the sizeof operator returns *the size of the pointer in bytes*, not the built-in array's size.

```cpp
#include <iostream>
using namespace std;

size_t getSize( double * ); // prototype
size_t getSize2( double [] ); // prototype

int main()
{
   double numbers[ 20 ]; // 20 doubles; occupies 160 bytes on our system
   //160
   cout << "The number of bytes in the array is " << sizeof( numbers );
   // 8
   cout << "\nThe number of bytes returned by getSize is "
      << getSize( numbers ) << endl;
   // 8
   cout << "\nThe number of bytes returned by getSize is "
      << getSize2( numbers ) << endl;
} // end main

// return size of ptr        
size_t getSize( double *ptr )
{                            
   return sizeof( ptr );     
} // end function getSize

size_t getSize2( double ptr[] )
{                            
   return sizeof( ptr );     
} // end function getSize
```
* `char c; sizeof c`, `sizeof(char)`: The parentheses used with sizeof are required only if a type name is supplied as its operand.


# Pointer expressions and pointer arithmetic
* Pointer arithmetic is appropriate only for pointers that point to built-in array elements.
* When an integer is added to or substracted from a pointer, the pointer is not simply in/decremented by that integer, but by that **integer times the size of the object to which the pointer refers.**
* There is no bounds checking on pointer arithmetic. You must ensure a pointer arithmetic results in a pointer that references an element within the bounds.
* `x = ptr2 - ptr1` would assign x the number of built-in array elements from ptr1 to ptr2
  * Substracting or comparing two pointers that do not refer to elements of the same built-in array is a logic error

# `void*`
* A `void *` can't be dereferenced.
* Any pointer to a fundamental type or class type can be assigned to a `void *` without casting.
* However, a pointer of type `void *` can not be assigned directly to a pointer of another type.

# Pointer and built-in array
* For `int b[5];`
  * `int *bPtr = b` is equivalent to `int *bPtr = &b[0]`
  * `b[3]` is equivalent to `*(bPtr + 3)`
  * `bPtr[3]` refers to `b[3]`
