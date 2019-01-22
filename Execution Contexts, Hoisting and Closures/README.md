# Execution Contexts, Hoisting and Closures

## Execution Contexts
This is an important concept to grasp before even looking at hoisting and closures.
Tyler made a really handy tool to help understand the concept visually: [JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer)

The JavaScript engine first goes through a 'creation' phase, and then through an 'execution' phase.

In the creation phase, the `this` keyword is set, and all declared variables are first assigned an `undefined` value. In the global context, the global object is set (`window` or `global`, depending on the environment).



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

function doSomething() {
  console.log(arguments);
}
```