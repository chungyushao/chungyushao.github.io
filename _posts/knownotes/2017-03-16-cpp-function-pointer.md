---
layout: post
title: "C++ Function Pointer/Object"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [stack overflow](http://stackoverflow.com/questions/2592137/what-is-the-point-of-function-pointers)
> * [Lars Engelfried](http://www.newty.de/fpt/fpt.html)
> * [Coursera](https://www.coursera.org/learn/cpp-chengxu-sheji)

# What is Function Pointer
> 程序运行期间,每个函数都会占用一 段连续的内存空间。而函数名就是该函数所 占内存区域的起始地址(也称“入口地址”)。 我们可以将函数的入口地址赋给一个指针变 量,使该指针变量指向该函数。然后通过指 针变量就可以调用这个函数。这种指向函数 的指针变量称为“函数指针”


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

//Type 1: Pointers to ordinary C functions or to static C++ member functions
int (*pt2Function)(float, char, char) = NULL;                        // C

//Type 2: Pointers to non-static C++ member functions
int (TMyClass::*pt2Member)(float, char, char) = NULL;                // C++
int (TMyClass::*pt2ConstMember)(float, char, char) const = NULL;     // C++
```

# Assign an address to a Function Pointer
* Although it's optional for most compilers you should use the address operator & in front of the function's name in order to write portable code.

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


# Function Pointer as Comparator
* `int comparator(const void * elem1, const void * elem2);`
* 比较函数编写规则:
  * 1) 如果 * elem1应该排在 * elem2前面,则函数返回值是负整数
  * 2) 如果 * elem1和* elem2哪个排在前面都行,那么函数返回0
  * 3) 如果 * elem1应该排在 * elem2后面,则函数返回值是正整数
