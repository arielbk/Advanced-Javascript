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