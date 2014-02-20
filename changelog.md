# 1.0

* API: The following options are now DEPRECATED and superseded by plugins:
 * `define` -> `DefinePluging`
 * `prefetch` -> `PrefetchPlugin`
 * `provide` -> `ProvidePlugin`
 * `hot` -> `HotModuleReplacementPlugin`
 * `optimize.dedupe` -> `optimize.DedupePlugin`
 * `optimize.minimize` -> `optimize.UglifyJsPlugin`
 * `optimize.maxChunks` -> `optimize.LimitChunkCountPlugin`
 * `optimize.minChunkSize` -> `optimize.MinChunkSizePlugin`
 * `optimize.occurenceOrder` -> `optimize.OccurenceOrderPlugin`
* Warnings are emitted when using deprecated options
* API: plugins are now exported by webpack: `require("webpack").DefinePlugin`
* API: Labeled Modules are now disabled by default, use the `dependencies.LabeledModulesPlugin`
* API: Internal plugin arguments simplified
* API: added `ResolverPlugin`
* API: added chunk origin tracking and resolve logging (for finding compile bugs)
* API: added `eval-source-map` devtool
* API: default configuration depends on `target` option
* API: changed filenames in SourceMaps
* API: ids for entry chunks need to longer have the id 0
* API: output as amd module
* API: allow to configure the indent of the source
* API: added AggressiveMergingPlugin and ResolverPlugin
* API: added `--display-origins` to show chunk origins
* API: added `--display-error-details` to show resolving log
* SUPPORT: Support for the [browser-field](https://gist.github.com/defunctzombie/4339901)
* SUPPORT: free vars are tracked over IIFEs
* SUPPORT: allow to rename free vars
* SUPPORT: allow local named amd modules
* PERFORMANCE: Cache final module sources
* SIZE: no `require.e` if not needed
* bug fixes

# 0.11

* API: **`this` in modules is now `exports` (if this breaks a library, try prefixing `imports?this=>window!`)**
* API: added Hot Code Replacement `--hot` (web and node target) [experimental]
* API: added `define` option
* API: support new `sourceMappingURL` and `sourceURL` syntax
* API: added `CommonsChunkPlugin`
* API: `--profile --progress` now display process timings
* API: added `loaderContext.loadModule`
* PERFORMANCE: added `unsafeCache` and `noParse` option for performance
* SIZE: automatically remove require.ensure when no chunk was generated.
* SIZE: generate (sparse) array instead of object as module container when appropriate
* SUPPORT: extract dependencies from a bound callback
* SUPPORT: support evaluating of .replace and .split
* TEST: added many of the browsertest to the node.js tests

# 0.10

* SUPPORT: node 0.10 support
* PERFORMANCE: whole chunks can now be cached
* API: store state in json file (records) `--records-path`
* API: added `--devtool source-map` and `--devtool inline-source-map`
* SIZE: added option `--optimize-occurence-order`
* SIZE: added `--optimize-dedupe`

Small changes:

* PERFORMANCE: assets are only emitted if they changed
* API: added `--profile`
* PERFORMANCE: added `--prefetch` [experimental]
* API: added `BannerPlugin`
* API: added `[chunkhash]` [experimental]
* API: added `hashDigestLength`
* PERFORMANCE: increased filesystem caching to 60s
* PERFORMANCE: purge only changed files in watch mode
* PERFORMANCE: purge all files on compiling in not-watch mode
* SUPPORT: in-memory filesystem now supports windows-like paths too
* SIZE: merging chunk is more clever

# 0.9

...

# 0.8

* Updated to UglifyJs 2
* Query String are allowed for loaders and resources
* Updated many loaders to use query strings as parameters
* "jam" is no longer a default modules folder (still possible to add it per config)
* API: fixed typos: `modulesDirectories`, `separable`, `library`
* API: api of enhanced-resolve and enhanced-require changed.
* API: `options.minimize` and now be also a object, which is passed to UglifyJs2.Compressor
* API: added `"web"` to default package mains, added `"webLoader"` to default loader package mains
* API: removed `"webpack"` from default loader package mains
* added "node" template: bundle can run on node.js host (experimental)


# 0.7.6

* API: added experimental chunk merging via `options.maxChunks`

# 0.7

* API: `loaderContext.depencency` is more relaxed and don't need to be called before reading
* API: `loader.seperable` cannot combined with
 * `loaderContext.emitFile` and `loaderContext.emitSubStats` 
 * `loaderContext.options.events`
 * `loaderContext.options.resolve`
 * `loaderContext.resolve` and `loaderContext.resolve.sync`
* API: added `loader.seperableIfResolve`
* API: `loader.seperableIfResolve` cannot combined with
 * `loaderContext.emitFile` and `loaderContext.emitSubStats` 
 * `loaderContext.options.events`
* API: added `profile` option (and `--profile`)
* API: added `workers` option (and `--workers`)
* API: added `closeWorkers` option
* API: if option `workers` is used:
 * `options` must be `JSON.stringify`-able. Except `options.events`.
 * Any error thrown in loader must be an object (i. e. an Error object). Only `message`, `stack` and value of `toString` is passed to main process.
* API: added `workersNoResolve` option. Workers should not used while resolving.
* API: The expected `Cache` object for `options.cache` has changed.
* API: event `module` is emited *after* the module is finished.
* API: event `context` is now named `context-enum`
* API: added event `context` which is emited *after* the context is finished.
* API: event `dependency` is removed. Use `stats.dependencies` for this.
* API: event `loader` is removed. Use `stats.loaders` for this.
* API: added `stats.contexts` as a list of contexts.
* API: added `stats...modules[..].dependencies` for as list of files which affect the module's content.
* API: added `stats...modules[..].loaders` for as list of loaders which affect the module's content.
* API: removed `stats.modulesPerChunk`, it is useless and was deprecated.
* API: added `stats.chunkNameFiles` which export the files for named chunks
* API: added `stats.startTime`, timestamp as number
* cmd: more colorful output to indicate caching and timing
* API: webpack in watch mode emits the event `watch-end` if watch mode have to end (i. e. loader changed). You may restart it after clearing `require.cache`.
* API: added `loaderContext.loaderType` as one of `loader`, `preLoader` or `postLoader`.
* API: added `loaderContext.currentLoaders` as list of all loader of the current type.
* API: added `loaderContext.loaderIndex` as index of current loader in `loaderContext.currentLoaders`.
* API: added `loaderContext.loaders`, `loaderContext.preLoaders` and `loaderContext.postLoaders`.
* API: added stats.fileModules...reasons[].async = true instead of "async xxx"
* API: added `loaderContext.emitError` and `loaderContext.emitWarning`

# 0.6

* internal: resolving logic moved into [enhanced-resolve](https://github.com/webpack/enhanced-resolve).
* internal: moved loaders logic into [enhanced-require](https://github.com/webpack/enhanced-require).
* API: removed `require-polyfill`, use [enhanced-require](https://github.com/webpack/enhanced-require).
* API: removed deprecated `script-src-prefix`.
* API: removed deprecated direct compile into output, overwrite `emitFile`.
* API: parameter `options` is not longer optional.
* API: added `loader.seperable` (see [spec](https://github.com/webpack/webpack/wiki/Loader-Specification)).
* API: `loaderContext.resolve` is now async, even in sync mode.
* API: added `loaderContext.resolve.sync` as sync version.