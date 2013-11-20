A quick summary of all methods and variables availible in code compiled with webpack.

## Basic

#### `require(dependency: String)`

Returns the exports from a dependency. The call is sync. No request to the server is fired. The compiler ensures that the dependency is available.

Style: CommonJs

Example:

``` javascript
var $ = require("jquery");
var myModule = require("my-module");
```

---

#### `define([name: String], [dependencies: String[]], factoryMethod: function(...))`

The name argument is ignored. If the dependencies array is provided, the factoryMethod will be called with the exports of each dependency (in the same order). If the factoryMethod returns a value, this value is exported by the module. The call is sync. No request to the server is fired. The compiler ensures that each dependency is available.

Style: AMD

Example:

``` javascript
define(["jquery", "my-module"], function($, myModule) {
  // Do something with $ and myModule.
  // Export a function
  return function doSomething() {
    // Do something
  };
});
```

---

#### `module.exports`

This value is returned, when that module is required. It's default value is a new object.

Style: CommonJs

Example:

``` javascript
module.exports = function doSomething() {
  // Do something
};
```

---

#### `exports`

The exported object. It's the default value of `module.exports`. If `module.exports` gets overwritten, `exports` will no longer be exported.

Style: CommonJs

``` javascript
exports.someValue = 42;
exports.anObject = {
  x: 123
};
exports.aFunction = function doSomething() {
  // Do something
};
```

---

#### `require.resolve(dependency: String)`

Returns the module id of a dependency. The call is sync. No request to the server is fired. The compiler ensures that the dependency is available.

The module id is a number in webpack (in constast to node.js where it is a string, the filename).

Style: CommonJs

Example: 

``` javascript
var id = require.resolve("dependency");
typeof id === "number";
id === 0 // if dependecy is the entry point
id > 0 // elsewise
```

---

#### `module.id`

The module id of the current module.

Style: CommonJs

Example:

``` javascript
// in file.js
module.id === require.resolve("./file.js")
```

---

## Advanced

#### `require.cache`

Multiple requires to the same module result in only one module execution and only one export. Therefore a cache in the runtime exists. Removing values from this cache cause new module execution and a new export. This is only needed in rar cases.

Style: CommonJs

``` javascript
var d1 = require("dependency");
require("dependency") === d1
delete require.cache[require.resolve("dependency")];
require("dependency") !== d1
```

``` javascript
// in file.js
require.cache[module.id] === module
require("./file.js") === module.exports
delete require.cache[module.id];
require.cache[module.id] === undefined
require("./file.js") !== module.exports
require.cache[module.id] !== module
```

---

#### `require.ensure(dependencies: String[], callback: function(require), [chunkName: String])`

---

#### `require(dependencies: String[], callback: function(...))`

---

#### `require.include(dependency: String)`

---

#### `module.loaded`

---

#### `module.hot`

---

#### `__dirname`

---

#### `__filename`

---

#### `__resourceQuery`

---

#### `__webpack_public_path__`

---

#### `__webpack_require__`

---

#### `__webpack_chunk_load__`

---

#### `__webpack_modules__`

---

#### `DEBUG`