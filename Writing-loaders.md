# writing loaders

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
### `context: string`
### `request: string`
### `query: string`
### `data: Object`
### `cacheable: function(flag = true: boolean)`
### `loaders: {request: string, path: string, query: string, module: function}[]`
### `loaderIndex: number`
### `resource: string`
### `resourcePath: string`
### `resourceQuery: string`
### `emitWarning: function(message: string)`
### `emitError: function(message: string)`
### `exec: function(code: string, filename: string)`
### `resolve: function(context: string, request: string, callback: function(err, result: string))`
### `resolveSync: function(context: string, request: string) -> string`
### `addDependency: function(file: string)`
### `addContextDependency: function(directory: string)`
### `clearDependencies: function()`
### `values: any` (out)
### `inputValues: any`
### `options: {...}`
### `debug: boolean`
### `minimize: boolean` (webpack)
### `webpack = true` (webpack)
### `emitFile: function(name: string, content: Buffer|String, sourceMap: {...}` (webpack)
### `_compilation: Compilation` (webpack)
### `_compiler: Compiler` (webpack)

