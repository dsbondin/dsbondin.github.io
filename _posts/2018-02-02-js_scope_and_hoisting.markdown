---
layout: post
title:      "JS Hoisting, Scope and Context"
date:       2018-02-02 11:29:40 -0500
permalink:  js_scope_and_hoisting
---

Even though I'm almost done with the Javascript section of the course, it turns out there are still some blanks I need to fill. Specifically scope and hoisting. I will not go into every detail as this info is abundant on Learn.co and other resources, I'll just concentrate on the tricky parts using examples. 

1.
```
function logging() {
   console.log(greet());
   console.log(name);
   
   function greet() {
      return "Hello!";
   }
	 
   var name = "Donald Trump";

}

logging();
----------------
// Hello!
// undefined
```

Sorry, Donald, your name is `undefined` (which is still better than `not defined` :). So what happens here is that both the function and variable get hoisted, but the value of the variable `name` get assigned only during execution phase, after console.log is called. If we decide to assign the function to a variable, a TypeError will be thrown: 

```
function logging() {
   console.log(greet());
   console.log(name);
   
   var greet = function() {
      return "Hello!";
   }
	 
   var name = "Donald Trump";

}

logging();
----------------
// TypeError: greet is not a function
```

When the `console.log(greet())` gets executed, the engine doesn't know yet that `greet` is a function. It's still a hoisted undefined variable. 

The following example demonstrates that the scope is created during hoisting is preserved through the execution phase: 

2.
```
const name = 'Donald Trump';
 
function myNameIs() {
  console.log('My name is ' + name);
}
 
function clone() {
  const name = 'Jack Sparrow';
  myNameIs();
}

clone();
----------------
// My name is Donald Trump
```

Even though `myNameIs()` is called inside the `clone()` function, it was declared in the global scope. So when it cannot find the variable `name` locally it goes a scope higher, where `name`'s value is "Donald Trump". When we deal with the scope, what matters is where the function is declared, not where it's called. 

3.
```
let name = "Jack Sparrow";

function myNameIs() {
  let name = "Donald Trump";
  console.log(`My name is ${this.name}!`);
}

myNameIs();
----------------
// My name is Jack Sparrow!
```

This one is pretty simple. The function `myNameIs` is not an object's method (or we could say it's a global window's method), so `this` here refers to global window object. And `name` in the global context is "Jack Sparrow". Let's change this code a little bit. 

```
let name = "Jack Sparrow";

function myNameIs() {
  name = "Donald Trump";
  console.log(`My name is ${this.name}!`);
}

myNameIs();
----------------
// My name is Donald Trump!
```

Does `this` refer to the function myNameIs() with a local variable `name="Donald Trump"`? No, `this` is still global, but the function has assigned global variable `name` a new value, "Donald Trump".

```
const name = "Donald Trump";

const person = {
  name: "SpongeBob",
  myNameIs: function x() {
    console.log(this.name)
  }
}

person.myNameIs();
----------------
// SpongeBob
```

In this example `myNameIs' is a method on object `person` hence `this` refers to the object itself and `this.name` is "SpongeBob". 

```
const name = "Donald Trump";

const person = {
  name: "SpongeBob",
  myNameIs: function x() {
    function y() {
      console.log(this.name)
    }
    y();
  }
}

person.myNameIs();
----------------
// Donald Trump
```
 
Here `this` is inside a function which is not an object's method. It refers to window object that has a global variable `name` with a value of "Donald Trump".








