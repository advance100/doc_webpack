WIP

# Incremental builds

Make sure you don't do a full rebuild. webpack has a great caching layer that allows to keep already compiled modules in memory. There are some tools that help to use it:

* [[webpack-dev-server]]: Serves all webpack assets from memory. Best performance.
* [[webpack-dev-middleware]]: The same as middleware for advanced users.
* [[webpack --watch | cli]] or [[`watch: true` | node.js API]]: Caches stuff but write assets to disk. Ok performance.

# Prefetching modules

[[`prefetch` | http://webpack.github.io/docs/list-of-plugins.html#prefetchplugin]]

# Exclude modules from parsing

With [[`noParse` | http://webpack.github.io/docs/configuration.html#module-noparse]] you can exclude big libraries from parsing, but this can break stuff.

# Hints from build stats

There is an [analyse tool](http://webpack.github.io/analyse/) which visualise your build and also provides some hint how build size and build performance can be optimized.

# SourceMaps

SourceMaps are slow.

`devtool: "source-map"` cannot cache SourceMaps for modules and need to regenerate complete SourceMap for the chunk. It's something for production...

`devtool: "eval-source-map"` is really as good as `devtool: "source-map"`, but can cache SourceMaps for modules. It's much faster.

The best performance has `devtool: "eval"`, but it only maps to compiled source code per module. In many cases this is good enough. Hint: combine it with `output.pathinfo: true`.

The UglifyJsPlugin uses SourceMaps to map errors to source code. And SourceMaps are slow. As you should only use this is production this is fine. If your production build is really slow you can disable it with `new UglifyJsPlugin({ sourceMap: false })`.