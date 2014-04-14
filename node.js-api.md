## The short way

``` javascript
var webpack = require("webpack");
webpack({
	// configuration
}, function(err, stats) {
	// ...
});
```

## The long way

``` javascript
var webpack = require("webpack");

var compiler = webpack({
	// configuration
});

compiler.run(function(err, stats) {
	// ...
});

compiler.watch(/* watchDelay= */200, function(err, stats) {
	// ...
});
```

> Note: `watch` will automatically run an initial build, so you should either call `run` or `watch`.

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
