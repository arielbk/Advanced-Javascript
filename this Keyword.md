# The `this` keyword in JavaScript

What the `this` keyword *is* all depends on *where the function is invoked.*

## implicit binding

This is the simplest and probably most common `this` binding. Basically, just look *left of the dot* from where the function is invoked.<br>
```javascript
const dog = {
  name: 'Benji',
  noise: 'Bark!',
  makeNoise: function () {
    console.log(this.noise);
  }
}

dog.makeNoise(); // logs 'Bark!'
```

## explicit binding

### the `.call()` method
For example:
```javascript
myFunction.call(myObj, arg1, arg2, arg3);
```
This calls the `myFunction` function with explicit binding to `myObj` (so `this` refers to `myObj`). The next arguments are passed to the original function separately.
### `.apply()` method
Just like the `.call()` method, except the arguments are passed in one array:
```javascript
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
