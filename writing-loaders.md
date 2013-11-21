TODO

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

``` javascript
module.exports = function(content) {
  return someSyncOperation(content, this.data.value);
};
module.exports.pitch = function(remainingRequest, precedingRequest, data) {
  if(someCondition()) {
    // fast exit
    return "module.exports = require(" + JSON.stringify(remainingRequest) + ");";
  }
  data.value = 42;
};
```


## loader context

### `version = 1`

Loader API version.

### `context: string`

The directory of the requiring module.

### `request: string`

The resolved request string.

### `query: string`

The query of the request for the current loader.

### `data: Object`

A data object shared between the pitch and the normal phase.

### `cacheable: function(flag = true: boolean)`

Make this loader result cacheable.

### `loaders: {request: string, path: string, query: string, module: function}[]`

An array of all the loaders. It is writeable in the pitch phase.

### `loaderIndex: number`

The index in the loaders array of the current loader.

### `resource: string`

The resource part of the request, including query.

### `resourcePath: string`

The resource file.

### `resourceQuery: string`

The query of the resource.

### `emitWarning: function(message: string)`

Emit a warning.

### `emitError: function(message: string)`

Emit a error.

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

Remove all dependencies of the loader result. Even initial dependencies and these of other loaders.

### `values: any` (out)

Pass values to the next loaders `inputValues`

### `inputValues: any`

Passed from the last loader.

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
