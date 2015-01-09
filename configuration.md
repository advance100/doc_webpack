> webpack is fed a configuration object. Depending on your usage of webpack there are two ways to pass this configuration object:

### CLI

If you use the [[CLI]] it will read a file `webpack.config.js` (or the file passed by the `--config` option). This file should export the configuration object:

``` javascript
module.exports = {
	// configuration
};
```

### node.js API

If you use the [[node.js API]] you need to pass the configuration object as parameter:

``` javascript
webpack({
	// configuration
}, callback);
```





# configuration object content

> Hint: Keep in mind that you don't need to write pure JSON into the configuration. Use any JavaScript you want. It's just a node.js module...

Very simple configuration object example:

``` javascript
{
	context: __dirname + "/app",
	entry: "./entry",
	output: {
		path: __dirname + "/dist",
		filename: "bundle.js"
	}
}
```



## `context`

The base directory (absolute path!) for resolving the `entry` option. If `output.pathinfo` is set, the included pathinfo is shortened to this directory.

> Default: `process.cwd()`



## `entry`

The entry point for the bundle.

If you pass a string: The string is resolved to a module which is loaded upon startup.

If you pass an array: All modules are loaded upon startup. The last one is exported.

``` javascript
entry: ["./entry1", "./entry2"]
```

If you pass an object: Multiple entry bundles are created. The key is the chunk name. The value can be a string or an array.

``` javascript
{
	entry: {
		page1: "./page1",
		page2: ["./entry1", "./entry2"]
	},
	output: {
		// Make sure to use [name] or [id] in output.filename
		//  when using multiple entry points
		filename: "[name].bundle.js",
		chunkFilename: "[id].bundle.js"
	}
}
```



## `output`

Options affecting the output.

If you use any hashing (`[hash]` or `[chunkhash]`) make sure to have a consistent ordering of modules. Use the `OccurenceOrderPlugin` or `recordsPath`.

### `output.path`

The output directory as **absolute path** (required).

`[hash]` is replaced by the hash of the compilation.

### `output.filename`

The filename of the entry chunk as relative path inside the `output.path` directory.

`[name]` is replaced by the name of the chunk.

`[hash]` is replaced by the hash of the compilation.

> You **must** not specify an absolute path here! Use the `output.path` option.

### `output.chunkFilename`

The filename of non-entry chunks as relative path inside the `output.path` directory.

`[id]` is replaced by the id of the chunk.

`[name]` is replaced by the name of the chunk (or with the id when the chunk has no name).

`[hash]` is replaced by the hash of the compilation.

`[chunkhash]` is replaced by the hash of the chunk.

### `output.sourceMapFilename`

The filename of the SourceMaps for the JavaScript files. They are inside the `output.path` directory.

`[file]` is replaced by the filename of the JavaScript file.

`[id]` is replaced by the id of the chunk.

`[hash]` is replaced by the hash of the compilation.

> Default: `"[file].map"`

### `output.devtoolModuleFilenameTemplate`

Filename template for the `sources` array in a generated SourceMap.

`[resource]` is replaced by the path used by Webpack to resolve the file, including the query params to the rightmost loader (if any).

`[resource-path]` is the same as `[resource]` but without the loader query params.

`[loaders]` is the list of loaders and params up to the name of the rightmost loader (only explict loaders).

`[all-loaders]` is the list of loaders and params up to the name of the rightmost loader (including automatic loaders).

`[id]` is replaced by the id of the module.

`[hash]` is replaced by the hash of the module identifier.

`[absolute-resource-path]` is replaced with the absolute filename.

> Default (devtool=`[inline-]source-map`): `"webpack:///[resource-path]"`  
> Default (devtool=`eval`): `"webpack:///[resource-path]?[loaders]"`  
> Default (devtool=`eval-source-map`): `"webpack:///[resource-path]?[hash]"`

### `output.devtoolFallbackModuleFilenameTemplate`

Similar to `output.devtoolModuleFilenameTemplate`, but used in the case of duplicate module identifiers.

> Default: `"webpack:///[resourcePath]?[hash]"`

### `output.hotUpdateChunkFilename`

The filename of the Hot Update Chunks. They are inside the `output.path` directory.

`[id]` is replaced by the id of the chunk.

`[hash]` is replaced by the hash of the compilation. (The last hash stored in the records)

> Default: `"[id].[hash].hot-update.js"`

### `output.hotUpdateMainFilename`

The filename of the Hot Update Main File. It is inside the `output.path` directory.

`[hash]` is replaced by the hash of the compilation. (The last hash stored in the records)

> Default: `"[hash].hot-update.json"`

### `output.publicPath`

The `output.path` from the view of the JavaScript.

``` javascript
// Example
output: {
	path: "/home/proj/public/assets",
	publicPath: "/assets/"
}
// Example CDN
output: {
	path: "/home/proj/cdn/assets/[hash]",
	publicPath: "http://cdn.example.com/assets/[hash]/"
}
```

> If you don't know the publicPath while compiling you can omit it and set `__webpack_public_path__` on runtime.
> 
> Example:
> 
> ``` javascript
> var scripts = document.getElementsByTagName("script");
> var src = scripts[scripts.length - 1].getAttribute("src");
> __webpack_public_path__ = src.substr(0, src.lastIndexOf("/") + 1);
> ```

### `output.jsonpFunction`

The JSONP function used by webpack for asnyc loading of chunks.

A shorter function may reduce the filesize a bit. Use different identifier, when having multiple webpack instances on a single page.

> Default: `"webpackJsonp"`

### `output.hotUpdateFunction`

The JSONP function used by webpack for async loading of hot update chunks.

> Default: `"webpackHotUpdate"`

### `output.pathinfo`

Include comments with information about the modules.

`require(/* ./test */23)`

Do not use this in production.

> Default: `false`

### `output.library`

If set, export the bundle as library. `output.library` is the name.

Use this, if you are writing a library and want to publish it as single file.

### `output.libraryTarget`

Which format to export the library:

`"var"` - Export by setting a variable: `var Library = xxx` (default)

`"this"` - Export by setting a property of `this`: `this["Library"] = xxx`

`"commonjs"` - Export by setting a property of `exports`: `exports["Library"] = xxx`

`"commonjs2"` - Export by setting `module.exports`: `module.exports = xxx`

`"amd"` - Export to AMD (optionally named)

`"umd"` - Export to AMD, CommonJS2 or as property in root

> Default: `"var"`

If `output.library` is not set, but `output.libraryTarget` is set to a value other than `var`, every property of the exported object is copied (Except `amd`, `commonjs2` and `umd`).

### `output.sourcePrefix`

Prefixes every line of the source in the bundle with this string.

Default: `"\t"`



## `module`

Options affecting the normal modules (`NormalModuleFactory`)

### `module.loaders`

A array of automatically applied loaders.

Each item can have these properties:

* `test`: A condition that must be met
* `exclude`: A condition that must not be met
* `include`: A condition that must be met
* `loader`: A string of "!" separated loaders
* `loaders`: A array of loaders as string

A condition may be a RegExp, a string containing the absolute path, or an array of one of these combined with "and".

See more: [[loaders]]

*IMPORTANT*: The loaders here are resolved *relative to the resource* which they are applied to. The means they are not resolved relative the the configuration file. If you have loaders installed from npm and your `node_modules` folder is not in a parent folder of all source files, webpack cannot find the loader. You need to add the `node_modules` folder as absolute path to the `resolveLoader.root` option. (`resolveLoader: { root: path.join(__dirname, "node_modules") }`)

### `module.preLoaders`, `module.postLoaders`

Syntax like `module.loaders`.

A array of applied pre and post loaders.

### `module.noParse`

A RegExp or an array of RegExps. Don't parse files matching.

This can boost performance when ignoring big libraries.

The files are expected to have no call to `require`, `define` or similar. They are allowed to use `exports` and `module.exports`.

### automatically created contexts defaults `module.xxxContextXxx`

There are multiple options to configure the defaults for an automatically created context. We differentiate three types of automatically created contexts:

* `exprContext`: An expression as dependency (i. e. `require(expr)`)
* `wrappedContext`: An expression plus pre- and/or suffixed string (i. e. `require("./templates/" + expr)`)
* `unknownContext`: Any other unparsable usage of `require` (i. e. `require`)

Four options are possible for automatically created contexts:

* `request`: The request for context.
* `recursive`: Subdirectories should be traversed.
* `regExp`: The RegExp for the expression.
* `critical`: This type of dependency should be consider as critical (emits a warning).

All options and defaults:

`unknownContextRequest = "."`, `unknownContextRecursive = true`, `unknownContextRegExp = /^\.\/.*$/`, `unknownContextCritical = true`

`exprContextRequest = "."`, `exprContextRegExp = /^\.\/.*$/`, `exprContextRecursive = true`, `exprContextCritical = true`

`wrappedContextRegExp = /.*/`, `wrappedContextRecursive = true`, `wrappedContextCritical = false`

> Note: `module.wrappedContextRegExp` only refers to the middle part of the full RegExp. The remaining is generated from prefix and surfix.

Example:

``` javascript
{
  module: {
	// Disable handling of unknown requires
	unknownContextRegExp: /$^/,
	unknownContextCritical: false,

	// Disable handling of requires with a single expression
	exprContextRegExp: /$^/,
	exprContextCritical: false,

	// Warn for every expression in require
	wrappedContextCritical: true
  }
}
```



## `resolve`

Options affecting the resolving of modules.

### `resolve.alias`

Replace modules by other modules or paths.

Expected is a object with keys being module names. The value is the new path. It's similar to a replace but a bit more clever. If the the key ends with `$` only the exact match (without the `$`) will be replaced.

If the value is a relative path it will be relative to the file containing the require.

Examples: Calling a require from `/abc/entry.js` with different alias settings.

| `alias:` | `require("xyz")` | `require("xyz/file.js")` |
|---|---|---|
| `{}` | `/abc/node_modules/xyz/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "/absolute/path/to/file.js" }` | `/absolute/path/to/file.js` | error |
| `{ xyz$: "/absolute/path/to/file.js" }` | `/absolute/path/to/file.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "./dir/file.js" }` | `/abc/dir/file.js` | error |
| `{ xyz$: "./dir/file.js" }` | `/abc/dir/file.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "/some/dir" }` | `/some/dir/index.js` | `/some/dir/file.js` |
| `{ xyz$: "/some/dir" }` | `/some/dir/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "./dir" }` | `/abc/dir/index.js` | `/abc/dir/file.js` |
| `{ xyz: "modu" }` | `/abc/node_modules/modu/index.js` | `/abc/node_modules/modu/file.js` |
| `{ xyz$: "modu" }` | `/abc/node_modules/modu/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "modu/some/file.js" }` | `/abc/node_modules/modu/some/file.js` | error |
| `{ xyz: "modu/dir" }` | `/abc/node_modules/modu/dir/index.js` | `/abc/node_modules/dir/file.js` |
| `{ xyz: "xyz/dir" }` | `/abc/node_modules/xyz/dir/index.js` | `/abc/node_modules/xyz/dir/file.js` |
| `{ xyz$: "xyz/dir" }` | `/abc/node_modules/xyz/dir/index.js` | `/abc/node_modules/xyz/file.js` |

`index.js` may resolve to another file if defined in the `package.json`.

`/abc/node_modules` may resolve in `/node_modules` too.

### `resolve.root`

The directory (**absolute path**) that contains your modules. May also be an array of directories. This setting should be used to add individual directories to the search path.

> It **must** be an **absolute path**! Don't pass something like `./app/modules`.

### `resolve.modulesDirectories`

An array of directory names to be resolved to the current directory as well as its ancestors, and searched for modules. This functions similarly to how node finds "node_modules" directories. For example, if the value is `["mydir"]`, webpack will look in "./mydir", "../mydir", "../../mydir", etc.

> Default: `["web_modules", "node_modules"]`

> Note: Passing `"../someDir"`, `"app"`, `"."` or an absolute path isn't necessary here. Just use a directory name, not a path. Use only if you expect to have a hierarchy within these folders. Otherwise you may want to use the `resolve.root` option instead.

### `resolve.fallback`

A directory (or array of directories **absolute paths**), in which webpack should look for modules that weren't found in `resolve.root` or `resolve.modulesDirectories`.

### `resolve.extensions`

An array of extensions that should be used to resolve modules. For example, in order to discover CoffeeScript files, your array should contain the string `".coffee"`.

> Default: `["", ".webpack.js", ".web.js", ".js"]`

**IMPORTANT**: Setting this option will override the default, meaning that webpack will no longer try to resolve modules using the default extensions. If you want modules that were required with their extension (e.g. `require('./somefile.ext')`) to be properly resolved, you **must** include an empty string in your array. Similarly, if you want modules that were required without extensions (e.g. `require('underscore')`) to be resolved to files with ".js" extensions, you **must** include `".js"` in your array.

### `resolve.packageMains`

Check these fields in the `package.json` for suitable files.

> Default: `["webpack", "browser", "web", "browserify", ["jam", "main"], "main"]`

### `resolve.packageAlias`

Check this field in the `package.json` for an object. Key-value-pairs are threaded as aliasing according to [this spec](https://gist.github.com/defunctzombie/4339901)

> Not set by default

Example: `"browser"` to check the browser field.

### `resolve.unsafeCache`

Enable aggressive but unsafe caching for the resolving of a part of your files. Changes to cached paths may cause failure (in rare cases). An array of RegExps, only a RegExp or `true` (all files) is expected. If the resolved path matches, it'll be cached.

> Default: `[]`



## `resolveLoader`

Like `resolve` but for loaders.

``` javascript
// Default:
{
	modulesDirectories: ["web_loaders", "web_modules", "node_loaders", "node_modules"],
	extensions: ["", ".webpack-loader.js", ".web-loader.js", ".loader.js", ".js"],
	packageMains: ["webpackLoader", "webLoader", "loader", "main"]
}
```

### `resolveLoader.moduleTemplates`

That's a `resolveLoader` only property.

It describes alternatives for the module name that are tried.

> Default: `["*-webpack-loader", "*-web-loader", "*-loader", "*"]`



## `externals`

Specify dependencies that shouldn't be resolved by webpack, but should become dependencies of the resulting bundle. The kind of the dependency depends on `output.libraryTarget`.

As value an object, a string, a function, a RegExp and an array is accepted.

* string: An exact matched dependency becomes external. The same string is used as external dependency.
* object: If an dependency matches exactly a property of the object, the property value is used as dependency. The property value may contain a dependency type prefixed and separated with a space. If the property value is `true` the property name is used instead. If the property value is `false` the externals test is aborted and the dependency is not external. See example below.
* function: `function(context, request, callback(err, result))` The function is called on each dependency. If a result is passed to the callback function this value is handled like a property value of an object (above bullet point).
* RegExp: Every matched dependency becomes external. The request is also used as request for the external dependency.
* array: Multiple values of the scheme (recursive).

Example:

``` javascript
{
	output: { libraryTarget: "commonjs" },
	externals: [
		{
			a: false, // a is not external
			b: true, // b is external (require("b"))
			"./c": "c", // "./c" is external (require("c"))
			"./d": "var d" // "./d" is external (d)
		},
		// Every non-relative module is external
		// abc -> require("abc")
		/^[a-z\-0-9]+$/,
		function(context, request, callback) {
			// Every module prefixed with "global-" becomes external
			// "global-abc" -> abc
			if(/^global-/.test(request))
				return callback(null, "var " + request.substr(7));
			callback();
		},
		"./e" // "./e" is external (require("./e"))
	]
}
```

| type        | value               | resulting import code |
|-------------|---------------------|-----------------------|
| "var"       | `"abc"`             | `module.exports = abc;` |
| "var"       | `"abc.def"`         | `module.exports = abc.def;` |
| "this"      | `"abc"`             | `(function() { module.exports = this["abc"]; }());` |
| "this"      | `["abc", "def"]`    | `(function() { module.exports = this["abc"]["def"]; }());` |
| "commonjs"  | `"abc"`             | `module.exports = require("abc");` |
| "commonjs"  | `["abc", "def"]`    | `module.exports = require("abc").def;` |
| "amd"       | `"abc"`             | `define(["abc"], function(X) { module.exports = X; })` |
| "umd"       | `"abc"`             | everything above |

Enforcing `amd` or `umd` in a external value will break if not compiling as amd/umd target.

> Note: If using `umd` you can specify an object as external value with property `commonjs`, `commonjs2`, `amd` and `root` to set different values for each import kind.




## `target`

* `"web"` Compile for usage in a browser-like environment (default)
* `"webworker"` Compile as WebWorker
* `"node"` Compile for usage in a node.js-like environment (use `require` to load chunks)
* `"async-node"` Compile for usage in a node.js-like environment (use `fs` and `vm` to load chunks async)
* `"node-webkit"` Compile for usage in webkit, uses jsonp chunk loading but also supports native node.js modules plus require("nw.gui") (experimental)



## `bail`

Report the first error as a hard error instead of tolerating it.



## `profile`

Capture timing information for each module.

> Hint: Use the [analyze tool](http://webpack.github.io/analyse) to visualize it. `--json` or `stats.toJson()` will give you the stats as JSON.



## `cache`

Cache generated modules and chunks to improve performance for multiple incremental builds.

This is enabled by default in watch mode.

You can pass `false` to disable it.

You can pass an object to enable it and let webpack use the passed object as cache. This way you can share the cache object between multiple compiler calls. Note: Don't share the cache between calls with different options.



## `watch`

Enter watch mode, which rebuilds on file change.

Only use it with the Node.js JavaScript api `webpack(options, fn)`.



## `watchDelay`

Delay the rebuilt after the first change. Value is a time in ms.

> Default: 200



## `debug`

Switch loaders to debug mode.



## `devtool`

Choose a developer tool to enhance debugging.

`eval` - Each module is executed with `eval` and `//@ sourceURL`.

`source-map` - A SourceMap is emitted. See also `output.sourceMapFilename`.

`hidden-source-map` - Same as `source-map`, but doesn't add a reference comment to the bundle.

`inline-source-map` - A SourceMap is added as DataUrl to the JavaScript file.

`eval-source-map` - Each module is executed with `eval` and a SourceMap is added as DataUrl to the `eval`.

Prefixing `@`, `#` or `#@` will enforce a pragma style.

> Hint: If your modules already contain SourceMaps you'll need to use the [source-map-loader](https://github.com/webpack/source-map-loader) to merge it with the emitted SourceMap.

> `eval` fast, fast rebuilds, development only, see generated code per module.

> `source-map` slow, slow rebuilds, also production possible, see original code per module, additional file.

> `inline-source-map` slow, slow rebuilds, development only, see original code per module.

> `eval-source-map` slow, fast rebuilds, development only, see original code per module. 

Example:

``` javascript
{
	devtool: "#inline-source-map"
}
// =>
//# sourceMappingURL=...
```

## `devServer`

Can be used to configure the behaviour of [webpack-dev-server](https://github.com/webpack/webpack-dev-server).

Example:

``` javascript
{
	devServer: {
		contentBase: "./build",
	}
}
```

## `node`

Include polyfills or mocks for various node stuff:

* `console`: `true` or `false`
* `global`: `true` or `false`
* `process`: `true`, `"mock"` or `false`
* `buffer`: `true`, `"mock"` or `false`
* `__filename`: `true` (real filename), `"mock"` (`"/index.js"`) or `false`
* `__dirname`: `true` (real dirname), `"mock"` (`"/"`) or `false`
* `<node buildin>`: `true`, `"mock"`, `"empty"` or `false`


``` javascript
// Default:
{
	console: false,
	process: true,
	global: true,
	buffer: true,
	__filename: "mock",
	__dirname: "mock"
}
```


## `amd`

Set the value of `require.amd` and `define.amd`.

Example: `amd: { jQuery: true }` (for old 1.x AMD versions of jquery)



## `loader`

Custom values available in the loader context.



## `recordsPath`, `recordsInputPath`, `recordsOutputPath`

Store/Load compiler state from/to a json file. This will result in persistent ids of modules and chunks.

An **absolute path** is excepted. `recordsPath` is used for `recordsInputPath` and `recordsOutputPath` if they left undefined.

This is required, when using Hot Code Replacement between multiple calls to the compiler.



## `plugins`

Add additional plugins to the compiler.