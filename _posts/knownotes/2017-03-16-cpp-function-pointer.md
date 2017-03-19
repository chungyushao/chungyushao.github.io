---
layout: post
title: "Function Pointer/Object"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [stack overflow](http://stackoverflow.com/questions/2592137/what-is-the-point-of-function-pointers)
> * [Lars Engelfried](http://www.newty.de/fpt/fpt.html)


# Why Function Pointer?

* Most examples boil down to **callbacks**: You call a function `f()` passing the address of another function `g()`, and `f()` calls `g()` for some specific task. If you pass `f()` the address of `h()` instead, then `f()` will call back `h()` instead.
  * Why doing so? this is a way to **parametrize a function**: Some part of its behavior is not hard-coded into `f()`, but into the callback function.
  * Callers can make f() behave differently by passing different callback functions.
* Other cases: Different patterns (like Strategy, Observer)

# Two types of Function Pointer
* The basic difference is that all pointers to non-static member functions need a hidden argument: The this-pointer to an instance of the class.
* These two types of function pointers are incompatible with each other.

```cpp
//define a function pointer and initialize to NULL

//Pointers to ordinary C functions or to static C++ member functions
int (*pt2Function)(float, char, char) = NULL;                        // C

//Pointers to non-static C++ member functions
int (TMyClass::*pt2Member)(float, char, char) = NULL;                // C++
int (TMyClass::*pt2ConstMember)(float, char, char) const = NULL;     // C++
```

# Assign an address to a Function Pointer
* Although it's optional for most compilers you should use the address operator & infront of the function's name in order to write portable code.

```cpp


int DoIt  (float a, char b, char c){ printf("DoIt\n");   return a+b+c; }
int DoMore(float a, char b, char c)const{ printf("DoMore\n"); return a-b+c; }

pt2Function = DoIt;      // short form, not recommend
pt2Function = &DoMore;   // correct assignment using address operator


// C++
class TMyClass {
public:
   int DoIt(float a, char b, char c){
     cout << "TMyClass::DoIt"<< endl; return a+b+c;
   };
   int DoMore(float a, char b, char c) const {
     cout << "TMyClass::DoMore" << endl; return a-b+c;
   };
};

pt2ConstMember = &TMyClass::DoMore; // correct assignment using address operator
pt2Member = &TMyClass::DoIt; // note: <pt2Member> may also legally point to &DoMore

```

# Comparing Function Pointers
* `!=`, `==` can be used!

```cpp
if(pt2Function >0) {                           // check if initialized
   if(pt2Function == &DoIt)
      printf("Pointer points to DoIt\n");
}
else
   printf("Pointer not initialized!!\n");

if(pt2ConstMember == &TMyClass::DoMore)
   cout << "Pointer points to TMyClass::DoMore" << endl;

```

# Calling a Function using a Function Pointer

```cpp
int result1 = pt2Function    (12, 'a', 'b');          // C short way
int result2 = (*pt2Function) (12, 'a', 'b');          // C

TMyClass instance1;
int result3 = (instance1.*pt2Member)(12, 'a', 'b');   // C++
int result4 = (*this.*pt2Member)(12, 'a', 'b');       // C++ if this-pointer can be used

TMyClass* instance2 = new TMyClass;
int result4 = (instance2->*pt2Member)(12, 'a', 'b');  // C++, instance2 is a pointer
delete instance2;
```

# Pass a Function Pointer as an Argument

```cpp
void PassPtr(int (* pt2Func)(float, char, char)) {
   int result = (* pt2Func)(12, 'a', 'b');
   cout << result << endl;
}

void Pass_A_Function_Pointer() {
   cout << endl << "Executing 'Pass_A_Function_Pointer'" << endl;
   PassPtr(&DoIt);
}

```

# Return a Function Pointer
* 'Plus' and 'Minus' are defined above. They return a float and take two float

```cpp
// Direct solution: Function takes a char and returns a pointer to a
// function which is taking two floats and returns a float. <opCode>
// specifies which function to return
float (* GetPtr1(const char opCode))(float, float)
{
   if(opCode == '+')
      return &Plus;
   else
      return &Minus; // default if invalid operator was passed
}


// Solution using a typedef: Define a pointer to a function which is taking
// two floats and returns a float
typedef float(* pt2Func)(float, float);

// Function takes a char and returns a function pointer which is defined
// with the typedef above. <opCode> specifies which function to return
pt2Func GetPtr2(const char opCode) {
   if(opCode == '+')
      return &Plus;
   else
      return &Minus; // default if invalid operator was passed
}

// Execute example code
void Return_A_Function_Pointer()
{
   cout << endl << "Executing 'Return_A_Function_Pointer'" << endl;

   // define a function pointer and initialize it to NULL
   float (* pt2Function)(float, float) = NULL;

   pt2Function = GetPtr1('+');   // get function pointer from function 'GetPtr1'
   cout << (* pt2Function)(2, 4) << endl;   // call function using the pointer


   pt2Function = GetPtr2('-');   // get function pointer from function 'GetPtr2'
   cout << (* pt2Function)(2, 4) << endl;   // call function using the pointer
}

```

# Arrays of Function Pointers
