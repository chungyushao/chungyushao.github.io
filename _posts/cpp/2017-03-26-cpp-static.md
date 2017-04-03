---
layout: post
title: "C++ Storage class specifiers"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [Coursera](https://www.coursera.org/learn/cpp-chengxu-sheji)
> * [cppreference.com](http://en.cppreference.com/w/cpp/language)

# `static` Members
* Inside a class definition, the keyword `static` declares members that are not bound to class instances.
* Outside a class definition, it has a different meaning: **Storage class specifiers**.

# Storage class specifiers
* The storage class specifiers with the scope of the name, control two independent properties of the name:
	* Its storage duration and its linkage.
* `auto` - automatic storage duration. (until C++11)
* `static` - static or thread storage duration and internal linkage
* `extern` - static or thread storage duration and external linkage
* `thread_local` - thread storage duration. (since C++11)
* Only one storage class specifier may appear in a declaration except that `thread_local` may be combined with `static` or with `extern` (since C++11)

### Storage duration
* All objects in a program have one of the following storage durations:
###### automatic storage duration
* The object is allocated at the beginning of the enclosing code block and deallocated at the end.
* All local objects have this storage duration, except those declared static, extern or thread_local.
###### static storage duration
* The storage for the object is allocated when the program begins and deallocated when the program ends.
* Only one instance of the object exists.
* All objects declared at namespace scope (including global namespace) have static storage duration, plus those declared with static or extern.
###### thread storage duration
* The object is allocated when the thread begins and deallocated when the thread ends.
* Each thread has its own instance of the object.
* Only objects declared `thread_local` have this storage duration.
* `thread_local` can appear together with `static` or `extern` to adjust linkage. (since C++11)
###### dynamic storage duration
* The object is allocated and deallocated per request by using dynamic memory allocation functions.
