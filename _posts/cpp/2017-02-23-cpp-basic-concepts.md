---
layout: post
title: "C++ Basic Concepts"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [cppreference.com](http://en.cppreference.com/w/cpp/language/basic_concepts)


> A C++ program is a sequence of text files (typically header and source files) that contain **declarations**. They undergo translation to become an executable program, which is executed when the OS calls its `main` function.

### [Declarations](http://en.cppreference.com/w/cpp/language/declarations)
* Declarations introduce names into the C++ program.
* Each kind of entity is declared differently.
* A simple declaration is a statement that introduces, creates, and optionally initializes one or several identifiers, typically variables.


### Definitions
* **Definitions** are declarations that are sufficient to use the entity identified by the name.

### [Identifiers](http://en.cppreference.com/w/cpp/language/identifiers)
* An identifier is an arbitrarily long sequence of digits, underscores, lowercase and uppercase Latin letters, and most Unicode characters
* A valid identifier must begin with a non-digit character (Latin letter, underscore, or Unicode non-digit character).
* Identifiers are case-sensitive (lowercase and uppercase letters are distinct), and every character is significant.
* Identifiers with special meaning may be used as names of objects or functions, but have special meaning in certain contexts.


> Certain words in a C++ program have special meaning, and these are known as **keywords**. Others can be used as identifiers. Comments are ignored during translation. Certain characters in the program have to be represented with escape sequences.

### [Keywords](http://en.cppreference.com/w/cpp/keyword)
* Reserved keywords in C++ are used by the language, these keywords are not available for re-definition or overloading.
* All identifiers that contain a double underscore __ in any position is always reserved
* Each identifier that begins with an underscore followed by an uppercase letter is always reserved
* All identifiers that begin with an underscore are reserved for use as names in the global namespace.

> The entities of a C++ program are values, objects, references, functions, enumerators, types, class members, templates, template specializations, namespaces, parameter packs, and the `this` pointer. Preprocessor macros are not C++ entities.

### [Template Specializations](http://en.cppreference.com/w/cpp/language/template_specialization)
* Allows customizing the template code for a given set of template arguments.

### [Namespaces](http://en.cppreference.com/w/cpp/language/namespace)
* Namespaces provide a method for preventing name conflicts in large projects.
  * Symbols declared inside a namespace block are placed in a named scope that prevents them from being mistaken for identically-named symbols in other scopes.
  * Multiple namespace blocks with the same name are allowed.
* All declarations within those blocks are declared in the named scope.

> Entities are introduced by declarations, which associate them with names and define their properties. The declarations that define all properties required to use an entity are definitions. A program must contain only one definition of any non-inline function or variable that is odr-used.

### [ODR-use, One Definition Rule](http://en.cppreference.com/w/cpp/language/definition)
* Only one definition of any variable, function, class type, enumeration type, or template is allowed in any one translation unit (some of these may have multiple declarations, but only one definition is allowed).
* One and only one definition of every non-inline function or variable that is odr-used is required to appear in the entire program (including any standard and user-defined libraries).
* The compiler is not required to diagnose this violation, but the behavior of the program that violates it is undefined.

```cpp
//code from [wikipedia](https://en.wikipedia.org/wiki/One_Definition_Rule)
struct S;     // declaration of S
S * p;        // ok, no definition required
void f(S&);   // ok, no definition required
void f(S*);   // ok, no definition required
S f();        // ok, no definition required - this is a function declaration only!

S s;          // error, definition required
sizeof(S);    // error, definition required
```

> Definitions of functions include sequences of **statements**, some of which include **expressions**, which specify the computations to be performed by the program.

### [Statements](http://en.cppreference.com/w/cpp/language/statements)
* Statements are fragments of the C++ program that are executed in sequence. The body of any function is a sequence of statements.

### [Expressions](http://en.cppreference.com/w/cpp/language/statements)
* An expression is a sequence of operators and their operands, that specifies a computation.
* Expression evaluation may produce a result (e.g., evaluation of 2+2 produces the result 4) and may generate side-effects (e.g. evaluation of `std::printf("%d", 4)` prints the character '4' on the standard output).

> Names encountered in a program are associated with the declarations that introduced them using name lookup. Each name is only valid within a part of the program called its **scope**. Some names have linkage which makes them refer to the same entities when they appear in different scopes or translation units.

### [Scope](http://en.cppreference.com/w/cpp/language/scope)
* Each name that appears in a C++ program is only valid in some possibly discontiguous portion of the source code called its scope.
* Within a scope, unqualified name lookup can be used to associate the name with its declaration.

### [Linkage](http://en.cppreference.com/w/cpp/language/storage_duration)
* A name that denotes object, reference, function, type, template, namespace, or value, may have linkage.
* If a name has linkage, it refers to the same entity as the same name introduced by a declaration in another scope.
* If a variable, function, or another entity with the same name is declared in several scopes, but does not have sufficient linkage, then several instances of the entity are generated.

> Each object, reference, function, expression in C++ is associated with a **type**, which may be fundamental, compound, or user-defined, complete or incomplete, etc.

### [Type](http://en.cppreference.com/w/cpp/language/type)
* Objects, references, functions including function template specializations, and expressions have a property called `type`, which both restricts the operations that are permitted for those entities and provides semantic meaning to the otherwise generic sequences of bits.

> Named objects and named references to objects are known as variables.
