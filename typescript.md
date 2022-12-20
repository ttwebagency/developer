# TypeScript

+ [Configuration File](#configuration-file)
	+ [Create the Configuration File](#create-the-configuration-file)
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
We can install the [gatsby-plugin-typescript](gatbsy-plugin-typescript plugin) that allows Gatsby to run TypeScript files with the `.ts` and `.tsx` file extensions. This plugin is automatically included with Gatsby so we only need install it if we need to extend the default configuration options. This plugin does not run type checking during `build time`.
