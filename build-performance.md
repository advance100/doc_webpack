WIP

* Incremental builds with [[webpack-dev-server]], [[webpack-dev-middleware]], [[webpack --watch | cli]] or [[`watch: true` | node.js API]]
* [[`prefetch` | http://webpack.github.io/docs/list-of-plugins.html#prefetchplugin]]
* [[`noParse` | http://webpack.github.io/docs/configuration.html#module-noparse]]
* [analyse tool gives hints](http://webpack.github.io/analyse/)
* SourceMaps are slow
  * `devtool: "eval-source-map"`
  * `new UglifyJsPlugin({ sourceMap: false })`