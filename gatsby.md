# Gatsby

+ [Install and Setup](#install-and-setup)
+ [Development](#development)
+ [TypeScript](typescript.md#gatsby)
+ [SEO](seo.md#gatsby)

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
Once the Gatsby website has been installed and created, we can run the local `development` server to being working on the site locally.

```shell
$ npm run develop
```

This will start the development server and will open the Gatsby site in a browser window/tab: `http://localhost:8000/`. To stop the development server running in the terminal, hit `CTRL` + `C` on the keyboard.
