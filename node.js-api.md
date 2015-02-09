## The short way

``` javascript
var webpack = require("webpack");

// returns a Compiler instance
webpack({
	// configuration
}, function(err, stats) {
	// ...
});
```

## The long way

``` javascript
var webpack = require("webpack");

// returns a Compiler instance
var compiler = webpack({
	// configuration
});

compiler.run(function(err, stats) {
	// ...
});
// or
compiler.watch(/* watchDelay= */200, function(err, stats) {
	// ...
});
```



## `Compiler`

An instance of `Compiler` has the following methods

`compiler.run(callback)` - Builds the bundle(s).
* callback(err, stats) - A function that will be called with the build is complete.

`var watcher = compiler.watch(watchDelay, handler)` - Builds the bundle(s) then starts the watcher, which rebuilds bundles whenever their source files change. Returns a `Watching` instance. Note: since this will automatically run an initial build, so you only need to run `watch` (and not `run`).
* `watchDelay` - The delay (in milliseconds) after a change before the handler is called. Default: 300.
* `handler(err, stats)` - A function that will be called when a build has been completed, or an error or warning has occurred.

## `Watching`

An instance of `Watching` has the following method:

`watcher.close(callback)` - stops the watcher.
* `callback` - A function that's called when the watcher has closed.



## stats

The `Stats` object expose this methods:

### `stats.hasErrors`

Returns `true` if there were errors while compiling.

### `stats.hasWarnings`

Returns `true` if there were errors while compiling.

### `stats.toJson(options)`

Return information as json object

You can specify the information by the `options` argument: (Boolean)

`options.hash` add the hash of the compilation

`options.version` add webpack version information 

`options.timings` add timing information

`options.assets` add assets information

`options.chunks` add chunk information

`options.chunkModules` add built modules information to chunk information

`options.modules` add built modules information

`options.cached` add also information about cached (not built) modules

`options.reasons` add information about the reasons why modules are included

`options.source` add the source code of modules

`options.errorDetails` add details to errors (like resolving log)

`options.chunkOrigins` add the origins of chunks and chunk merging info

`options.modulesSort` (string) sort the modules by that field

`options.chunksSort` (string) sort the chunks by that field

`options.assetsSort` (string) sort the assets by that field

In `toJson` every flag defaults to `true` (except `chunkModules`). By default it's not sorted.

Here is an [example of the resulting JSON](https://github.com/webpack/analyse/blob/master/app/pages/upload/example.json).

> Note: If you want to extract the asset name for generating the HTML page, use the `assetsByChunkName` property, which contains an object mapping `chunkName` to asset name(s) (it's a string or an array of strings).

### `stats.toString(options)`

Returns a formatted string of the result.

`options` are the same as `options` in `toJson`.

`options.colors` With console colors

## error handling

to handle all errors and warnings with the node.js API you need to test `err`, `stats.errors` and `stats.warnings`:

``` javascript
var webpack = require("webpack");
webpack({
	// configuration
}, function(err, stats) {
	if(err)
		return handleFatalError(err);
	var jsonStats = stats.toJson();
	if(jsonStats.errors.length > 0)
		return handleSoftErrors(jsonStats.errors);
	if(jsonStats.warnings.length > 0)
		handleWarnings(jsonStats.warnings);
	successfullyCompiled();
});
```



