# writing webpack plugins

A (webpack) plugin is a object that has a `apply` method with one parameter, the compiler. I. e. a function is a plugin, because the function prototype defines a `apply` method. But you also can define a "class" with the `apply` method.

Many objects in webpack extends the Tapable class, which mean they expose a `plugin` method, which plugins can use to bind custom stuff.

I. e. the Compiler expose the `"compile"` plugin interface, which is called when the Compiler compiles

``` javascript
compiler.plugin("compile", function(params) {
  // Just print a text
  console.log("Compiling...");
});
```

A plugin only gets a reference to the compiler object, so if it want to plugin stuff into other webpack objects it have to gain access to them. I. e. the Compilation object:

``` javascript
compiler.plugin("compilation", function(compilation) {
  compilation.plugin("optimize", function() {
    console.log("The compilation is now optimizing your stuff");
  });
});
```

## interface types

There are multiple types of plugin interfaces.

* sync (default): As seen above. Use return.
* async: Last parameter is a callback. Signature: function(err, result)
* parallel: The handlers are invoked parallel (async).

* not bailing (default): No return value.
* bailing: The handlers are invoked in order until one handler return something.
* parallel bailing: The handlers are invoked parallel (async). The first (by order) returned value is significant.
* waterfall: Each handler get the result value of the last handler as argument.

## Compiler

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

### `after-resolve(data)` async waterfall

## ContextModuleFactory

### `before-resolve(data)` async waterfall

### `after-resolve(data)` async waterfall

### `alternatives(options: Array)` async waterfall

## Compilation

### `seal()`

### `optimize()`

### `optimize-modules(modules: Module[])`

### `after-optimize-modules(modules: Module[])`

### `optimize-chunks(chunks: Chunk[])`

### `after-optimize-chunks(chunks: Chunk[])`

### `optimize-module-ids(modules: Module[])`

### `after-optimize-module-ids(modules: Module[])`

### `optimize-chunk-ids(chunks: Chunk[])`

### `after-optimize-chunk-ids(chunks: Chunk[])`

### `optimize-chunk-assets(chunks: Chunk[])` async

### `optimize-assets(assets: Object{name: Source})`

### `after-optimize-assets(assets: Object{name: Source})`

### `build-module`

### `succeed-module`

### `failed-module`

## Parser `compiler.parser`

### `call <identifier>`

### `expression <identifier>`

### `evaluate <expression type>`

### `evaluate typeof <identifier>`

### `typeof <identifier>`

### `statement if`