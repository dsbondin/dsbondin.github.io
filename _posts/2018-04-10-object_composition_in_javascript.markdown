---
layout: post
title:      "Object Composition in JavaScript"
date:       2018-04-10 14:20:40 +0000
permalink:  object_composition_in_javascript
---


There's a famous saying *'favor composition over inheritance'* that, in case of JavaScript, should sound *'favor composition over classical inheritance'*. As we know, one of the JavaScript's distinctive features is prototypal inheritance but it's a subject for a whole separate post. Here I'd like to show why composition is more flexible than classical inheritance and how it can be implemented in JS. 

Classical inheritance is one of the pillars of object oriented programming. A subclass inherits all properties and behavior from it's parent class which makes it easy to reuse code that's already been written. However this great feature, if used abusively, can lead to instances that inherit more than they need. Also fixing bugs becomes harder as they can hide all the way in the root parent. In the example I'm going to show classical inheritance also becomes problematic due to the nature of the instance object we need to create. 

Lets say we have two classes: a Dog and a Robot class. An instance of a Dog can bark(), run() and eat(). A Robot instance can charge. What if we want to create a robotDog instance that can bark(), run() and charge()? Do we inherit from a Dog or a Robot class? Our robotDog can't eat so we can't inherit from a Dog class. But if we inherit from Robot we'll need to add bark() and run() methods which are already written for the Dog class. We repeat ourselves. Meet composition: 

```
const barker = function(name) {
  return {
    bark: function() {
      console.log(`Woof, I am ${name}!`)
    }
  }
}

const runner = function(position, distance) {
  return {
    run: function() {
      return position += distance
    }
  }
}

const charger = function() {
  return {
    charge: function() {
      console.log("I'm fully charged!")
    }
  }
}
```

Here we are creating 3 factories that return 3 different objects with methods: bark(), run() and charge(). Our robotDog will later be composed of these methods. 

```
const robotDog = function(name, position, distance) {
  this.name = name;
  this.position = position;
  this.distance = distance;

  return Object.assign(
    {},
    barker(this.name),
    runner(this.position, this.distance),
    charger()
  );
}
```

And here is the actual robotDog() blueprint that will be used to instantiate a robotDog with a name, initial position and distance to run. It then returns an object composed of the previously created objects barker, runner and charger. 

Lets instantiate a new robotDog and test it: 

```
const fido = new robotDog("Fido", 0, 5);

fido.bark() // Woof, I am Fido!
fido.run() // => 5
fido.run() // => 10
fido.charge() // I'm fully charged!
```

Voila! 
Happy coding!





