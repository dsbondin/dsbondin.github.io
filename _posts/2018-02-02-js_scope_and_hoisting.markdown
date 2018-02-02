---
layout: post
title:      "JS Scope and Hoisting"
date:       2018-02-02 16:29:39 +0000
permalink:  js_scope_and_hoisting
---

Even though I'm almost done with the Javascript section of the course, it turns out there are still some blanks I need to fill. Specifically scope and hoisting. I will not go into every detail as this info is abundant on Learn.co and other resources, I'll just concentrate on the tricky parts using examples. 

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
TypeError: greet is not a function
```

When the `console.log(greet())` gets executed, the engine doesn't know yet that `greet` is a function. It's still a hoisted undefined variable. 

To be continued...








