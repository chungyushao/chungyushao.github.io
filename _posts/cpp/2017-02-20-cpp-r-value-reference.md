---
layout: post
title: "C++ Rvalue references"
excerpt: "LValue, RValue, `T &&`"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> Noted from
> * [C++ Rvalue References Explained By Thomas Becker](http://thbecker.net/articles/rvalue_references/section_01.html)
> * [Cprogramming.com by Alex Allain](http://www.cprogramming.com/c++11/rvalue-references-and-move-semantics-in-c++11.html)
> * [cplusplus.com](http://www.cplusplus.com/articles/y8hv0pDG/)
> * [Effective Morder C++](http://shop.oreilly.com/product/0636920033707.do)


```cpp
#include <iostream>

using namespace std;

vector<int> doubleValues (const vector<int>& v) {
    vector<int> new_values;
    new_values.reserve(v.size());
    for (auto itr = v.begin(), end_itr = v.end(); itr != end_itr; ++itr) {
        new_values.push_back(2 * * itr);
    }
    return new_values;
}

int main() {
    vector<int> v;
    for (int i = 0; i < 100; i++){
        v.push_back(i);
    }
    v = doubleValues(v);
}
```

# Problems of the code: redundant copies
* When doubleValues is called, it constructs a new vector, new_values, and fills it up.
* When we hit the return statement, the entire contents of new_values must be copied.
  * One into a temporary object to be returned (Maybe optimized out by compiler)
  * a second when the vector assignment operator runs on the line `v = doubleValues(v);` (Need must)
* Object returned from doubleValues is a temporary value that's no longer needed, and get thrown away once it is copied!

# Why cannot we simply `move` the object?
* (In C++03) there was no way to tell if an object was a temporary or not.

# The solution: RValue assignment
* Rvalue references allow a function to branch at compile time (via overload resolution) on the condition "Am I being called on an lvalue or an rvalue?"
* Rvalue references solve at least two problems:
  * Implementing move semantics
  * Perfect forwarding

# What is lvalue
* An lvalue is an expression that refers to a memory location and allows us to take the address of that memory location via the `&` operator.
  * An lvalue is an expression whose address can be taken, a locator value--essentially, an lvalue provides a (semi)permanent piece of memory. You can make assignments to lvalues.

```cpp
// lvalues:
int a;
a = 1; // here, a is an lvalue

//You can also have lvalues that aren't variables:
int x;
int & getRef () {
    return x;
}
getRef() = 4;
cout << x << endl; //4
cout << getRef() << endl; //4
cout << & x << endl; //same as below
cout << & getRef() << endl;
```

# Another view of lvalue
* "non-const references cannot bind to temporary objects." --- C++ standard

```cpp
// Improperly declared function:  parameter should be const reference:
void print_me_bad( std::string& s ) {
    std::cout << s << std::endl;
}

// Properly declared function: function has no intent to modify s:
void print_me_good( const std::string& s ) {
    std::cout << s << std::endl;
}

std::string hello( "Hello" );

print_me_bad( hello );  // Compiles ok; hello is not a temporary
print_me_bad( std::string( "World" ) );  // Compile error; temporary object
print_me_bad( "!" ); // Compile error; compiler wants to construct temporary
                     // std::string from const char*

print_me_good( hello ); // Compiles ok
print_me_good( std::string( "World" ) ); // Compiles ok
print_me_good( "!" ); // Compiles ok
```

# What is rvalue
* An rvalue is an expression that is not an lvalue.
  * An expression is an rvalue if it results in a temporary object.

```cpp
string getName () { return "Alex"; }
string name = getName();
const string& name = getName(); // ok
string& name = getName(); // NOT ok
```

* The differences is  you're assigning from a temporary object (copied in the return statement), not from some value that has a fixed location. `getName()` is an rvalue.
* The `const string& name = getName();` before C++11 is called lvalue reference. But only if it was `const`.
  * Why need to be `const`? if you didn't, you'd be able to modify some object that is about to disappear, and that would be dangerous.
  * Solutions in C++11: rvalue reference

> So rvalue reference can also be considered as a way to bind a mutable reference to an rvalue

# Summary of lvalue and rvalue
* A useful heuristic to determine whether an expression is an lvalue is to ask if you can take its address. If you can, it typically is. If you can’t, it’s usually an rvalue.
* When dealing with a parameter of rvalue reference type, because the parameter itself is an lvalue.

```cpp
class Widget {
public:
     Widget(Widget&& rhs); // rhs is an lvalue, though it has
     ...                   // an rvalue reference type
};
```

# Rvalue reference

```cpp
const string&& name = getName(); // ok
string && name = getName(); // also ok
```
* if `X` is any type
  * `X &&` is called an rvalue reference to X.
  * `X &` is now also called an lvalue reference.

```cpp
string getName() { return "Alex"; }
printReference (const String& str) { cout << str; }
printReference (String&& str) { cout << str; }

string author("alex");
printReference(author);
// calls the first printReference function,
// taking an lvalue reference

printReference(getName());
// calls the second printReference function,
// taking a mutable rvalue reference
```

* the `printReference` function taking a const lvalue reference will accept any argument that it's given, whether it be an lvalue or an rvalue, and regardless of whether the lvalue or rvalue is mutable or not.
  * **However, when the presence of the second overload, `printReference` will be given all values except mutable rvalue-references.**
    * The "branching" effect right reference provides
  * **This kind of overload should occur only for copy constructors and assignment operators, for the purpose of achieving move semantics**

> rvalue reference version of the method is like the secret back door entrance to the club that you can only get into if you're a temporary object

# Move Semantics

```cpp
#include <iostream>
using namespace std;

struct X {
	X(int v) : v_(v) { cout << "Construct " << v_ << endl; }
	X(const X& rhs) : v_(rhs.v_) { cout << "Cpy " << v_ << endl; }
	~X() { cout << "Destruct " << v_ << endl; }
	int v_;
};

X foo() {
	cout << "into foo" << endl;
	return X(2);
}

int main() {
	X x(1);
	x = foo();
}

/*
  Construct 1
  into foo
  Construct 2
  Destruct 2
  Destruct 2   */

```

* The `x = foo();` will
  * clones the resource from the temporary returned by foo,
  * destructs the resource held by x and replaces it with the clone,
  * destructs the temporary and thereby releases its resource.
* However, in the special case where the right hand side of the assignment is an rvalue, we want the copy assignment operator to act like this: `swap m_pResource and rhs.m_pResource`
  * swap resource pointers (handles) between x and the temporary,
  * let the temporary's destructor destruct x's original resource.
* This is called **move semantics**.

# Move constructor and move assignment operator

```cpp
class ArrayWrapper {
public:
    ~ArrayWrapper () { delete [] _p_vals; }
    // default constructor produces a moderately sized array
    ArrayWrapper () : _p_vals(new int[64]), _size(64) {}
    ArrayWrapper (int n) : _p_vals(new int[n]), _size( n ) {}

    // copy constructor
    ArrayWrapper (const ArrayWrapper& other) :
    _p_vals(new int[other._size]), _size(other._size) {
        for (int i = 0; i < _size; ++i) {
            _p_vals[i] = other._p_vals[i];
        }
    }

    // move constructor
    ArrayWrapper (ArrayWrapper&& other) :
    _p_vals(other._p_vals), _size(other._size) {
        other._p_vals = NULL;
        other._size = 0;
    }

private:
    int *_p_vals;
    int _size;
};
```
