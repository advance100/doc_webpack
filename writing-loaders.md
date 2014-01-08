Writing a loader is pretty simple. A loader is just a file, which exports a function. That function is called by the compiler. The result of the previous loader is passed to the next loader. The `this` context is filled by the compiler with some useful methods. I. e. loaders can change there invocation style to async or get query parameters. The first loader is passed one argument: the content of the resource file. The compiler expect a result from the last loader. The first result should be a String or a Buffer (which is converted to a string), representing the javascript source code of the module. As additional optional second result is a SourceMap (as JSON object) expected.

A single result can be returned in sync mode. For multiple result the `this.callback` must be called. In async mode `this.async()` must be called. It returns `this.callback` if async mode is allowed. Then the loader must return `undefined` and call the callback.

Errors can be thrown in sync mode or the `this.callback` can be called with the error.

webpack allows async mode in every case.

enhanced-require allows async mode only with `require.ensure` or AMD `require`.

## examples

### sync loader

``` javascript
module.exports = function(content) {
  return someSyncOperation(content);
};
```

### async loader

``` javascript
module.exports = function(content) {
  var callback = this.async();
  if(!callback) return someSyncOperation(content);
  someAsyncOperation(content, function(err, result) {
    if(err) return callback(err);
    callback(null, result);
  });
};
```

### raw loader

By default the resource file is threaded as `utf-8` string and passed as String to the loader. By setting `raw` to `true` the loader is passed the raw Buffer.

Every loader is allowed to deliver its result as String or as Buffer. The compiler converts them between loaders.

``` javascript
module.exports = function(content) {
  assert(content instanceof Buffer);
  return someSyncOperation(content);
  // return value can be a Buffer too
  // This is also allowed if loader is not "raw"
};
module.exports.raw = true;
```

### pitching loader

The loaders are called from right to left. But in some cases loaders doesn't care for the results of the previous loader or the resource. They only care for metadata. The `pitch` method on the loaders is called from left to right before the loaders are called. If a loader delivers a result in the pitch method the process turns around and skips the remaining loaders, continuing with the calls to the more left loaders. `data` can be passed between pitch and normal call.

``` javascript
module.exports = function(content) {
  return someSyncOperation(content, this.data.value);
};
module.exports.pitch = function(remainingRequest, precedingRequest, data) {
  if(someCondition()) {
    // fast exit
    return "module.exports = require(" + JSON.stringify("-!" + remainingRequest) + ");";
  }
  data.value = 42;
};
```


## loader context

This stuff is available on `this` in a loader.

For the example this require call is used:

In `/abc/file.js`:

``` javascript
require("./loader1?xyz!loader2!./resource?rrr");
```

### `version = 1`

Loader API version.

### `context: string`

The directory of the module. Can be used as context for resolving other stuff.

In the example: `/abc` because `resource.js` is in this directory

### `request: string`

The resolved request string.

In the example: `"/abc/loader1.js?xyz!/abc/node_modules/loader2/index.js!/abc/resource.js?rrr"`

### `query: string`

The query of the request for the current loader.

In the example: in loader1: `"?xyz"`, in loader2: `""`

### `data: Object`

A data object shared between the pitch and the normal phase.

### `cacheable: function(flag = true: boolean)`

Make this loader result cacheable. By default it's not cacheable.

### `loaders: {request: string, path: string, query: string, module: function}[]`

An array of all the loaders. It is writeable in the pitch phase.

In the example:

``` javascript
[
  { request: "/abc/loader1.js?xyz",
    path: "/abc/loader1.js",
    query: "?xyz",
    module: [Function]
  },
  { request: "/abc/node_modules/loader2/index.js",
    path: "/abc/node_modules/loader2/index.js",
    query: "",
    module: [Function]
  }
]
```

### `loaderIndex: number`

The index in the loaders array of the current loader.

In the example: in loader1: `0`, in loader2: `1`

### `resource: string`

The resource part of the request, including query.

In the example: `"/abc/resource.js?rrr"`

### `resourcePath: string`

The resource file.

In the example: `"/abc/resource.js"`

### `resourceQuery: string`

The query of the resource.

In the example: `"?rrr"`

### `emitWarning: function(message: string)`

Emit a warning.

### `emitError: function(message: string)`

Emit an error.

### `exec: function(code: string, filename: string)`

Execute some code fragment like a module.

### `resolve: function(context: string, request: string, callback: function(err, result: string))`

Resolve a request like a require expression.

### `resolveSync: function(context: string, request: string) -> string`

Resolve a request like a require expression.

### `addDependency: function(file: string)`

Add a file as dependency of the loader result.

### `addContextDependency: function(directory: string)`

Add a directory as dependency of the loader result.

### `clearDependencies: function()`

Remove all dependencies of the loader result. Even initial dependencies and these of other loaders. Consider using `pitch`.

### `values: any` (out)

Pass values to the next loaders `inputValues`. If you know what your result exports if executed as module, set this value here (as a only element array).

### `inputValues: any`

Passed from the last loader. If you would execute the input argument as module, consider reading this variable for a shortcut (for performance).

### `options: {...}`

The options passed to the Compiler.

### `debug: boolean`

In debug mode

### `minimize: boolean` (webpack)

Should the result be minimized.

### `webpack = true` (webpack)

Is this compiled by webpack.

### `emitFile: function(name: string, content: Buffer|String, sourceMap: {...})` (webpack)

Emit a file.

### `_compilation: Compilation` (webpack)

Hacky access to the Compilation object of webpack.

### `_compiler: Compiler` (webpack)

Hacky access to the Compiler object of webpack.