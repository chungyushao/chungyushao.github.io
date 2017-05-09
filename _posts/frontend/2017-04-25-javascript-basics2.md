---
layout: post
title: "Javascript Basics 2 -- this"
excerpt: "this"
categories: frontend
tags: [javascript]
comments: true
share: true
author: chungyu
---
> * [Udemy](https://www.udemy.com/understand-javascript/learn/)
> * [javascriptissexy.com](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)

# Intro

* in JavaScript, we use the this keyword as a shortcut, a referent; it refers to an object; that is, the subject in context, or the subject of the executing code.
* when a function executes, it gets the `this` property—a variable with the value of the object that invokes the function where `this` is used.
* We need this to access methods and properties of the object that invokes function A, especially since we don’t always know the name of the invoking object, and sometimes there is no name to use to refer to the invoking object.
* `this` is really just a shortcut reference for the “antecedent object”—the invoking object.

> * `this` is not assigned a value until an object invokes the function where this is defined.
> * Call it **this Function** below

* JQuery example:

```js
$ ("button").click (function (event) {
// $(this) will have the value of the button ($("button")) object​
​// because the button object invokes the click () method​
  console.log ($ (this).prop ("name"));
});
```
* The use of `$(this)`, which is jQuery’s syntax for the `this` keyword in JavaScript, is used inside an anonymous function, and the anonymous function is executed in the button’s `click ()` method.
* jQuery library binds `$(this)` to the object that invokes the click method.
* the button is a DOM element on the HTML page, and it is also an object; in this case it is a jQuery object because we wrapped it in the jQuery `$()` function.

# The hard part
* `this` gets a bit troublesome in situations where the original context (where this was defined) changes, particularly in callback functions, when invoked with a different object, or when borrowing methods. Always remember that `this` is assigned the value of the object that invoked the **this Function**.

### `this` when used in a method passed as a callback

```js
var user = {
  data : [
    {name : "T. Woods", age : 37},
    {name : "P. Mickelson", age : 43}
  ],
  clickHandler : function (event) {
    // random number between 0 and 1​
    var randomNum = ((Math.random () * 2 | 0) + 1) - 1;
  ​
    // This line is printing a random person's name and age from the data array​
    console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
  }
}

$ ("button").click (user.clickHandler);
// Cannot read property '0' of undefined
```
* The button is wrapped inside a jQuery `$` wrapper, so it is now a jQuery object​
* And the output will be undefined because there is no `data` property on the button object​
* Solution: `$("button").click (user.clickHandler.bind (user));`

### `this` inside closure

```js
var user = {
  tournament : "The Masters",
  data       : [
    {name : "T. Woods", age : 37},
    {name : "P. Mickelson", age : 43}
  ],
​
  clickHandler : function () {
    // the use of this.data here is fine,
    // because "this" refers to the user object,
    //and data is a property on the user object.​
​
    this.data.forEach (function (person) {
      // But here inside the anonymous function (that we pass to the forEach method),
      // "this" no longer refers to the user object.​
      // This inner function cannot access the outer function's "this"​

      console.log ("What is This referring to? " + this); //[object Window]​
      console.log (person.name + " is playing at " + this.tournament);
      // T. Woods is playing at undefined​
      // P. Mickelson is playing at undefined​
    })
  }​
}
user.clickHandler(); // What is "this" referring to? [object Window]
```

* Solution:

```js
var user = {
  ...
  clickHandler : function (event) {
    // To capture the value of "this" when it refers to the user object,
    // we have to set it to another variable here:​
    // We set the value of "this" to theUserObj variable, so we can use it later​
    var theUserObj = this;
    this.data.forEach (function (person) {
      // Instead of using this.tournament, we now use theUserObj.tournament​
      console.log (person.name + " is playing at " + theUserObj.tournament);
    })
  }
}
user.clickHandler();
```

### `this` when method is assigned to a variable

```js
// This data variable is a global variable​
var data = [
  {name : "Samantha", age : 12},
  {name : "Alexis", age : 14}
];
​
var user = {
// this data variable is a property on the user object​
data    :[
  {name:"T. Woods", age:37},
  {name:"P. Mickelson", age:43}
  ],
  showData : function (event) {
    var randomNum = ((Math.random () * 2 | 0) + 1) - 1;
  ​
    // This line is adding a random person from the data array to the text field​
    console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
  }
​}
​
// Assign the user.showData to a variable​
var showUserData = user.showData;
​
// When we execute the showUserData function, the values printed to the console
// are from the global data array, not from the data array in the user object​
​
showUserData (); // Samantha 12 (from the global data array)​
```

* Solution: setting the `this` value with the `bind` method:

```js
// Bind the showData method to the user object​
var showUserData = user.showData.bind (user);
​
// Now we get the value from the user object,
// because the this keyword is bound to the user object​
showUserData (); // P. Mickelson 43
```

### borrowing methods
