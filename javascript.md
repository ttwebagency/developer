# JavaScript

+ [Modules](#modules)
    + [CommonJS](#commonjs)
    + [ESModules](#esmodules)
    + [Default](#default)

## Modules
A module is a piece of code in a file that we can call and use from other files. Both `CommonJS` and `ESModules (ESM)` are two examples of widely used module types.

The `ES6 (ECMAScript 2015)` specification introduced `modules` to the JavaScript language. This allowed the use of `import` and `export` statements.

Modules are automatically interpreted in `strict mode`.

+ Modules do not add anything to the global `window` scope.
+ Modules always are in `strict` mode.
+ Loading the same module twice in the same file will have no effect, as modules are only executed once.
+ Modules require a server environment.

Modules are often used alongside bundlers like `Webpack` for increased browser support, but they are also available for use directly in browsers.

Because of the `CORS policy`, modules must be used in a server environment. This can be setup locally with `http-server`.

To ensure our code gets loaded as a module in `HTML` and not loaded as a regular script, add `type="module"` to the `<script>` tag. Any code that uses `import` or `export` must use this attribute.

```javascript
<script type="module" src="functions.js"></script>
```

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

In this example, we have a module named `functions.js` that allows the functions to be exported and available to any other module.

```javascript
export const sum(x, y) {
    return x + y
}

export const difference(x, y) {
    return x - y
}

export const product(x, y) {
    return x * y
}

export const quotient(x, y) {
    return x / y
}
```

Now to use these functions, we can `import` them into the code.

```javascript
import { sum, difference, product, quotient } from "./functions.js"
```

### Default
In the previous examples, we exported multiple named exports and imported them individually or as one object. Modules can contain a default export using the `default` keyword. A `default` export will not be imported with curly brackets, but will be directly imported into a named identifier.

```javascript
// functions.js
export default function sum(x, y) {
    return x + y
}
```

We can then import:

```javascript
import sum from "./functions.js"
```
