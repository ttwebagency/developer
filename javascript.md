# JavaScript

+ [Modules](#modules)
    + [CommonJS](#commonjs)

## Modules
A module is a piece of code in a file that we can call and use from other files. Both `CommonJS` and `ESModules (ESM)` are two examples of widely used module types.

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
