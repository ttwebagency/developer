# TypeScript

+ [Configuration File](#configuration-file)
	+ [Create the Configuration File](#create-the-configuration-file)
	+ [Configuration Options](#configuration-options)
+ [Gatsby](#gatsby)
	+ [Install TypeScript](#install-typescript)
	+ [Rename Files](#rename-files)
	+ [Plugins](#plugins)

TypeScript is an `open-source` programming language developed and maintained by `Microsoft` and is a subset of `JavaScript`. TypeScript files usually have the file extension of `.ts` or `.tsx`.

## Configuration File
The TypeScript configuration file is named `tsconfig.json` and is placed in the root of the project folder. The configuration file has two uses:

+ Establish the root directory of the TypeScript project using the `include` property.
+ Override the default settings of the TypeScript compiler using the `compilerOptions` object.

### Create the Configuration File
To create the TypeScript configuration file, we can either create a file named `tsconfig.json` in the root of the project, or we can generate the configuration file using the terminal.

```shell
$ npx tsc --init
```

### Configuration Options
Once the `tsconfig.json` file has been created we can add configuration options to override the default settings of the TypeScript compiler. Configuration options can be found here: `https://www.typescriptlang.org/tsconfig`

+ `jsx` - this options sets how `JSX` syntax is handled in `.tsx` files. If we use the `preserve` option, this leaves `JSX` unchanged. This only affects output of JS files that are `.tsx` files.
+ `strict` - when this is set to `true`, this option enables TypeScript type-checking at `build time`. This does not run by default.
+ `resolveJsonModule` - when this is set to `true`, this allows modules with a `.json` extension, which is common practice in Node projects, to be imported. TypeScript does not support resolving JSON by default. Resolves the error: `Cannot find module './settings.json'. Consider using '--resolveJsonModule' to import module with '.json' extension.`
+ `noUnusedLocals` - when this is set to `true`, this reports errors on unused variables to show an error if a variable is declared but not used. Error example: `'variableNameHere' is declared but its value is never read.`
+ `noUnusedParameters` - when this is set to `true`, this reports errors on unused parameters in functions to show an error. Error example: `'parameterNameHere' is declared but its value is never read.`.
+ `module` - defines the `output` module resolution system that is used. For example, if we use `es2015` here, it would assume that our JavaScript engine is capable of parsing `es2015 (ES6)` import statements (this would fail in `Node version 8`). If we set this to `commonjs`, it would export the module using the `exports` property, so would run in `Node version 8` and later. This is sometimes set to `commonjs`.
+ `moduleResolution` - this configures how the compiler tries to find our modules and resolve them. This could be set to `classic` or `node`. This is usually set to `node` and this is TypeScript's default mode for module resolution. We can use `classic` for backwards compatibility.
+ `target` - this is related to the output of our code, the compiled code targets a specific language variant. If we specify `ES5`, the compiled code can be run by an `ES5` compliant browser and engines like Node. If we specify `ES6 (ES2015)`, the compiled code cannot be run by browsers or engines that do not support the `ES6 (ES2015)` syntax/features. Modern browsers support `ES6 (ES2015)` as does modern `Node` versions. Modern browsers support all `ES6` features, so `ES6` is a good choice - `https://www.typescriptlang.org/tsconfig#target`
+ `lib` - also relates to the output of the code. This tells the compiler which language features are available when the compiled code is run. In most cases, this would be the same as `target` except if we polyfilled the runtime environment. Example could be: `"lib": ["dom", "dom.iterable", "esnext"],`
+ `sourceMap` - enables the generation of `sourcemap files` allowing debuggers and other tools to display the original TypeScript source code when working with JavaScript files. Source files are emitted as `.js.map` or `.jsx.map` next to the corresponding `.js` output file. This option can be set to `true` to enable. So a file called `helloWorld.ts` will compile to `helloWorld.js` and create a sourcemap file called `helloWorld.js.map`
+ `allowSyntheticDefaultImports` - when set to `true`, allows the import to be written as `import React from "react"` rather than `import * as React from "react"` when the module does not explicitly specify a default export. An error would look like this: `Module '"/home/runner/work/TypeScript-Website/TypeScript-Website/utilFunctions"' has no default export.` - `https://www.typescriptlang.org/tsconfig#allowSyntheticDefaultImports`
+ `isolatedModules` - While we can use TypeScript to produce JavaScript code from TypeScript code, it is also common to use other transpilers such as `Babel` to do this. However, other transpilers only operate on a single file at a time which means they cannot apply code transforms that depend on understanding the full type system. This can cause runtime problems with some TypeScript features. Setting this flag to `true` tells TypeScript to warn us if we write certain code that cannot be correctly interpreted by a single-file transpilation process. This only changes the behaviour of TypeScript's checking and emitting process.
+ `allowJS` - Set to `true`, allows JavaScript files to be imported inside our project. Instead of just `.ts` and `.tsx` files. Will usually raise an error if not set to `true`.
+ `forceConsistentCasingInFileNames` - If set to `true`, allows TypeScript to check the case sensitivity rules of the file system. For example, an error will be thrown if `fileText.ts` is being imported using `FileTest.ts`.
+ `noFallthroughCasesInSwitch` - If set to `true`, reports errors for fallthrough cases in switch statements. This ensures that a switch statement does not have an empty-case including either `break` or `return` to avoid a `fallthrough bug` being introduced into the code.

One key take away is that these options are mostly about what TypeScript compiles `into` and not what it compiles `from`. This is different from `Babel` where both `input (parsing)` and `output (compilation)` settings exist, so no matter how we configure TypeScript, the language syntax remains the same.

## Gatsby

### Install TypeScript
To setup Gatsby to use `TypeScript` we first need to install the `typescript` package as a `devDependency`. This will allow us to use the `tsc` command in the terminal.

```shell
$ npm install typescript --save-dev
```

+ Run `npm run clean` or `gatsby clean` to clear the Gatsby cache and remove any artefacts.
+ Convert `.js` files to `.ts` and `.jsx` to `.tsx`.
+ Install the `@types` packages using `npm` or `yarn` as `devDependencies`.

```shell
$ npm install @types/node @types/react @types/react-dom --save-dev
```

+ `@types/react` - add type checking specifically for React.
+ `@types/react-dom` - add type checking specifically for React DOM.

We can then run the `develop` command to run the development server.

```shell
$ npm run develop
```

### Rename Files
Gatsby configuration files can be renamed to use TypeScript file extensions:

+ `gatsby-node.js` to `gatsby-node.ts`
+ `gatsby-config.js` to `gatsby-config.ts`
+ `gatsby-browser.js` to `gatsby-browser.tsx`
+ `gatsby-ssr.js` to `gatsby-ssr.tsx`

### Plugins
We can install the [gatbsy-plugin-typescript plugin](https://www.gatsbyjs.com/plugins/gatsby-plugin-typescript/) that allows Gatsby to run TypeScript files with the `.ts` and `.tsx` file extensions. This plugin is automatically included with Gatsby so we only need install it if we need to extend the default configuration options. This plugin does not run type checking during `build time`.
