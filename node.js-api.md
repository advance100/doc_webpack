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

## stats

The `Stats` object expose this methods:

### `stats.hasErrors`

Returns `true` if there were errors while compiling.

### `stats.hasWarnings`

Returns `true` if there were errors while compiling.

### `stats.toJson(options)`

Return information as json object

You can specify the whished information by the `options` argument: (Boolean)

`options.hash` add the hash of the compilation

`options.timings` add timing information

`options.assets` add assets information

`options.chunks` add chunk information

`options.chunkModules` add built modules information to chunk information

`options.modules` add built modules information

`options.cached` add also information about cached (not built) modules

`options.reasons` add information about the reasons why modules are included

`options.source` add the source code of modules

`options.modulesSort` (string) sort the modules by that field

`options.chunksSort` (string) sort the chunks by that field

`options.assetsSort` (string) sort the assets by that field

### `stats.toString(options)`

Returns a formated string of the result.

`options` are the same as `options` in `toJson`.

`options.colors` With console colors