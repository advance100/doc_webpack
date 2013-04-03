# webpack options

``` javascript
module.exports = {
  context: "/home/proj",
  entry: "./entry",
  module: {
    loaders: [{
      test: /\.coffee$/,
      include: /lib/, exclude: /test/,
      loader: "coffee"
    }],
    preLoaders: [/*...*/],
    postLoaders: [/*...*/]
  },
  output: {
    path: "/home/proj/assets",
    filename: "[hash].bundle.js",
    chunkFilename: "[id].[hash].bundle.js",
    namedChunkFilename: "[name].[hash].js",
    sourceMapFilename: "[file].map", // 0.10
    publicPath: "/assets/",
    jsonpFunction: "webpackJsonp",
    pathInfo: true,
    library: "Lib",
    libraryTarget: "commonjs"
  },
  target: "web",
  bail: true,
  cache: true,
  watch: true,
  watchDelay: 200,
  debug: true,
  devtool: "eval",
  amd: { jQuery: true },
  node: {
    process: "mock",
    http: "mock",
    console: true,
    __filename: "mock",
    __dirname: "mock"
  },
  resolve: {
    alias: {
      module: "other-module",
      module2: "/home/proj/shim-module.js"
    },
    root: "/home/proj/app",
    modulesDirectories: ["module", "node_modules"],
    fallback: "/home/proj/fallback",
    extensions: ["", ".client.js", ".js"]
  },
  resolveLoader: {/*...*/},
  provide: {
    $: "jquery",
    jQuery: "jquery"
  },
  optimize: {
    minChunkSize: 20000,
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

`[hash]` is replaced by the hash of the compilation.

### `output.filename`

The filename of the entry chunk as relative path inside the `output.path` directory.

`[name]` is replaced by the name of the chunk.

`[hash]` is replaced by the hash of the compilation.

### `output.chunkFilename`

The filename of non-entry chunks as relative path inside the `output.path` directory.

`[id]` is replaced by the id of the chunk.

`[hash]` is replaced by the hash of the compilation.

### `output.namedChunkFilename`

The filename of named chunks as relative path inside the `output.path` directory.

`[name]` is replaced by the name of the chunk.

`[hash]` is replaced by the hash of the compilation.

### `output.sourceMapFilename` (0.10)

The filename of the SourceMaps for the Javascript files. They are inside the `output.path` directory.

`[file]` is replaced by the filename of the Javascript file.

`[id]` is replaced by the id of the chunk.

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

`umd` - Export to AMD, CommonJS2 or as property in root

## `target`

* `"web"` Compile for usage in a browser-like environment (default)
* `"webworker"` Compile as WebWorker
* `"node"` Compile for usage in a node.js-like environment

## `bail`

Report the first error als hard error instead of tolerating it.

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

`source-map` - A SourceMap is emitted. See also `output.sourceMapFilename`. (0.10)

`inline-source-map` - A SourceMap is added as DataUrl to the Javascript file. (0.10)

## `node`

Include polyfills or mocks for various node stuff:

* `console`: `true` or `false`
* `process`: `true`, `"mock"` or `false`
* `__filename`: `true` (real filename), `"mock"` (`"/index.js"`) or `false`
* `__dirname`: `true` (real dirname), `"mock"` (`"/"`) or `false`
* `<node buildin>`: `true`, `"mock"` or `false`

## `amd`

Set the value of `require.amd` and `define.amd`.

Example: `amd: { jQuery: true }`

## `resolve`

Options affecting the resolving of modules.

### `resolve.alias`

Replace modules by other modules or paths.

### `resolve.root`

Look of modules in this directory (or directories if you pass an array).

### `resolve.modulesDirectories`

Look of modules in this directories. It'll check these directories in current directory and each parent directory.

### `resolve.fallback`

Look of modules in this directory (or directories if you pass an array), if no module found in `resolve.root` and `resolve.modulesDirectories`.

### `resolve.extensions`

Resolve to files by adding this extensions.

## `resolveLoader`

Like `resolve` but for loaders.

### `resolveLoader.moduleTemplates`

That's a `resolveLoader` only property.

It describes alternatives for the module name that are tried.

The default value is: `["*-webpack-loader", "*-web-loader", "*-loader", "*"]`

## `optimize`

Options affecting the optimization of the compilation.

### `optimize.minChunkSize`

Merge small chunks that are lower than this min size (in chars). Size is approximated.

### `optimize.maxChunks`

Limit the chunk count to a defined value. Chunks are merged until it fits.

### `optimize.minimize`

Mimimize all javascript output of chunks. Loaders are switched into minimizing mode.

### `plugins`

Add additional plugins to the compiler.

# Good start config (example)

``` javascript
var path = require("path");
module.exports = {
  context: __dirname,
  entry: "./app/app.js",
  output: {
    path: path.join(__dirname, "public", "assets"),
    publicPath: "/assets/",
    filename: "[hash].js"
  },
  module: {
    loaders: [
      { test: /\.json$/,   loader: "json-loader" },
      { test: /\.coffee$/, loader: "coffee-loader" },
      { test: /\.css$/,    loader: "style-loader!css-loader" },
      { test: /\.less$/,   loader: "style-loader!css-loader!less-loader" },
      { test: /\.jade$/,   loader: "jade-loader" },
      { test: /\.png$/,    loader: "url-loader?limit=10000&minetype=image/png" },
      { test: /\.jpg$/,    loader: "url-loader?limit=10000&minetype=image/jpg" },
      { test: /\.gif$/,    loader: "url-loader?limit=10000&minetype=image/gif" },
      { test: /\.woff$/,   loader: "url-loader?limit=10000&minetype=application/font-woff" }
    ],
    preLoaders: [
      {
        test: [
          /\.js$/,
          /\.json$/,
        ],
        include: pathToRegExp(path.join(__dirname, "app")),
        loader: "jshint-loader"
      }
    ]
  },
  resolve: {
    fallback: path.join(__dirname, "app")
  },
  cache: true,
  amd: { jQuery: true },
  optimize: {
    minChunkSize: 10000,
    maxChunks: 20,
  },
  plugins: [
  ]
};
function escapeRegExpString(str) { return str.replace(/[\-\[\]\/\{\}\(\)\*\\+\?\.\\\\\^\$\|]/g, "\\\\$&"); }
function pathToRegExp(p) { return new RegExp("^" + escapeRegExpString(p)); }
```

``` sh
# development: compile and watch
webpack -d --watch --progress --colors

# development: server
webpack-dev-server -d --colors

# production: compile
webpack -p --progress --colors
```