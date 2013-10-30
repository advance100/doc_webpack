# changelog

# 0.11

* **`this` in modules is now `exports` (if this breaks a library, try prefixing `imports?this=>window!`)**
* added Hot Code Replacement `--hot` [experimental]
* added `define` option
* support new `sourceMappingURL` and `sourceURL` syntax
* added `unsafeCache` and `noParse` option for performance
* `--profile --progress` now display process timings
* added `loaderContext.loadModule`
* automatically remove require.ensure when no chunk was generated.

# 0.10

* node 0.10 support
* whole chunks can now be cached
* store state in json file (records) `--records-path`
* added `--devtool source-map` and `--devtool inline-source-map`
* added option `--optimize-occurence-order`
* added `--optimize-dedupe`

Small changes:

* assets are only emitted if they changed
* added `--profile`
* added `--prefetch` [experimental]
* added `BannerPlugin`
* added `[chunkhash]` [experimental]
* added `hashDigestLength`
* increased filesystem caching to 60s
* purge only changed files in watch mode
* purge all files on compiling in not-watch mode
* in-memory filesystem now supports windows-like paths too
* merging chunk is more clever

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