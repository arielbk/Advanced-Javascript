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