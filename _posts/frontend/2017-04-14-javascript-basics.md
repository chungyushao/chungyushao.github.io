---
layout: post
title: "Javascript Basics 1"
excerpt: "object, functional statement, functional expression"
categories: frontend
tags: [javascript]
comments: true
share: true
author: chungyu
---
> [Udemy](https://www.udemy.com/understand-javascript/learn/)


# Object
* Collections of name, value pair
* Sitting in the memory with reference (if have those members) to its
  * Primitive "property"
  * Object "property"
  * Function "method"

```js
var person = new Object();
person.firstname = "Joey"
console.log(person[firstname])
console.log(person.firstname)
```

### Object literals
* Define object with curly braces
* `var person = {};`: create an empty object
* `var person = {firstname: 'Joey'};`: object with one property

### Create object on the Flyweight

```js
function greet(person) {
  console.log('Hi' + person.firstname);
}
greet({firstname: 'Joey'});
```

### Use object as fake namespace

```js
var english = {};
var spanish = {};
english.greetings = {};
spanish.greetings = {};
english.greetings.greet = "Hello";
spanish.greetings.greet = "Hola";
//or
// english = { greetings : {greet: "Hello"} } //object literals
```

### Json: JavaScript Object Notation

```js
var person = { firstname: 'Joey', lastname: 'Shao' };
console.log(JSON.stringify(person));
var obj = JSON.parse('{"firstname": "Joey", "lastname": "Shao"}');
```

---

# Function
### Functions are objects
* **First class functions**: Everything you can do with other types you can do with functions.
  * Assign them to variables, pass them around, create them on the fly
* Like ordinary object has property method, function (object) has some more properties
  * Name: Function's name is optional, (can be anonymous)
  * Code: an invocable property that can be called by `()`

```js
function greet() { console.log('hi'); }
console.log(greet);
greet(); //invocable
```

### Function statement
* Statement:
  * `var a;`, `if (a === 3) {...}`

```js
function greet() { console.log('hi'); }
greet();
```  
* function "statement", didn't return a value, just store into memory instead

### Function expression
* Expression: a unit of code that results a value (doesn't have to save to a variable)
  * `a = 3;`, `1 + 2;`, `a === 3`

```js
var anonymousGreet = function() { console.log('hi'); }
anonymousGreet();
```

### Difference between function statement/expression

```js
//can print out the statement
statement();
function statement() {
	console.log('statement');
};

//below will cause: Uncaught TypeError: expression is not a function
expression();
var expression = function () { console.log('expression'); }
```
* Statement and variables will be preload into memory
* Only `var expression` is preload into memory, but the result of expression
* So `expression` will be `undefined` before it really got executed
  * `undefined` itself is not callable
