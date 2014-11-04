Using webpack with gulp is as easy as using the [[node.js API]].

``` javascript
var gulp = require("gulp");
var gutil = require("gulp-util");
var webpack = require("webpack");
var WebpackDevServer = require("webpack-dev-server");
```

## Normal compilation

``` javascript
gulp.task("webpack", function(callback) {
	// run webpack
	webpack({
		// configuration
	}, function(err, stats) {
		if(err) throw new gutil.PluginError("webpack", err);
		gutil.log("[webpack]", stats.toString({
			// output options
		}));
		callback();
	});
});
```

## Use with [gulp-webpack](https://github.com/shama/gulp-webpack)

gulp-webpack is a plugin that streams assets from webpack allowing a more idiomatic gulp way of building with webpack. First install with `npm install gulp-webpack` and then use as follows:

``` javascript
var gulp = require('gulp');
var webpack = require('gulp-webpack');
gulp.task("webpack", function() {
	return gulp.src('src/entry.js')
		.pipe(webpack({ /* webpack configuration */ }))
		.pipe(gulp.dest('dist/'));
});
```

## [[webpack-dev-server]]

> Don't be too lazy to integrate the webpack-dev-server into your development process. It's an important tool for productivity.

``` javascript
gulp.task("webpack-dev-server", function(callback) {
	// Start a webpack-dev-server
	var compiler = webpack({
		// configuration
	});

	new WebpackDevServer(compiler, {
		// server and middleware options
	}).listen(8080, "localhost", function(err) {
		if(err) throw new gutil.PluginError("webpack-dev-server", err);
		// Server listening
		gutil.log("[webpack-dev-server]", "http://localhost:8080/webpack-dev-server/index.html");

		// keep the server alive or continue?
		// callback();
	});
});
```

## Example

Take a look at a example gulpfile. It covers three modes:

* webpack-dev-server
* build - watch cycle
* production build

[Example gulpfile](https://github.com/webpack/webpack-with-common-libs/blob/master/gulpfile.js)