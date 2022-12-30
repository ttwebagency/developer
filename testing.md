# Testing

+ [Gatsby](#gatsby)
    + [Setup](#setup)
    + [Configuration](#configuration)
    + [Running Tests](#running-tests)
    + [Code Coverage](#code-coverage)
    + [Writing Tests](#writing-tests)

## Gatsby
Unit testing is a great way to protect against errors in our code before we deploy it to production. Gatsby does not include support for `unit testing` out of the box. The most popular testing framework for React is `Jest`, which was created by Facebook. Jest is a general-purpose unit testing framework.

The Gatsby documentation on Unit Testing: `https://www.gatsbyjs.com/docs/how-to/testing/unit-testing/` further documentation can be found here: `https://danielabaron.me/blog/gatsby-unit-testing/`

### Setup
We need to begin by installing `Jest` and some required packages. This can be done using `npm`. We install `babel-jest` and `babel-preset-gatsby` to ensure the `babel` presets that are used to match what are used internally within our Gatsby site. 

```shell
$ npm install jest babel-jest react-test-renderer babel-preset-gatsby identity-obj-proxy --save-dev
```

It can be better to install the `React Testing Library (RTL)` instead of `react-test-renderer` as shown in the Gatsby documentation. This is because `react-test-renderer` will only render a component but does not support interactivity like clicking a button or entering text into an input field. `React Testing Library (RTL)` will provide more flexibility with interactions and querying the DOM.

Install the libraries using `npm` as `devDependencies`:

```shell
$ npm install jest babel-jest babel-preset-gatsby identity-obj-proxy @testing-library/jest-dom @testing-library/react @testing-library/user-event --save-dev
```

### Configuration
Because Gatsby handles its own `Babel` configuration, we will need to manually tell `Jest` to use `babel-jest`. The easiest way to do this is to add a `jest.config.js` file. If using `Jest 28` or above, we can skip adding the `moduleNameMapper` option for `gatsby-page-utils`, `gatsby-core-utils` and `gatsby-plugin-utils` as this was a bug that was fixed in Jest 28.

One additional step is to set the `testEnvironment` property to `jsdom`. Be default, its set to `node` and any tests that attempt to query the DOM will fail without this change. When using this option, an error was shown in the terminal:

```shell
Test environment jest-environment-jsdom cannot be found. Make sure the testEnvironment configuration option points to an existing node module.

Configuration Documentation:
https://jestjs.io/docs/configuration

As of Jest 28 "jest-environment-jsdom" is no longer shipped by default, make sure to install it separately.
```

To fix this issue, the [jest-environment-jsdom](https://www.npmjs.com/package/jest-environment-jsdom) `npm` package can be installed as a `devDependency`.

```shell
$ npm install jest-environment-jsdom --save-dev
```

Here is the Jest configuration file that is in the root of our project folder:

```javascript
// jest.config.js

module.exports = {
  testEnvironment: `jsdom`,
  transform: {
    "^.+\\.jsx?$": `<rootDir>/jest-preprocess.js`,
  },
  testPathIgnorePatterns: [`node_modules`, `\\.cache`, `<rootDir>.*/public`],
  transformIgnorePatterns: [
    `node_modules/(?!(gatsby|gatsby-script|gatsby-link)/)`,
  ],
  globals: {
    __PATH_PREFIX__: ``,
  },
  testEnvironmentOptions: {
    url: `http://localhost`,
  },
}
```

Here is the `jest-preprocess.js` file, also in the root of the project folder:

```javascript
// jest-preprocess.js

const babelOptions = {
  presets: [`babel-preset-gatsby`],
}

module.exports = require(`babel-jest`).default.createTransformer(babelOptions)
```

If using `Jest 26.6.3` or below, the last line needs to be changed to:

```javascript
module.exports = require("babel-jest").createTransformer(babelOptions)
```

Add the following line in the `.eslintrc.js` ESLint configuration file to allow the `describe`, `it` and `expect` syntax to be valid. Without this amendment, ESLint will flag these as issues:

```
"env": {
    "jest": true
}
```

### Running Tests
We can update the `test` script inside the `package.json` file to run the `jest` executable. The Jest library is installed in the `node_modules` directory of the project. When running `npm` scripts, `npm` will search the project's local `node_modules` directory for the binary, making it simple to use `npm` packages.

```javascript
// package.json

{
    "scripts": {
        "test": "jest"
    }
}
```

To run the tests using this script:

```shell
$ npm run test
```

A `snapshot` file will generate when the `test` script runs.

### Code Coverage
To generate a `code coverage report` using Jest, we use the `--coverage` flag:

```javascript
// package.json

{
    "scripts": {
        "test": "jest",
        "test:coverage": "jest --coverage"
    }
}
```

We can then generate/run the coverage report:

```shell
$ npm run test:coverage
```

### Writing Tests
Test files can either be put into a `__tests__` directory or we can put test files elsewhere, usually next to the component itself. Test files have the file extension of `.spec.js` or `.test.js`. For example, if we had a component named `heading.js` then the test file could be named `heading.test.js`.

```
src /
|__ components /
    |__ headings /
        |__ __snapshots__ /
            headings.test.js.snap
        headings.test.js
        index.js
```

This is a simple test file for a `Heading` component. The test file is named `headings.test.js` and will check if the component renders before generating a `snapshot` file. The snapshot file will be found inside the `__snapshots__` directory and named `headings.test.js.snap`.

```javascript
// src/components/headings/headings.test.js

import "@testing-library/jest-dom"

import { render } from "@testing-library/react"
import React from "react"

import Heading from "./index"

// eslint-disable-next-line no-undef
describe(`Heading`, () => {
  // eslint-disable-next-line no-undef
  it(`renders correctly`, () => {
    const container = render(<Heading level="1">This is a Heading</Heading>)
    // eslint-disable-next-line no-undef
    expect(container).toMatchSnapshot()
  })
})
```

Once ESLint has been configured:

```javascript
import "@testing-library/jest-dom"

import { render } from "@testing-library/react"
import React from "react"

import Heading from "./index"

describe(`Heading`, () => {
  it(`renders correctly`, () => {
    const container = render(<Heading level="1">This is a Heading</Heading>)
    expect(container).toMatchSnapshot()
  })
})
```
