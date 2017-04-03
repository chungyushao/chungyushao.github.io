---
layout: post
title: "C++ string"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---


> Code and text copied/summarized from C++ How to program 9 ed by Paul Deitel, Harvey Deitel


C++ string class is preferred for use in new programs.

# Characters
* The value of a character constant is the integer value of the character in the machine's character set.
* Every program is composed of a sequence of characters that is interpreted by the compiler as instructions used to accomplish a task.

# Strings
* `string` is a series of characters treated as a single unit.
* *String literals, string constants* are written in `""`
  * String literals have static storage duration and may not be shared if the same string literal is referenced from multiple locations in a program

# Pointer-based Strings
* A pointer-based string is a built-in array of characters ending with a null character `'\0'`.
* `sizeof` a string literals is the length of the string **including the '\0'**

# Initialization
* `char color[] = "blue";`: a five element built-in array containing `'\0'`
  * same as `char color[] = {'b', 'l', 'u', 'e', '\0'}`
  * You must make sure the built-in array is large enough to store the string and its terminating null character.
* `const char *colorPtr = "blue"`: a pointer that points to the letter `b`
* When storing a string of characters in a built-in array of `chars`, if a string is longer than the built-in array of `chars`, characters beyond the end of the built-in array will overwrite data in memory following the built-in array, leading logic errors and potential breaches.
