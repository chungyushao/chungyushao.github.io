---
layout: post
title: "Design Pattern"
excerpt: ""
categories: knownotes
tags: [design pattern, C++]
comments: true
share: true
author: chungyu
---
> * [C++ Programming/Code/Design Patterns](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Code/Design_Patterns#Programming_Patterns)
> * [Design Patterns - An introduction by in28minutes](https://www.youtube.com/watch?v=0jjNjXcYmAU)

# Three Types
* Creational
  * How you create objects (efficiently)
  * Prototype, Builder, Singleton, Factory
* Structural
  * how the objects are composed, is there an inheritance relationship in between
  * Proxy, Decorator, Facade, Adapter, Flyweight
* Behavioral
  * Different objects, how to interacts
  * Chain of responsibilities, Iterator, State, Strategy, Observer, Visitor, Template Method, Command, Memento, Meditator

---
# Creational Pattern
> design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

### Prototype
* When the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.
* Declare an abstract base class that specifies a `pure virtual clone()` method. Any class that needs a "polymorphic constructor" capability derives itself from the abstract base class, and implements the `clone()` operation.

### Buillder
* Problem: We want to construct a complex object, however we do not want to have a complex constructor member or one that would need many arguments.
* Solution: Define an intermediate object whose member functions define the desired object part by part before the object is available to the client. Builder Pattern lets us **defer the construction of the object until all the options for creation have been specified**.


### Singleton
* The Singleton pattern ensures that a class has only one instance and provides a global point of access to that instance. This is useful when exactly one object is needed to coordinate actions across the system.
* Check list
  * Define a private static attribute in the "single instance" class.
  * Define a public static accessor function in the class.
  * Do "lazy initialization" (creation on first use) in the accessor function.
  * Define all constructors to be protected or private.
  * Clients may only use the accessor function to manipulate the Singleton.

### Factory
* Problem: We want to decide at run time what object is to be created based on some configuration or application parameter. When we write the code, we do not know what class should be instantiated.
Solution
* Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method **lets a class defer instantiation to subclasses**.


---

# Structural

### Proxy
* An object representing anther object
  * ex: Credit card is a proxy for what is in bank account

### Decorator
* The decorator pattern helps to attach additional behavior or responsibilities to an object dynamically.
* Decorators provide a flexible alternative to subclassing for extending functionality. This is also called “Wrapper”

### Facade
* The Facade Pattern hides the complexities of the system by providing an interface to the client from where the client can access the system on an unified interface.
* Facade defines a higher-level interface that makes the subsystem easier to use. For instance making one class method perform a complex process by calling several other classes.

### Adapter
* Convert the interface of a class into another interface that clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

### Flyweight
* Imagine a huge amount of similar objects which all have most of their properties the same. It's natural to move these properties out of these objects to some external data structure and provide each object with the link to that data structure.

### Bridge
* separate out the interface from its implementation. Doing this gives the flexibility so that both can vary independently.

### Composite
* Composite lets clients treat individual objects and compositions of objects uniformly. The Composite pattern can represent both the conditions.

---
# Behavior

### Chain of responsibilities
* A way of passing a request between a chain of objects
* The request pass through them until it can find somebody who can handle that particular request
  * ex: exception handling
  * ex: logger

### Iterator
* Sequentially access the elements of a collection

### State
* Alter an object's behavior when its state change

### Strategy
* Encapsulates an algorithm inside a class
  * ex: comparator

### Observer
* A way of notifying change to number of classes
  * ex: online-bidding

### Visitor
* Defines a new operation to a class without change the class

### Template Method
* Defer the exact steps of an algorithm to a subclass

### Command
* Encapsulate a command request as an object

### Memento
* Capture and restore an object's internal state
  * ex: Save/load in game...

### Mediator
* Defines simplified communication between classes
  * ex: air traffic controller unifies the communication between airplane
