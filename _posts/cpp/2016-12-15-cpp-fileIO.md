---
layout: post
title: "C++ input/output stream"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---


> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel



```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
  fstrem fFile;
  fFile.open("filename", ios::out);
  //is equivalent to...
  ofstream oFile;
  oFile.open("filename");
}
```
