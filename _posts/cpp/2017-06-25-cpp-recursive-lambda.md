---
layout: post
title: "C++ Recursive lambda function"
excerpt: ""
categories: cpp
tags: [C++, lambda]
comments: true
share: true
author: chungyu
---

> [Stackoverflow](https://stackoverflow.com/questions/2067988/recursive-lambda-functions-in-c11)

* Think about the difference between the `auto` version and the fully specified type version.
* The `auto` keyword infers its type from whatever it's initialized with
  * but what you're initializing it with needs to know what its type is
  * (in this case, the **lambda closure needs to know the types it's capturing**). Something of a chicken-and-egg problem.

> On the other hand, a fully specified function object's type doesn't need to "know" anything about what is being assigned to it, and so the lambda's closure can likewise be fully informed about the types its capturing.


# Recursive factorial as example

```cpp
#include <iostream>
#include <functional>
using namespace std;


int main() {
    int N; cin >> N;

    std::function<int(int)> factorial = [&factorial](int n) {
        if (n == 1) return 1;
        else return n * factorial(n - 1);
    };
    cout << factorial(N) << endl;
    return 0;
}
```
