A (webpack) plugin is a object that has an `apply` method with one parameter, the compiler. For example, a plugin is a function, and the function prototype defines an `apply` method. But you can also define a "class" with the `apply` method.

Many objects in webpack extend the Tapable class, which exposes a `plugin` method. And with the `plugin` method, plugins can bind custom stuff.

The following example is the Compiler exposing the `"compile"` plugin interface, which is called when the Compiler compiles.

``` javascript
compiler.plugin("compile", function(params) {
	// Just print a text
	console.log("Compiling...");
});
```

A plugin only gets a reference to the compiler object, so if it wants to plug stuff into other webpack objects it has to gain access to them, i.e., to the Compilation object:

``` javascript
compiler.plugin("compilation", function(compilation) {
	compilation.plugin("optimize", function() {
		console.log("The compilation is now optimizing your stuff");
	});
});
```

## interface types

There are multiple types of plugin interfaces.

* Timing
	* sync (default): As seen above. Use return.
	* async: Last parameter is a callback. Signature: function(err, result)
	* parallel: The handlers are invoked parallel (async).

* Return value
	* not bailing (default): No return value.
	* bailing: The handlers are invoked in order until one handler returns something.
	* parallel bailing: The handlers are invoked in parallel (async). The first returned value (by order) is significant.
	* waterfall: Each handler gets the result value of the last handler as an argument.

# `Compiler` instance

### `run(compiler: Compiler)` async

The `run` method of the Compiler is used to start a compilation. This is not called in watch mode.

### `watch-run(watching: Watching)` async

The `watch` method of the Compiler is used to start a watching compilation. This is not called in normal mode.

### `compilation(c: Compilation, params: Object)`

A `Compilation` is created. A plugin can use this to obtain a reference to the `Compilation` object. The `params` object contains useful references.

### `normal-module-factory(nmf: NormalModuleFactory)`

A `NormalModuleFactory` is created. A plugin can use this to obtain a reference to the `NormalModuleFactory` object.

### `context-module-factory(cmf: ContextModuleFactory)`

A `ContextModuleFactory` is created. A plugin can use this to obtain a reference to the `ContextModuleFactory` object.

### `compile(params)`

The Compiler starts compiling. This is used in normal and watch mode. Plugins can use this point to modify the `params` object (i. e. to decorate the factories).

### `make(c: Compilation)` parallel

Plugins can use this point to add entries to the compilation or prefetch modules. They can do this by calling `addEntry(context, entry, name, callback)` or `prefetch(context, dependency, callback)` on the Compilation.

### `after-compile(c: Compilation)` async

The compile process is finished and the modules are sealed. The next step is to emit the generated stuff. Here modules can use the results in some cool ways.

The handlers are not copied to child compilers.

### `emit(c: Compilation)` async

The Compiler begins with emitting the generated assets. Here plugins have the last chance to add assets to the `c.assets` array.

### `after-emit(c: Compilation)` async

The Compiler has emitted all assets.

### `done(stats: Stats)`

All is done.

### `failed(err: Error)`

The Compiler is in watch mode and a compilation has failed hard.

### `invalid()`

The Compiler is in watch mode and a file change is detected. The compilation will be begin shortly (`options.watchDelay`).

### `after-plugins()`

All plugins extracted from the options object are added to the compiler.

### `after-resolvers()`

All plugins extracted from the options object are added to the resolvers.

## NormalModuleFactory

### `before-resolve(data)` async waterfall

Before the factory starts resolving. The `data` object has this properties:

* `context` The absolute path of the directory for resolving.
* `request` The request of the expression.

Plugins are allowed to modify the object or to pass a new similar object to the callback.

### `after-resolve(data)` async waterfall

After the factory has resolved the request. The `data` object has this properties:

* `request` The resolved request. It acts as an identifier for the NormalModule.
* `userRequest` The request the user entered. It's resolved, but does not contain pre or post loaders.
* `rawRequest` The unresolved request.
* `loaders` A array of resolved loaders. This is passed to the NormalModule and they will be executed.
* `resource` The resource. It will be loaded by the NormalModule.
* `parser` The parser that will be used by the NormalModule.

## ContextModuleFactory

### `before-resolve(data)` async waterfall

### `after-resolve(data)` async waterfall

### `alternatives(options: Array)` async waterfall

# `Compilation` instance
```javascript
compiler.plugin("compilation", function(compilation) {
    //the main compilation instance
    //all subsequent methods are derived from compilation.plugin
});
```

### `normal-module-loader`
```javascript
compilation.plugin('normal-module-loader', function(loaderContext, module) {
    //this is where all the modules are loaded
    //one by one, no dependencies are created yet
});
```

### `seal`

The sealing of the compilation has started.

### `optimize`

Optimize the compilation.

### `optimize-tree(chunks, modules)` async

Async optimization of the tree.

### `optimize-modules(modules: Module[])`

Optimize the modules.

### `after-optimize-modules(modules: Module[])`

Optimizing the modules has finished.

### `optimize-chunks(chunks: Chunk[])`

Optimize the chunks.

### `after-optimize-chunks(chunks: Chunk[])`

Optimizing the chunks has finished.

### `revive-modules(modules: Module[], records)`

Restore module info from records.

### `optimize-module-order(modules: Module[])`

Sort the modules in order of importance. The first is the most important module. It will get the smallest id.

### `optimize-module-ids(modules: Module[])`

Optimize the module ids.

### `after-optimize-module-ids(modules: Module[])`

Optimizing the module ids has finished.

### `record-modules(modules: Module[], records)`

Store module info to the records.

### `revive-chunks(chunks: Chunk[], records)`

Restore chunk info from records.

### `optimize-chunk-order(chunks: Chunk[])`

Sort the chunks in order of importance. The first is the most important chunk. It will get the smallest id.

### `optimize-chunk-ids(chunks: Chunk[])`

Optimize the chunk ids.

### `after-optimize-chunk-ids(chunks: Chunk[])`

Optimizing the chunk ids has finished.

### `record-chunks(chunks: Chunk[], records)`

Store chunk info to the records.

### `before-hash`

Before the compilation is hashed.

### `after-hash`

After the compilation is hashed.

### `before-chunk-assets`

Before creating the chunk assets.

### `additional-chunk-assets(chunks: Chunk[])`

Create additional assets for the chunks.

### `record(compilation, records)`

Store info about the compilation to the records

### `optimize-chunk-assets(chunks: Chunk[])` async

Optimize the assets for the chunks.

The assets are stored in `this.assets`, but not all of them are chunk assets. A `Chunk` has a property `files` which points to all files created by this chunk. The additional chunk assets are stored in `this.additionalChunkAssets`.

### `after-optimize-chunk-assets(chunks: Chunk[])`

The chunk assets has been optimized.

### `optimize-assets(assets: Object{name: Source})` async

Optimize all assets.

The assets are stored in `this.assets`.

### `after-optimize-assets(assets: Object{name: Source})`

The assets has been optimized.

### `build-module`

Before a module build has started.

### `succeed-module`

A module has been built successfully.

### `failed-module`

The module build has failed.

### `module-asset(module, filename)`

An asset from a module was added to the compilation.

### `chunk-asset(chunk, filename)`

An asset from a chunk was added to the compilation.


## `Parser` instance (`compiler.parser`)

### `program(ast)` bailing

General purpose plugin interface for the AST of a code fragment.

### `statement(statement: Statement)` bailing

General purpose plugin interface for the statements of the code fragment.

### `call <identifier>(expr: Expression)` bailing

`abc(1)` => `call abc`

`a.b.c(1)` => `call a.b.c`

### `expression <identifier>(expr: Expression)` bailing

`abc` => `expression abc`

`a.b.c` => `expression a.b.c`

### `expression ?:(expr: Expression)` bailing

`(abc ? 1 : 2)` => `expression ?!`

Return a boolean value to omit parsing of the wrong path.

### `typeof <identifier>(expr: Expression)` bailing

`typeof a.b.c` => `typeof a.b.c`

### `statement if(statement: Statement)` bailing

`if(abc) {}` => `statement if`

Return a boolean value to omit parsing of the wrong path.

### `label <labelname>(statement: Statement)` bailing

`xyz: abc` => `label xyz`

### `var <name>(statement: Statement)` bailing

`var abc, def` => `var abc` + `var def`

Return `false` to not add the variable to the known definitions.

### `evaluate <expression type>(expr: Expression)` bailing

Evaluate an expression.

### `evaluate typeof <identifier>(expr: Expression)` bailing

Evaluate the type of an identifier.

### `evaluate Identifier <identifier>(expr: Expression)` bailing

Evaluate a identifier that is a free var.

### `evaluate defined Identifier <identifier>(expr: Expression)` bailing

Evaluate a identifier that is a defined var.

### `evaluate CallExpression .<property>(expr: Expression)` bailing

Evaluate a call to a member function of a successfully evaluated expression.


## Resolvers

* `compiler.resolvers.normal` Resolver for a normal module
* `compiler.resolvers.context` Resolver for a context module
* `compiler.resolvers.loader` Resolver for a loader

Any plugin should use `this.fileSystem` as fileSystem, as it's cached. It only has async named functions, but they may behave sync, if the user uses a sync file system implementation (i. e. in enhanced-require).

To join paths any plugin should use `this.join`. It normalizes the paths. There is a `this.normalize` too.

A bailing async forEach implementation is available on `this.forEachBail(array, iterator, callback)`.

To pass the request to other resolving plugins, use the `this.doResolve(types: String|String[], request: Request, callback)` method. `types` are multiple possible request types that are tested in order of preference.

``` javascript
interface Request {
	path: String // The current directory of the request
	request: String // The current request string
	query: String // The query string of the request, if any
	module: boolean // The request begins with a module
	directory: boolean // The request points to a directory
	file: boolean // The request points to a file
	resolved: boolean // The request is resolved/done
	// undefined means false for boolean fields
}

// Examples
// from /home/user/project/file.js: require("../test?charset=ascii")
{
	path: "/home/user/project",
	request: "../test",
	query: "?charset=ascii"
}
// from /home/user/project/file.js: require("test/test/")
{
	path: "/home/user/project",
	request: "test/test/",
	module: true,
	directory: true
}
```

### `resolve(context: String, request: String)`

Before the resolving process starts.

### `resolve-step(types: String[], request: Request)`

Before a single step in the resolving process starts.

### `module(request: Request)` async waterfall

A module request is found and should be resolved.

### `directory(request: Request)` async waterfall

A directory request is found and should be resolved.

### `file(request: Request)` async waterfall

A file request is found and should be resolved.

### The plugins may offer more extensions points

Here is a list what the default plugins in webpack offer. They are all `(request: Request)` async waterfall.

The process for normal modules and contexts is `module -> module-module -> directory -> file`.

The process for loaders is `module -> module-loader-module -> module-module -> directory -> file`.

#### `module-module`

A module should be looked up in a specified directory. `path` contains the directory.

#### `module-loader-module` (only for loaders)

Used before module templates are applied to the module name. The process continues with `module-module`.