In some cases webpack cannot parse some file, because it has a unsupported module format or isn't even in a module format. Therefore you have many options to convert the file into a module.

# Importing

The file has dependencies that are not required.

## [`imports-loader`](https://github.com/webpack/imports-loader)

This loader allows put some modules or arbitrary javascript onto a local variable of the file.

Examples: 

##### `file.js` expect a global variable `$` and you have a module `jquery` that should be used.

`require("imports?$=jquery!./file.js")`

##### `file.js` expect its configuration on a global variable `xConfig` and you want it do be `{value:123}`.

`require("imports?xConfig=>{value:123}!./file.js")`

> Note: you can also configure the loaders in your [[configuration]].

## [[plugin | list of plugins]] `ProvidePlugin`

This plugin makes a module available as variable in every module. The module is required only if you use the variable.

Example: Make `$` and `jQuery` available in every module without writing `require("jquery")`.

``` javascript
new webpack.ProvidePlugin({
	$: "jquery",
	jQuery: "jquery"
})
```



# Exporting

The file doesn't export its value.

## [`exports-loader`](https://github.com/webpack/exports-loader)

This loader exports variables from inside the file.

Examples:

##### The file sets a variable in the global context with `var XModule = ...`.

`var XModule = require("exports?XModule!./file.js")`

##### The file sets multiple variables in the global context with `var XParser, XMinimizer`.

`var XModule = require("exports?Parser=XParser&Minimizer!./file.js"); XModule.Parser; XModule.Minimizer`

##### The file sets a global variable with `XModule = ...`.

`require("imports?XModule=>undefined!exports?XModule!./file.js")` (import to not leak to the global context)



# Exposing

There are cases where you want a module to export itself to the global context.

Don't do this if you don't really need this

## [`expose-loader`](https://github.com/webpack/expose-loader)

This loader expose the exports to a module to the global context.

Example: 

##### Expose `file.js` as `XModule` to the global context

`require("expose?XModule!./file.js")`