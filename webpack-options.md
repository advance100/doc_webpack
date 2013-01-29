# webpack options

``` javascript
module.exports = {
  context: "/home/proj",
  entry: "./entry",
  output: {
    path: "/home/proj/assets",
    filename: "[hash].bunde.js",
    chunkFilename: "[id].[hash].bundle.js",
    namedChunkFilename: "[name].[hash].js",
    publicPath: "/assets/",
    jsonpFunction: "webpackJsonp",
    library: "Lib",
    libraryTarget: "commonjs"
  },
  console: true,
  cache: true,
  watch: true,
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