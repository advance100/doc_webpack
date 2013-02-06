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

* not bailing (default): No return value.
* bailing: The handlers are invoked in order until one handler return something.
* parallel bailing: The handlers are invoked parallel (async). The first (by order) returned value is significant.
* waterfall: Each handler get the result value of the last handler as argument.

## Compiler

### `run(compiler: Compiler)` async

### `watch-run(watching: Watching)` async

### `compilation(c: Compilation)`

### `normal-module-factory(nmf: NormalModuleFactory)`

### `context-module-factory(cmf: ContextModuleFactory)`

### `compile(params)`

### `make(c: Compilation)` async

### `after-compile(c: Compilation)` async

### `emit(c: Compilation)` async

### `after-emit(c: Compilation)` async

### `done(stats: Stats)`

### `failed(err: Error)`

### `invalid()`

### `after-plugins()`

### `after-resolvers()`

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