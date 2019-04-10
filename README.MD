# Advanced JavaScript
Following along with Tyler McGinnis' [*Advanced JavaScript*](https://tylermcginnis.com/courses/advanced-javascript/) tutorial series.

These are just intended as supplementary materials. Go check out the course! Tyler McGinnis has a teaching style where he will go into the details of why things are the way they are in JavaScript, 'look under the hood', but at the same time has a way of getting confusing concepts across simply.

There are also [blog posts](https://tylermcginnis.com/blog/) on most of these concepts, which probably do a better job of explaining some of these things than I do here.

I am trying to get a better understanding of these things by explaining them in my own terms and putting them in context. Most, apart from the most essential, code examples have been changed.

<br>

## Table of contents

- [Advanced JavaScript](#advanced-javascript)
  - [Table of contents](#table-of-contents)
- [Execution Contexts, Hoisting and Closures](#execution-contexts-hoisting-and-closures)
  - [Execution Contexts](#execution-contexts)
    - [The **creation** phase:](#the-creation-phase)
    - [The **execution** phase](#the-execution-phase)
  - [The execution stack](#the-execution-stack)
  - [Scope](#scope)
  - [Closures](#closures)
- [The `this` keyword in JavaScript](#the-this-keyword-in-javascript)
  - [Implicit binding](#implicit-binding)
  - [Explicit binding](#explicit-binding)
    - [the `.call()` method](#the-call-method)
    - [`.apply()` method](#apply-method)
    - [`.bind()` method](#bind-method)
  - [`new` binding](#new-binding)
  - [`window` binding](#window-binding)
- [Async JavaScript](#async-javascript)
  - [Some solutions](#some-solutions)
  - [Promises](#promises)
  - [Async / Await](#async--await)
    - [Error handling with Async/Await](#error-handling-with-asyncawait)
- [JavaScript Prototypes](#javascript-prototypes)
  - [Instantiation patterns in JavaScript](#instantiation-patterns-in-javascript)
  - [The `prototype` function object](#the-prototype-function-object)
  - [The `new` keyword](#the-new-keyword)
  - [ES6 classes](#es6-classes)
  - [Native Prototypes](#native-prototypes)
  - [Some tricky stuff](#some-tricky-stuff)
- [Private and Public Class Fields](#private-and-public-class-fields)
  - [Class field declarations proposal in ESNext](#class-field-declarations-proposal-in-esnext)
  - [React](#react)
  - [Downsides?](#downsides)
  - [Private fields](#private-fields)
- [JavaScript Inheritance and the Prototype Chain](#javascript-inheritance-and-the-prototype-chain)
  - [Chaining inheritance](#chaining-inheritance)
  - [Inheritance with ES6 Classes](#inheritance-with-es6-classes)
- [JavaScript Composition vs. Inheritance](#javascript-composition-vs-inheritance)
  - [Potential problems with object oriented programming](#potential-problems-with-object-oriented-programming)
- [JavaScript Modules](#javascript-modules)
  - [Modular design](#modular-design)
  - [Implementing modules](#implementing-modules)
    - [Leading through the evolution of modules in JavaScript](#leading-through-the-evolution-of-modules-in-javascript)
  - [Global App component](#global-app-component)
  - [IIFE Module Pattern](#iife-module-pattern)
  - [Common JS](#common-js)
    - [Some problems](#some-problems)
    - [Module bundlers](#module-bundlers)
  - [ES Modules](#es-modules)
- [Create your own array](#create-your-own-array)
  - [Defining the array prototype](#defining-the-array-prototype)
  - [Defining the `.push()` method](#defining-the-push-method)
  - [Defining the `.pop()` method](#defining-the-pop-method)
  - [Defining the `.filter()` method](#defining-the-filter-method)
  - [Other higher order functions](#other-higher-order-functions)
    - [Defining the `.map()` method](#defining-the-map-method)
    - [Defining the `.find()` method](#defining-the-find-method)
    - [Defining the `.reduce()` method](#defining-the-reduce-method)

<br>

# Execution Contexts, Hoisting and Closures

## Execution Contexts
This is an important concept to grasp before even looking at hoisting and closures.
Tyler made a really handy tool to help understand the concept visually: [JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer)

The JavaScript engine first goes through a 'creation' phase, and then through an 'execution' phase for every context.

### The **creation** phase:
- All declared variables are first assigned an `undefined` value.
- All functions in context are saved in memory.<br />
- The `this` keyword for the current context is also set.

Apart from this, somthing else will happen depending on the context:
- In the global context the creation phase will declase and assign a `global` or `window` object (depending on the environment).
- In a function context the creation phase will assign an `arguments` keyword object.

```js
console.log('name: ', name);
console.log('country: ', country);
console.log('getDetails: ', getDetails);

var name = 'Ariel', country = 'Croatia';

function getDetails() {
  return {
    name,
    country
  }
}
```
Because of these phases this code will log:
```
name:  undefined
country:  undefined
getDetails:  ƒ getDetails() {
  return {
    name,
    country
  }
}
```
 
### The **execution** phase

After the creation phase, the execution phase happens. This is where functions are invoked and each function has it's own function context. So the process repeats, the creation phase begins in the context of the function.


## The execution stack

So, as the execution context of a function is happening the context outside of it is still in process, in the execution phase... this is the execution stack.

[JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer) is the best way to actually visualise this.

## Scope

This is where we get scope. If we call a variable inside of a function, the JS engine will look for a definition of that variable inside the function's execution context (the function's scope).

If it cannot find that variable it will look to the execution context above it in the stack (the parent scope).

If it still cannot find the variable in the global scope, only then will a `TypeError` be thrown.

## Closures

Closures are notoriously difficult to understand, but with knowledge of the execution stack and scope they start to make a lot more sense.

The way I've always seen closures demonstrated is through an adder function:
```js
function makeAdder (x) {
  return function adder (y) {
    return x + y;
  }
}

// this returns a new function
 var addTwo = makeAdder(2);

 addTwo(3); // returns 5
```
The inner function (`adder`) has a closure over execution context of the outer function (`makeAdder`). That is, a new *closure scope* is created.

So, 2 is passed into the `makeAdder` function as the value of x. That value is saved in its own closure scope as the value for x whenever the returned function is called (since it is assigned to the variable `addTwo`).

Run it through the [JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer)!

<br>

# The `this` keyword in JavaScript

What the `this` keyword *is* all depends on *where the function is invoked.*

## Implicit binding

This is the simplest and probably most common `this` binding. Basically, just look *left of the dot* from where the function is invoked.<br>
```js
const dog = {
  name: 'Benji',
  noise: 'Bark!',
  makeNoise: function () {
    console.log(this.noise);
  }
}

dog.makeNoise(); // logs 'Bark!'
```

## Explicit binding
This takes precedence over implicit binding.

### the `.call()` method
```js
myFunction.call(myObj, arg1, arg2, arg3);
```
This calls the `myFunction` function with explicit binding to `myObj` (so `this` refers to `myObj`). The next arguments are passed to the original function separately.

### `.apply()` method
Just like the `.call()` method, except the arguments are passed in one array:
```js
myFunction.call(myObj, [arg1, arg2, arg3]);
```

### `.bind()` method
Same as `.call()` except it returns a new bound function.<br>
This would be useful for when we want to reuse the bound function again; think back to the React pattern of binding functions in the constructor. 

## `new` binding

Creating an object from a constructor using the `new` keyword automatically assigns it its own `this` object.

## `window` binding

This is not great, but it happens. If `this` is not found In the ways outlined above, then `this` will default to the `window` object.

This will throw an `TypeError` in `strict mode` though.

<br>

# Async JavaScript

***Higher order functions*** are functions that take another function as an argument.

The ***callback function*** is the function that gets passed into the higher order function.

Callbacks are everywhere:
- The built-in `.map()`, `.reduce()`, `.find()`, etc. higher order functions
- Libraries like lodash
- jQuery — this is a good example of asynchronous callbacks

The asynchronous nature of callbacks can lead to an endless cascade of callback functions that trigger one another in a huge unreadable waterfall of code... ***callback hell!***

## Some solutions
Modularize your code (split things into their own functions) — although this can still be unintuitive.

A problem with passing callback functions to a 3rd party library is that it gives the control over to that library to deal with the callback responsibly.

## Promises
```js
function onSuccess () {
  console.log('🙌');
}

function onError () {
  console.log('👎');
}

var promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    // resolve(); // set promise to fulfilled
    reject(); // reject promise
  }, 2000);
});

promise
  .then(onSuccess)
  .catch(onError);
```

The real power of promises comes with *chaining* (`.then().then().then()`). Every step is transformed into a promise.

If we need to pass things along the chain (other than the just the returned value of the previous promise) then we simply pass it into the `resolve()` function of the promise to pass on.

## Async / Await

If we prepend `async` to a function it is *always* going to return a promise, even if there is an explicitly set return value.

```js
async function add(x, y) {
  return x + y;
}

// it returns a promise, so we need `.then()` to get the return value
add(2, 3).then(result => console.log(result));
```

### Error handling with Async/Await
Either use a .catch() where the function is invoked<br>
If that's not possible then we can use a try / catch block within the function itself.

<br>

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

<br>

# Private and Public Class Fields

## Class field declarations proposal in ESNext

Wouldn't it be nice not to have to deal with assigning all of our class fields in the constructor with `this.something = something`.

This proposal is at the time of writing in stage 3 (meaning it will be coming soon to stable JavaScript):
[TC39 Proposal](https://github.com/tc39/proposal-class-fields).

It's already in Babel — that's why it's being used all the time in React land now!

## React

Defining the state in constructor? With Babel we can now just declare state: `state = {}`.

Another part of this stage 3 TC39 proposal is defining static objects in the class body. This is now being used a lot with
```js
static propTypes = {};
static defaultProps = {};
```

Another thing is using arrow functions in the class rather than binding them in the `constructor()` function at the top. This allows us to get rid of the constructor function altogether.

## Downsides?

There is a *potential* performance hit when we use arrow functions within a class. Whether it's worth it is really dependent.

Whereas normally the function would be stored once in the prototype of the object, with arrow functions they are *newly created* functions that are assigned to a property within the class.


## Private fields

```js
class Car {
  #milesDriven = 0;
  drive(distance) {
    #milesDriven += distance;
  }
  getMilesDriven() {
    return #milesDriven;
  }
}
```

This is a feature that allows us to make some fields private. So, an instance can only access `milesDriven` through the `milesDriven()` class method here. This is extremely useful for security.

There is a currently a [pull request](https://github.com/babel/proposals/issues/12) to bring this into Babel.

<br>

# JavaScript Inheritance and the Prototype Chain

## Chaining inheritance

This is really tricky stuff using pre-ES6 JavaScript, but good to understand a little bit more about what is going on under the hood.

Here is my own contrived example, to get an idea of how it is done:

```js
// This is the parent class
function Vehicle (hasMotor, color) {
  this.hasMotor = hasMotor;
  this.color = color;
}
// Add a method to the Vehicle prototype
Vehicle.prototype.move = function() {
  console.log('moving!');
}

// Now we begin to chain this class
// Car is a subclass of Vehicle
function Car (color, make) {
  // allows the `this` to be bound to the Vehicle constructor function
  // sets hasMotor to true
  Vehicle.call(this, true, color);
  this.make = make;
}

// Allows lookup to continue up the chain
Car.prototype = Object.create(Vehicle.prototype);

// Adding a method specific to just the Dog prototype
Car.prototype.honk = function() {
  console.log(`beep! I'm a ${this.color} ${this.make}`);
}

// This is necessary, otherwise it will point to the Vehicle constructor 
Car.prototype.constructor = Car;

const myCar = new Car('blue', 'mercedes');

// Calls a method inherited from the grandparent Vehicle class
console.log(myCar.move()); // logs 'moving!'
// Calls a method inherited from the parent Car class
console.log(myCar.honk()); // logs 'beep! I'm a blue mercedes'
```

That was a lot to wrap my head around and definitely not simple or intuitive. It gives me a deep appreciation for ES6 classes abstracting it away.

## Inheritance with ES6 Classes

```js
class SubClass extends Parent {
  constructor(arg1, arg2) {
    super(arg1, arg2);
    this.arg1 = arg1;
    this.arg2 = arg2;
  }

  classMethod() {
    console.log('Simple as that!');
  }
}
```

<br>

# JavaScript Composition vs. Inheritance

## Potential problems with object oriented programming
> The problem with object oriented programming languages is they've got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding a banana and the entire jungle. — **Joe Armstrong**, creator of *Erlang*

Rather than seeing our individual components as things that *are* something, what if we view them as things that *do* something.

Instead of coupling methods to objects, we pull them apart and assign objects with methods (the ability to do certain things) with reusable functions.

```js
function Dog (name, breed) {
  let dog = { name, breed };

  return Object.assign(
    dog,
    eater(dog),
    barker(dog),
  )
}
```
So we pass the state of the object to the function and it becomes a method of the class. This way it is not tied closely to the `Dog` class, it is just something that it has the ability to connect with.

<br>

# JavaScript Modules

## Modular design

The design of a wristwatch is a great example to showcase the concept of modular design. Think about the number of connected parts. The tiny cogs that serve a simple function on their own but contribute to the whole complex system.

Some benefits of modular design:

- **Reusability** — Individual specialized pieces can be used again and again
  
- **Composability** — It's easier to fit together these individual pieces, as well as delete them
  
- **Leverage** — Ability to outsource particular pieces
  
- **Isolation** — Different developers can work on different parts of the code, pieces can be tested for their specific function
  
- **Organisation** — Modules provide an obvious point of separation between thing

Modularity in programming:
- Imports (*dependencies*)
- Code
- Exports

NPM packages are a well-known example of modules that have been packaged and shared. With it we can focus on how the pieces work together, we can build on the shoulders of giants for some things so that we don't get caught up in the smaller implementation details.

## Implementing modules

### Leading through the evolution of modules in JavaScript

Intuition may tell us to just separate code into files. This is a good start but if we do that and just import the files into the html then everything uses the global namespace.

This is dangerous because we could easily run into naming collisions, especially as an app gets larger, or if we want to bring in more and more modules.

## Global App component

We could wrap each module in a function. This would scope the namespace to the function of the module. Each module would have its own namespace and only the wrapper itself would be exposed.

This is better, but still the namespace is polluted with the name of every module (every function wrapper). 

## IIFE Module Pattern

Remember, an IIFE is an immediately invoked function expression:
```js
(function () {
  console.log('do something here');
})()
```
If we make all of the modules an IIFE along with the global app component, then we don't even pollute the global name space with the wrapper functions.

So now there is only the main app component taking up the global namespace. This could potentially lead to naming collisions if we decide to export our entire app as a module, or import another library.

Another issue is that the order of loading in the script tags matters, so it could get tricky — especially working with a team.

So far it is the best solution...

## Common JS

```js
// importing a module
const myModule = require('myModule');

// exporting within a module
module.exports = { myModule }
```

This is familiar because Common JS is baked into Node itself.

### Some problems
There are a couple of problems with Common JS.

Browsers don't support it natively and it's synchronous (this means that it is blocking and could lead to performance issues).

This is where a module bundler comes in!

### Module bundlers

The module bunders brings in all the modules and creates a single *intelligently* bundled js file.

**Webpack** is a well-known example of a module bundler.

Basic `webpack.config.js`:
```js
var path = require('path');

module.exports = {
  entry: 'dom.js',
  output: {
    path: path.resolve(__dirname),
    filename: 'bundle.js'
  },
  mode: 'development' 
}
```

With that, we can just import a single bundled JS file into our HTML.

## ES Modules

This is a part of ES6 and is supported by most modern web browsers apart from IE11.

What's great is that it supports async.

This is what we use in React now.

```js
// default import
import React from 'react';
// named import
import { Component } from 'react';

// default export
export default const defaultExport = 'export something';
// named export
export const namedExport = 'export something else';
```

I haven't used es6 modules outside of React, but to use it in a page we actually do this:

```js
<script type="module" src="/app.js"></script>
```

<br>

# Create your own array

So basically, this is a practice exercise to practice some of the ideas from this course.

The goal is to create an Array prototype with the following methods on it:
- `.push()`
- `.pop()`
- `.filter()`

I think I will add some of my own for practice, continuing on with some of these 'higher order methods'.

First things first, an array is really just a fancy kind of object. Arrays just have numerical keys and their own special methods.

## Defining the array prototype

Defining the length property is a tricky trick here, but comes in handy for the higher order methods further along. It means that when numerating through the array, the `length` property will be skipped.
```js
function array () {
  // look to array prototype on failed lookup
  let arr = Object.create(array.prototype);

  // this makes sure that the length property of arrays are non-numerable
  Object.defineProperty(arr, 'length', {
    value: 0,
    enumerable: false,
    writable: true
  });

  for (key in arguments) {
    // assign the argument to the numerical key
    arr[key] = arguments[key];
    // and increment the length property
    arr.length += 1;
  }

  return arr;
}
```

## Defining the `.push()` method

This is relatively simple, add the element, increment the length and return it:
```js
array.prototype.push = function (element) {
  // add on the element
  this[this.length] = element;
  // increment the length
  this.length++;

  // return the length
  return this.length;
}
```

## Defining the `.pop()` method

Pop is along the same lines, but the element is not passed in, we are just taking the last item from the stack:
```js
array.prototype.pop = function () {
  // decrement the length
  this.length--;
  // save the element to remove to return it
  const elementToRemove = this[this.length];
  // remove the last element
  delete this[this.length];

  //  return the element that was removed
  return elementToRemove;
}
```

## Defining the `.filter()` method

This is where things get a little tricky. Still, it's relatively straight forward.

It loops through every key in the array, making sure it belongs to that instance of the array itself, and if it returns `true` when it runs through the callback, then it is added to the results array.
```js
array.prototype.filter = function (cb) {
  // the results will be a new array
  let results = array();

  for (let index in this) {
    // making sure the property belongs to this instance
    if (this.hasOwnProperty(index)) {
      const element = this[index];

      // if running the callback returns truthy value
      if (cb(element, index)) {
        result.push(element);
      }
    }
  }

  // finally, return the result
  return results;
}
```

## Other higher order functions

Let's give this a try: implementing some of  the more common array higher order functions.

### Defining the `.map()` method

The map method will iterate every value in the array and return a new array with every value passed through a callback function.

```js
array.prototype.map = function (cb) {
  // result will be a new array
  let results = array();

  for (let index in this) {
    // making sure the property belongs to this instance
    if (this.hasOwnProperty(index)) {
      const element = this[index];

      // assign whatever is returned from the cb
      results.push(cb(element, index));
    }
  }

  return results;
}
```

### Defining the `.find()` method

Iterates through the array values, runs the value through a callback and returns the first value that resolves to true.

```js
array.prototype.find = function (cb) {
  // result will be a new array
  let results = array();

  for (let index in this) {
    // making sure the property belongs to this instance
    if (this.hasOwnProperty(index)) {
      const element = this[index];

      // if cb resolves to true, return and end
      if (cb(element, index)) return element;
    }
  }

  // if nothing was found, return undefined
  return undefined;
}
```

### Defining the `.reduce()` method

[MDN docs for `reduce()` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

IMO this is the hardest array method to understand, so it is great practice to break down the functionality of it a little bit.

The reduce method can be passed two arguments:
- The reducer callback function *(required)*
- An initial value *(optional, defaults to 0)*

The reducer callback function can be passed four arguments:
1. Accumulator (`acc`) *(required)*
   - The value of the accumulator persists through each iteration, and is eventually what gets returned
2. Current value (`cur`) *(required)*
   - The current value in the iteration
3. Current index (`idx`) *(optional)*
   - The current index in the iteration
4. Source array (`src`) *(optional)*
   - The original array that the reduce method is being run on

```js
array.prototype.reduce = function (reducer, initialValue) {
  // define the source array
  const src = this;
  // accumulator is derived from initialValue
  let acc = initialValue || 0;

  for (let idx in this) {
    // making sure the property belongs to this instance
    if (this.hasOwnProperty(idx)) {
      // define current value
      const cur = this[idx];

      // all variables are now set
      // run the reducer on accumulator
      acc = reducer(acc, cur, idx, src);
    }
  }
  // return the accumulator
  return acc;
}
```

🙌 🚀