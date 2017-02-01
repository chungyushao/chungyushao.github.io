---
layout: post
title: "Clean Code: Chapter 2 - Naming"
excerpt: ""
categories: knownotes
tags: [SE]
comments: true
share: true
author: chungyu
---

> From Clean Code

##### Use Intention-Revealing Names
* It should tell you why it exists, what it does, and how it is used.

##### Avoid Disinformation
* Abbreviation that might be meaningful in other context
* Don's use `accountList` unless it's really a `List` in data structure
  * `accountGroup`, might be better
* Beware of using names which vary in small ways.
  * `XYZControllerForEfficientHandlingOfStrings` vs `XYZControllerForEfficientStorageOfStrings`
  * auto completion will set u up...
* `l` and `1`; `O`, `o` and `0`

##### Make Meaningful Distinctions
* If names must be different, then they should also mean something different.
* `(a1, a2, .. aN)` is the opposite of intentional naming

##### Noise words
> Distinguish names in such a way that the reader knows what the differences offer.

* `ProductInfo` and `ProductData`,
  * `Info` and `Data` are indistinct noise words like `a`, `an`, and `the`.
* `a`, `the` could still be Meaningful
  * `a` for all local variables and `the` for all function arguments.
* Variable should never appear in a variable name; the word table should never appear in a table name.
* `Customer` vs `CustomerObject`
  * What should you understand as the distinction?

##### Use Pronounceable Names
> Programming is a social activity.

##### Use Searchable Names
* The length of a name should correspond to the size of its scope
  * single-letter names can ONLY be used as local variables inside short methods.
  * If a variable or constant might be seen or used in multiple places in a body of code,
it is imperative to give it a search-friendly name.

##### Avoid Encodings

##### Member Prefixes
* people quickly learn to ignore the prefix (or suffix) to see the meaningful part of the name.

##### Interfaces and Implementations
* prefer to leave interfaces unadorned.
  * `Imp`, `Factory` might be some choices to use

##### Avoid Mental Mapping
> One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king.

* using the name `c` than because `a` and `b` were already taken.  
* Except for the loop counter, a single-letter name is generally a poor choice; it’s just a place holder that the reader must mentally map to the actual concept.


##### Class Names
* Classes and objects should have noun or noun phrase names
* Avoid words like Manager, Processor, Data, or Info in the name of a class.
* A class name should not be a verb.

##### Method Names
* Methods should have verb or verb phrase names
* When constructors are overloaded, use static factory methods with names that describe the arguments.
  * `Complex fulcrumPoint = Complex.FromRealNumber(23.0);` vs `Complex fulcrumPoint = new Complex(23.0);`

##### Don’t Be Cute
> Say what you mean. Mean what you say.

* If names are too clever, they will be memorable only to people who share the author’s sense of humor, and only as long as these people remember the joke.

##### Pick One Word per Concept
* It’s confusing to have fetch, retrieve, and get as equivalent methods of different classes.
* it’s confusing to have a controller and a manager and a driver in the same code base.
  * What is the essential difference between a `DeviceManager` and a `ProtocolController`?
  * Why are both not `Controllers` or both not `Managers`
* Avoid using the same word for two purposes.

##### Use Solution Domain Names
> people who read your code will be programmers

* It is not wise to draw every name from the problem domain because we don’t want our coworkers to have to run back and forth to the customer asking what every name means when they already know the concept by a different name.  

##### Add Meaningful Context
* Imagine that you have variables named `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, and `zipcode`. Taken together it’s pretty clear that they form an address.
  * But what if you just saw the `state` variable being used alone in a method?
  * You can add context by using prefixes: `addrFirstName`, `addrLastName`, `addrState`, and so on.

##### Don’t Add Gratuitous Context
* In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with `GSD`.
  * Why make it hard for the IDE to help you?
