## node.js

Install [node.js](http://nodejs.org).

node.js comes with a package manager called `npm`.

## webpack

webpack can be installed through `npm`:

``` sh
$ npm install webpack -g
```

webpack is now installed globally and the `webpack` command is available.

## Use webpack in a project

It's the best to have webpack also as dependency in your project. Through this you can choose a local webpack version and will not be forced to use the single global one.

Add a `package.json` configuration file for `npm` with:

``` sh
$ npm init
```

The answers to the questions are not so important if you don't want to publish your project to npm.

Install and add `webpack` to the `package.json` with:

``` sh
$ npm install webpack --save-dev
```

You can continue reading [[Usage]].