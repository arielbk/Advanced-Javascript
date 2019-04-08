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