# JavaScript

+ [Modules](#modules)
    + [CommonJS](#commonjs)
    + [ESModules](#esmodules)

## Modules
A module is a piece of code in a file that we can call and use from other files. Both `CommonJS` and `ESModules (ESM)` are two examples of widely used module types.

The `ES6 (ECMAScript 2015)` specification introduced `modules` to the JavaScript language. This allowed the use of `import` and `export` statements.

Modules are automatically interpreted in `strict mode`.

### CommonJS
CommonJS is a standard used to implement modules in JavaScript from a project developed by Kevin Dangoor at Mozilla in 2009. This was for use in Node projects where Node only supported `CommonJS` at the time, but Node can also now support `ESModules (ESM)` which is a more modern approach.

`module.exports` is the keyword we use to declare all we want to export from the file.

```javascript
// module.js

const myFunc = () => {
    console.log("This is a function")
}

module.exports = myFunc
```

If we want to export more than one thing from a single module, we can do it like this:

```javascript
// module.js

const myFunc = () => {
    console.log("This is a function")
}

const anotherFunc = () => {
    console.log("Another function")
}

module.exports = { myFunc, anotherFunc }
```

To use this function in a JavaScript file, we use the `require` keyword.

```javascript
myFunc = require("./module.js")

const mainFunction = () => {
    myFunc()
}
```

If we want to import multiple functions from the module:

```javascript
({ myFunc, anotherFunc }) = require("./module.js")

const mainFunction = () => {
    myFunc()
    anotherFunc()
}
```

### ESModules
ESModules (ESM) were introduced with `ES6 (ES2015)` to help standardise how JavaScript modules work and to implement the features in browsers. ESModules are supported by browsers and server-side apps with Node. ESModules (ESM) are native to JavaScript.

Within the `package.json` file, add `"type": "module"`:

```javascript
// package.json
{
    "type": "module"
}
```

We may get an error if we try to implement ESModules (ESM) on Node without using the `type` property.

To create an `ESModule (ESM)` in JavaScript:

```javascript
// module.js

const myFunc = () => {
    console.log("This is a function")
}

export { myFunc }
```

To import the module:

```javascript
import { myFunc } from "./module.js"

const mainFunction = () => {
    myFunc()
}
```
