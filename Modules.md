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