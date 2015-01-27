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

## Use with [gulp-webpack-build](https://github.com/mdreizin/gulp-webpack-build)

`gulp-webpack-build` also is a plugin that streams assets from webpack allowing a more idiomatic gulp way of building with webpack, but it uses `webpack.config.js` as source for stream instead of referencing entries.

It has some benefits:

- overrides any properties of `webpack.config.js` file via `webpack.compile` or `webpack.watch`;
- control webpack stats output via `webpack.format`;
- fail your build if `webpack` returns some errors or warnings via `webpack.failAfter`;
- work in watching mode more productive via `webpack.watch` and have all benefits above.

First install with `npm install gulp-webpack-build` and then use as follows:

``` javascript
'use strict';

var path = require('path'),
    gulp = require('gulp'),
    webpack = require('gulp-webpack-build');

var src = './src/**',
    dest = './dist',
    webpackOptions = {
        debug: true,
        devtool: '#source-map'
    };

gulp.task('webpack', [], function() {
    return gulp.src(src)
        .pipe(webpack.compile(webpackOptions))
        .pipe(webpack.format({
            version: false,
            timings: true
        }))
        .pipe(webpack.failAfter({
            errors: true,
            warnings: true
        }))
        .pipe(gulp.dest(dest));
});

gulp.task('watch', function() {
    gulp.watch(src).on('change', function(event) {
        if (event.type === 'changed') {
            webpack.watch(event.path, webpackOptions, { base: path.resolve(event.path) }, function(err, stats) {
                gulp.src(event.path)
                    .pipe(webpack.proxy(err, stats))
                    .pipe(webpack.format({
                        verbose: true,
                        version: false
                    }))
                    .pipe(gulp.dest(dest));
            });
        }
    });
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