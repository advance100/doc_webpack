# webpack options

``` javascript
module.exports = {
  context: "/home/proj",
  entry: "./entry",
  module: {
    loaders: [
      test: /\.coffee$/,
      include: /lib/, exclude: /test/,
      loader: "coffee"
    ],
    preLoaders: [/*...*/],
    postLoaders: [/*...*/]
  },
  output: {
    path: "/home/proj/assets",
    filename: "[hash].bunde.js",
    chunkFilename: "[id].[hash].bundle.js",
    namedChunkFilename: "[name].[hash].js",
    publicPath: "/assets/",
    jsonpFunction: "webpackJsonp",
    pathInfo: true,
    library: "Lib",
    libraryTarget: "commonjs"
  },
  bail: true,
  console: true,
  cache: true,
  watch: true,
  watchDelay: 200,
  debug: true,
  devtool: "eval",
  amd: { jQuery: true },
  resolve: {
    alias: {
      module: "other-module",
      module2: "/home/proj/shim-module.js"
    },
    modulesDirectories: ["module", "node_modules"],
    extensions: ["", ".client.js", ".js"]
  },
  resolveLoader: {/*...*/},
  optimize: {
    maxChunks: 5,
    minimize: true
  },
  plugins: [
    new MyPlugin()
  ]
}
```

## `context`

The base directory for resolving the `entry` option. If `output.pathinfo` is set, the included pathinfo is shortened to this directory.

## `entry`

The entry point for the bundle.

If you pass a string: The string is resolve to a module which is loaded upon startup.

If you pass an array: All modules are loaded upon startup. The last one is exported.

``` javascript
entry: ["./entry1", "./entry2"]
```

If you pass an object: Multiple entry bundles are created. The key is the chunk name. The value can be a string or an array.

``` javascript
entry: {
  page1: "./page1",
  page2: ["./entry1", "./entry2"]
},
output: {
  filename: "[name].bundle.js",
  chunkFilename: "[id].bundle.js"
}
```

## `module`

Options affecting the normal modules (`NormalModuleFactory`)

### `module.loaders`

A array of automatically applied loaders.

Each item can have this properties:

* `test`: A condition that must be met
* `exclude`: A condition that must not be met
* `include`: A condition that must be met
* `loader`: A string of "!" separated loaders
* `loaders`: A array of loaders as string

A condition can be a RegExp, or a array of RegExps combound with "and".

If the request starts with `!`, automatic loaders are not applied.

### `module.preLoaders`, `module.postLoaders`

Syntax like `module.loaders`.

A array of applied pre and post loaders.

If the request starts with `!!`, pre and post loaders are not applied.

## `output`

Options affecting the output.

### `output.path`

The output directory as absolute path.

`[hash]` is replace by the hash of the compilation.

### `output.filename`

The filename of the entry chunk as relative path inside the `output.path` directory.

`[name]` is replaced by the name of the chunk.

`[hash]` is replace by the hash of the compilation.

### `output.chunkFilename`

The filename of non-entry chunks as relative path inside the `output.path` directory.

`[id]` is replaced by the id of the chunk.

`[hash]` is replace by the hash of the compilation.

### `output.namedChunkFilename`

The filename of named chunks as relative path inside the `output.path` directory.

`[name]` is replaced by the name of the chunk.

`[hash]` is replace by the hash of the compilation.

### `output.publicPath`

The `output.path` from the view of the javascript.

``` javascript
// Example
output: {
  path: "/home/proj/public/assets",
  publicPath: "/assets"
}
// Example CDN
output: {
  path: "/home/proj/cdn/assets/[hash]",
  publicPath: "http://cdn.example.com/assets/[hash]/"
}
```

### `output.jsonpFunction`

The JSONP function used by webpack for asnyc loading of chunks

### `output.pathInfo`

Include comments with information about the modules.

### `output.library`

If set, export the bundle as library. `output.library` is the name.

### `output.libraryTarget`

Kind of exporting as library.

`var` - Export by setting a variable: `var Library = xxx`

`this` - Export by setting a property of `this`: `this["Library"] = xxx`

`commonjs` - Export by setting a property of `exports`: `exports["Library"] = xxx`

`commonjs2` - Export by setting `module.exports`: `module.exports = xxx`

## `bail`

Report the first error als hard error instead of tolerating it.

## `console`

Include `console` polyfill.

## `cache`

Cache generated modules to improve performance for multiple builds.

## `watch`

Enter watch mode, which rebuilds on file change.

## `watchDelay`

Delay the rebuilt of this time after the first change.

## `debug`

Switch loaders to debug mode.

## `devtool`

Choose a developer tool to enhance debugging.

`eval` - Each module is executed with `eval` and `//@ sourceURL`.

## `amd`

Set the value of `require.amd` and `define.amd`.

Example: `amd: { jQuery: true }`

## `resolve`

Options affecting the resolving of modules.

### `resolve.alias`

Replace modules by other modules or paths.

### `resolve.modulesDirectories`

Look of module in this directories.

### `resolve.extensions`

Resolve to files by adding this extensions.

## `resolveLoader`

Like `resolve` but for loaders.

## `optimize`

Options affecting the optimization of the compilation.

### `optimize.maxChunks`

Limit the chunk count to a defined value. Chunks are merged until it fits.

### `optimize.minimize`

Mimimize all javascript output of chunks. Loaders are switched into minimizing mode.

### `plugins`

Add additional plugins to the compiler.