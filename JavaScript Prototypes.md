 # JavaScript Prototypes

## Instantiation patterns in JavaScript

First there is just creating an object from a simply function constructor. It returns a new object. This can be cumbersome because every property must be explicitly defined.

```js
const child = Object.create(parent);
```
This is a solution to that problem. Any lookup on `child` will first happen on the child object, if it is not found then it will continue its search in the parent constructor.

## The `prototype` function object

A `prototype` is a property that every JavaScript function has that points to an object.

It allows us to access a prototypes methods from any instantiation, without defining it in a separate object.

(At this point we are using `Object.create(animals.prototype)` to get the methods on the constructor).

## The `new` keyword

The `new` keyword makes things easier for us.

When we invoke a constructor with `new` it automatically adds the prototype methods, and creates a new `this` within the constructor function.

When I started learning JS, this was the first kind of instatiation I learnt:

```js
function Person (name) {
  this.name = name; // each instantation gets a new `this`
}

// defining a prototype method
Person.prototype.bio = function () {
  console.log(`My name is ${this.name}`);
}

const ariel = new Person('arielbk');
ariel.bio(); // logs 'My name is arielbk'
```

This is really just a crude version of actual classes in other languages, it's just contructed through a function.

## ES6 classes

React used their own constructor method `.createClass()` in the beginning. For a few years now they have been using ES6 classes for class components.

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  // Now we can define our class methods here 👌
  bio(name) {
    console.log(`My name is ${this.name}`);
  }
}
```

This looks so much nicer, doesn't it? I think it's important to remember that JavaScript is still a prototypal language, and not a class based language like Java. That means: ***this is just syntactic sugar***.

It is much more readable, but it is important to keep in mind what is really happening under the hood (as the instantiation patterns before this teach us).

## Native Prototypes

So when we create a new array, it is the same as instantiating the native prototype:
```js
const array = {};
const otherArray = new Array();
```
That's how the new array has access to all of the native array methods (`.split()`, `.push()`, `.sort()`, etc.) — as well as properties (`.length`).

## Some tricky stuff

If, for whatever reason, we want to get the prototype of an object, we can use the native `Object.getPrototypeOf()` method.

Methods on an objects prototype are enumerable by default (so a `for...in` loop will also loop through all of the prototype methods, when we probably don't actually want that).

We can then use `object.hasOwnProperty()` to check if the property actually exists on the object and not the prototype.



`instanceof` tells us whether an object is an instance of a constructor.
```js
arielbk instanceof Person // true
```

Arrow functions cannot be constructors because they have no `this` keyword — it will just throw an error!

Arrow functions actually have no constructor property.
