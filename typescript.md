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
