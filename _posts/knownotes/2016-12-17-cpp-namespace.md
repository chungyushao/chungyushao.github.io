---
layout: post
title: "C++ `namespace`"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---


> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel


* A Program may include many identifiers defined in different scopes. Variable of the same name in a different scope might create naming conflict. `namespace` is aiming to solve this problem.
* `MyNameSpace::member`, each `namespace` defines a scope in which identifiers and variable are placed, usign scope resolution operator `::` to access.
* Not all `namespace` are guaranteed to be unique.
* different namespace members **can be defined in separate `namespace` blocks (Unlike `class`).**
* Compiler first attempts to locate a local declaration of the variables in `main`, if there are no local declarations, the compiler will assume variables are in the global `namespace`.

> There should **NEVER** be a `using` directive in a header. Having one brings the corresponding names into any file that includes the header. This could result in name collisions and subtle, hard-to-find errors. Instead, use only **fully qualified names** in headers (ex: `std::cout`, `std::string`).

# Unnamed `namespace`
* Variables, classes and functions in an unnamed `namespace` are accessible only in the current translation unit (the .cpp file and the files it includes).
* The unnamed `namespace` has an implicit `using` directive, so its members appear to occupy the `global namespace`, are accessible directly and do not have to be qualified with a `namespace` name.
* member functions of the same `namespace` can directly use the members without using a `namespace` qualifier.
  * Placing `main` in a `namespace` is a compilation error


```cpp
// create unnamed namespace
namespace
{
   double doubleInUnnamed = 88.22; // declare variable
} // end unnamed namespace
int main()
{
  cout << "unnamed namespace double: " << doubleInUnnamed;
} // end main
```
