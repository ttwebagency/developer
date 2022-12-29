# Gatsby

+ [Install and Setup](#install-and-setup)
+ [Development](#development)
+ [TypeScript](typescript.md#gatsby)
+ [SEO](seo.md#gatsby)
+ [Testing](testing.md#gatsby)

## Install and Setup
To install `Gatsby` we first need to install the `Gatsby CLI`. This should be installed globally and this will allow us to use the `gatsby` command in the terminal to create Gatsby sites.

```shell
$ npm install gatsby-cli -g
```

Once installed and completed, check the version of `Gatsby` installed:

```shell
$ gatsby --version
```

To create a new Gatsby site:

```shell
$ npm init gatsby
```

Follow the options within the terminal to configure the Gatsby site.

## Development
Once the Gatsby website has been installed and created, we can run the local `development` server to being working on the site locally. The `development` option compiles the code and this is handled by the browser. When we run the `build` option, it is compiled by the server, so it's always recommended to fully test using the `develop` and `build` options as not all options or functionality will be available in the browser and using `build` will also check for any errors when before deployment, for instance when deploying to `Gatsby Cloud` or `Netlify`.

```shell
$ npm run develop
```

This will start the development server and will open the Gatsby site in a browser window/tab: `http://localhost:8000/`. To stop the development server running in the terminal, hit `CTRL` + `C` on the keyboard.

Gatsby can be run in `debug` mode by using the `debug` flag when starting the development server.

```shell
$ npm run develop --debug
```
