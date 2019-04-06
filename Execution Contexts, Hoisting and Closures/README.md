# Execution Contexts, Hoisting and Closures

## Execution Contexts
This is an important concept to grasp before even looking at hoisting and closures.
Tyler made a really handy tool to help understand the concept visually: [JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer)

The JavaScript engine first goes through a 'creation' phase, and then through an 'execution' phase for every context.

### In the **creation** phase:
- All declared variables are first assigned an `undefined` value.
- All functions in context are saved in memory.<br />
- The `this` keyword for the current context is also set.

Apart from this, somthing else will happen depending on the context:
- In the global context the creation phase will declase and assign a `global` or `window` object (depending on the environment).
- In a function context the creation phase will assign an `arguments` keyword object.

```javascript
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
 
## Global vs. function context execution

After the creation phase, the execution phase happens. This is where functions are invoked and each function has it's own function context. So the process repeats, the creation phase begins in the context of the function.


## The execution stack

So, as the execution context of a function is happening the context outside of it is still in process, in the execution phase... this is the execution stack ([JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer) is the best way to actually visualise this).

## Scope

This is where we get scope. If we call a variable inside of a function, the JS engine will look for a definition of that variable inside the function's execution context (the function's scope).

If it cannot find that variable it will look to the execution context above it in the stack (the parent scope).

If it still cannot find the variable in the global scope, only then will a `TypeError` be thrown.

## Closures

Closures are notoriously difficult to understand, but with knowledge of the execution stack and scope they start to make a lot more sense.

The way I've always seen closures demonstrated is through an adder function:
```javascript
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
