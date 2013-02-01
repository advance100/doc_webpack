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
    maxChunks: 5
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