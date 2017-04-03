---
layout: post
title: "Copy elision"
excerpt: "Named Return Value Optimization, Return Value Optimization"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [stackoverflow 1](http://stackoverflow.com/questions/12953127/what-are-copy-elision-and-return-value-optimization/12953145#12953145)
> * [stackoverflow 2](http://stackoverflow.com/questions/17473753/c11-return-value-optimization-or-move)

# General

> Copy elision is a compiler optimization technique that eliminates unnecessary copying/moving of objects.


###### NRVO (Named Return Value Optimization)
* If a function returns a class type by value and the return statement's expression is the name of a non-volatile object with automatic storage duration (which isn't a function parameter), then the copy/move that would be performed by a non-optimising compiler can be omitted.
  * If so, the returned value is constructed directly in the storage to which the function's return value would otherwise be moved or copied.

```cpp
class Thing {
public:
  Thing();
  ~Thing();
  Thing(const Thing&);
};
Thing f() {
  Thing t;
  return t;
}
Thing t2 = f();
```

###### RVO (Return Value Optimization)
* If the function returns a nameless temporary object that would be moved or copied into the destination by a naive compiler, the copy or move can be omitted as per 1.

```
class Thing {
public:
  Thing();
  ~Thing();
  Thing(const Thing&);
};
Thing f() {
  return Thing();
}
Thing t2 = f();
```

###### Other copy elision place
* Temporary is passed by value

```cpp
class Thing {
public:
  Thing();
  ~Thing();
  Thing(const Thing&);
};
void foo(Thing t);

foo(Thing());
```

* Exception is thrown and caught by value

```cpp
struct Thing{
  Thing();
  Thing(const Thing&);
};

void foo() {
  Thing c;
  throw c;
}

int main() {
  try {
    foo();
  }
  catch(Thing c) {  
  }             
}
```

# When to use `std::move`?

```cpp
// Choices:

// (a) let compiler optimize it
SerialBuffer read( size_t size ) const
{
    SerialBuffer buffer( size );
    read( begin( buffer ), end( buffer ) );
    // Return Value Optimization
    return buffer;
}

// (b) explicit move
SerialBuffer read( size_t size ) const
{
    SerialBuffer buffer( size );
    read( begin( buffer ), end( buffer ) );
    return move( buffer );
}
```

* Choice (a): `return buffer;`
  * NRVO might be applied
  * if NRVO not applied, local variables `buffer` will become R-values within a return statement and will be moved.
* Choice (b): `return std::move( buffer );`
  * NVRO will not happen, and `buffer` will be moved from return statement.
  * Using `move` here force the compiler to use a move (or a copy if move is unavailable).
  * If you're returning a local variable, don't use `move()`.

* If you're returning something other than a local variable, NRVO isn't an option anyway
  * and you should use `move()` if (and only if) you intend to pilfer (盜,偷,竊) the object.

* There is one exception to this rule

```cpp
Buffer read(Buffer&& buffer) {
    //...
    return std::move( buffer );
}
```

* If `buffer` is an rvalue reference, then you should use `std::move`.
* This is because **references are not eligible for NRVO**, so without `std::move` it would result in a copy from an lvalue.

# Conclusion
> * Always move rvalue references and forward universal references
> * Never move a return value
