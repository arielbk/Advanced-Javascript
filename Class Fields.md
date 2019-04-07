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

There is a currently a [pull request](https://github.com/babel/proposals/issues/12) to bring this into Babel soon.
