---
layout: post
title: "C++ Compiler Translation"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [cppreference.com](http://en.cppreference.com/w/cpp/language/translation_phases)


# Phases of translation
> The C++ source file is processed by the compiler as if the following phases


#### Phase 1
* The individual bytes of the source code file are mapped to the characters of the basic source character set. The basic source character set consists of 96 characters:
  * 5 whitespace characters (space, horizontal tab, vertical tab, form feed, new-line)
  * 10 digit characters from '0' to '9'
  * 52 letters from 'a' to 'z' and from 'A' to 'Z'
  * 29 punctuation characters: `_ { } [ ] # ( ) < > % : ; . ? * + - / ^ & | ~ ! = , \ " '`

#### Phase 2
* Whenever backslash appears at the end of a line (immediately followed by the newline character), both backslash and newline are deleted, combining two physical source lines into one logical source line.

#### Phase 3
* The source file is decomposed into comments, sequences of whitespace characters (space, horizontal tab, new-line, vertical tab, and form-feed), and preprocessing tokens
* preprocessing tokens:
  * a) header names such as `<iostream>` or `"myfile.h"` (only recognized after `#include`)
  * b) identifiers
  * c) preprocessing numbers
  * d) character and string literals , including user-defined (since C++11)
    * * Any transformations performed during phases 1 and 2 between the initial and the final double quote of any raw string literal are reverted.
  * e) operators and punctuators (including alternative tokens), such as `+`, `<<=`, `new`, `<%`, `##`, or `and`
  * f) individual non-whitespace characters that do not fit in any other category

* Each comment is replaced by one space character.

#### Phase 4
1) The preprocessor is executed.
2) Each file introduced with the #include directive goes through phases 1 through 4, recursively.
3) At the end of this phase, all preprocessor directives are removed from the source.

#### Phase 5
* All characters in character literals and string literals are converted from the source character set to the execution character set
* the conversion performed at this stage can be controlled by command line options in some implementations

#### Phase 6
* Adjacent string literals are concatenated.

#### Phase 7
* Compilation takes place: each preprocessing token is converted to a token. The tokens are syntactically and semantically analyzed and translated as a translation unit.

#### Phase 8
* Each translation unit is examined to produce a list of required template instantiations, including the ones requested by explicit instantiations.
* The definitions of the templates are located, and the required instantiations are performed to produce instantiation units.

#### Phase 9
* Translation units, instantiation units, and library components needed to satisfy external references are collected into a program image which contains information needed for execution in its execution environment.
