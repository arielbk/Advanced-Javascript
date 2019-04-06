# Async JavaScript

***Higher order functions*** are functions that take another function as an argument.

The ***callback function*** is the function that gets passed into the higher order function.

Callbacks are everywhere:
- The built-in `.map()`, `.reduce()`, `.find()`, etc. higher order functions
- Libraries like lodash
- jQuery — this is a good example of asynchronous callbacks

The asynchronous nature of callbacks can lead to an endless cascade of callback functions that trigger one another in a huge unreadable waterfall of code... ***callback hell!***

### Some solutions
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
