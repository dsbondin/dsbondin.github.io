---
layout: post
title:      "Prototypal Inheritance in JavaScript"
date:       2018-05-07 02:13:55 +0000
permalink:  prototypal_inheritance_in_javascript
---


There is an interesting feature in JavaScript that makes it different from all other programming languages: Prototypal Inheritance. The difference from Class Inheritance is really simple: classes inherit from other classes (its parents) while in Prototypal Inheritance objects inherit directly from it's prototypes. 

So what is a prototype and why is it important? Consider the following example: 

```
function Rectangle(width, height) {
 this.width = width;
 this.height = height;
}

Rectangle.prototype.area = function() {
 return this.width * this.height;
}

let rect = new Rectangle(4, 5);
rect.area();
// 20
```

Now lets say we want to create a Square object. A square is a rectangle where `width = height`:

```
function Square(size) {
 this.width = this.height = size;
}
```

Now here's the interesting part: 

```
Square.prototype = Object.create(Rectangle.prototype);
```

We know that `Rectangle.prototype` has a property `.area`. So by creating a clone of `Rectangle.prototype` as a `Square.prototype` object our `Square` object is inheriting properties of `Rectangle`. 

```
var square = new Square(6);
square.area();
// 36
```

Happy coding!


