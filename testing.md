# Testing

+ [Introduction to Tests](#introduction-to-tests)
	+ [Snapshots](#snapshots)
+ [Gatsby](#gatsby)
    + [Setup](#setup)
    + [Configuration](#configuration)
    + [Running Tests](#running-tests)
    + [Code Coverage](#code-coverage)
    + [Writing Tests](#writing-tests)

## Introduction to Tests
To write a `unit test` we use the `to()` or `test()` block. We can optionally wrap our tests inside of a `describe()` block to group them together. Within our `*.test.js` or `*.spec.js` file, our code would look like this:

```javascript
describe('This is the optional description of the test', () => {
	test('This is a test', () => {
		// The test code goes here
	})
})
```

+ `describe()` - often used to create a `test suite` to describe what is being tested. This is included with `Jest`.
+ `test()` or `to()` - the test block containing the test code.

### Snapshots
A `snapshot` is used to tell us if the syntax of our code, line by line, has changed since the last test. The lines of code that have changed is known as the `diff`.

When a test runs, a `snapshot` file is created. On every run of the test, a new `snapshot` is created and this is compared to the previous `snapshot` to identify any changes in the code. The test will `pass` if the snapshot has not changed but will `fail` if it has changed. A failed snapshot does not mean out app isn't working as expected, only that our code has changed since the last time we ran the test.

We use the `toMatchSnapshot()` method to create a `snapshot` file. When we run a test, a `__snapshot__` folder is created containing a `*.test.snap.js` file. This is our `snapshot`.

We need to take care to not update the test without looking at what has changed.

If we want to `update` and `regenerate` a snapshot:

```shell
$ jest --updateSnapshot
```

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

+ `transform` - this section tells Jest that all `js` and `jsx` files need to be transformed using a `jest-preprocess.js` file in the project root. The `jest-preprocess.js` file is where we set up our `Babel` config.
+ `testPathIgnorePatterns` - this option tells Jest to ignore any tests in the `node_modules` or `.cache` directories.
+ `transformIgnorePatterns` - Gatsby includes un-transpiled `ES6` code. By default, Jest doesn't try to transform code inside `node_modules` so it will return an error. This is because `gatsby-browser-entry.js` isn't being transpiled before running in Jest. We can fix this by including the `gatsby` module here.

The `globals` section sets `__PATH_PREFIX__` which is usually set by Gatsby and which some components need. We need to also set `testURL` to a valid URL because some DOM APIs such as `localstorage` are unhappy with the default `about:blank`.

We can use the `setupFiles` array that lets us list files that will be included before all tests are run.

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
Code coverage is used as a measure to determine how much of our code is being tested. Often code coverage will highlight if the code we write has tests written. A high code coverage confirms that our code is being fully tested, but will also highlight where we need to write tests.

`Jest` has an integrated test coverage reporter built-in that requires no configuration and works with `ES6` JavaScript syntax.

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

Our `Heading` component within the `src/components/headings/index.js` file looks like this:

```javascript
import classNames from "classnames"
import PropTypes from "prop-types"
import React from "react"

const Heading = ({
  level,
  className = ``,
  headingSize = ``,
  headingSizeMedium = ``,
  headingSizeLarge = ``,
  children,
  ...others
}) => {
  const Tag = `h` + level

  const headingClasses = classNames({
    "text-xl": headingSize === `extraLarge`,
    "text-lg": headingSize === `large`,
    "text-md": headingSize === `medium`,
    "text-sm": headingSize === `small`,
    "text-xs": headingSize === `extraSmall`,
    "md:text-xl": headingSizeMedium === `extraLarge`,
    "md:text-sm": headingSizeMedium === `medium`,
    "lg:text-lg": headingSizeLarge === `large`,
    "lg:text-md": headingSizeLarge === `medium`,
  })

  return (
    <Tag className={classNames(headingClasses, className)} {...others}>
      {children}
    </Tag>
  )
}

Heading.propTypes = {
  level: PropTypes.oneOf([`1`, `2`, `3`, `4`, `5`, `6`]).isRequired,
  className: PropTypes.string,
  headingSize: PropTypes.string,
  headingSizeMedium: PropTypes.string,
  headingSizeLarge: PropTypes.string,
  children: PropTypes.node.isRequired,
}

export default Heading
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

If we expand the `headings.test.js` test file, it looks like this:

```javascript
import "@testing-library/jest-dom"

import { render } from "@testing-library/react"
import React from "react"

import Heading from "./index"

describe(`Test the <Heading /> component.`, () => {
  test(`The <Heading /> component renders as <h1> correctly.`, () => {
    const heading = render(
      <Heading level="1">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
  test(`The <Heading /> component renders as <h2> correctly.`, () => {
    const heading = render(
      <Heading level="2">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
  test(`The <Heading /> component renders as <h3> correctly.`, () => {
    const heading = render(
      <Heading level="3">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
  test(`The <Heading /> component renders as <h4> correctly.`, () => {
    const heading = render(
      <Heading level="4">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
  test(`The <Heading /> component renders as <h5> correctly.`, () => {
    const heading = render(
      <Heading level="5">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
  test(`The <Heading /> component renders as <h6> correctly.`, () => {
    const heading = render(
      <Heading level="6">The Home for Brand Management</Heading>
    )
    // Generate or check the snapshot file for a match
    expect(heading).toMatchSnapshot()
  })
})
```
