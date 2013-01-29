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
  progressCallback: function(percentage, msg) { },
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