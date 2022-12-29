# Testing

+ [Gatsby](#gatsby)
    + [Setup](#setup)

## Gatsby
Unit testing is a great way to protect against errors in our code before we deploy it to production. Gatsby does not include support for `unit testing` out of the box. The most popular testing framework for React is `Jest`, which was created by Facebook. Jest is a general-purpose unit testing framework.

### Setup
We need to begin by installing `Jest` and some required packages. This can be done using `npm`. We install `babel-jest` and `babel-preset-gatsby` to ensure the `babel` presets that are used to match what are used internally within our Gatsby site. 

```shell
$ npm install jest babel-jest react-test-renderer babel-preset-gatsby identity-obj-proxy --save-dev
```

It can be better to install the `React Testing Library (RTL)` instead of `react-test-renderer` as shown in the Gatsby documentation. This is because `react-test-renderer` will only render a component but does not support interactivity like clicking a button or entering text into an input field. `React Testing Library (RTL)` will provide more flexibility with interactions and querying the DOM.

Install the libraries using `npm` as `devDependencies`:

```shell
$ npm install jest babel-jest babel-preset-gatsby identity-obj-proxy @testing-library/jest-dom testing-library/react testing-library/user-event --save-dev
```
