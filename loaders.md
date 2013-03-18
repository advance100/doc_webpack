# loaders

Loaders are transformations you can apply on files. They are linked to the compiler, so they are executed in node.js.

They resolve similar to modules.

The developer can apply loaders to modules by prefixing them in the `require` call:

``` javascript
var moduleWithOneLoader = require("loader!module");

// Loader in current directory
require("./loader.js!module");

// multiple loaders
require("loader1/main!./loader2!module");
```

## parameters

Loaders accept query parameters:

``` javascript
require("loader?with=parameter!./file");
```

The format of the query string is up to the loader, so check the loader documentation.

## loaders by config

In many cases the correct loader can be inferred from the filename. Therefore loaders can be specified in the configuration:

``` javascript
// webpack.config.js or enhanced-require.config.js
module.exports = {
  module: {
    loaders: [
      { test: /\.coffee$/, loader: "coffee-loader" }
    ],
    preLoaders: [
      { test: /\.coffee$/, loader: "coffee-hint-loader" }
    ]
  }
};
```

If the loaders from the configuration is not suitable for a specific case it can be overwritten:

``` javascript
require("raw!./script.coffee"); // loaders, preLoaders and postLoaders are applied
// => coffee-hint-loader ! coffee-loader ! raw ! ./script.coffee
require("!raw!./script.coffee"); // only preLoaders and postLoaders are applied
// => coffee-hint-loader ! raw ! ./script.coffee
require("!!raw!./script.coffee"); // no loaders from configuration are applied
// => raw ! ./script.coffee
// The last case should only be used in generated code (by loaders generated)
```

Read more about [[writing loaders]].