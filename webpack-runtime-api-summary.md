A quick summary of all methods and variables available in code compiled with webpack.

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

The name argument is ignored. If the `dependencies` array is provided, the factoryMethod will be called with the exports of each dependency (in the same order). If `dependencies` is not provided the factoryMethod is called with `require`, `exports` and `module` (for compatibility!). If the factoryMethod returns a value, this value is exported by the module. The call is sync. No request to the server is fired. The compiler ensures that each dependency is available.

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

#### `define(value: !Function)`

Just exports the provided `value`. The `value` cannot be a function.

Style: AMD (for compatibility!)

Example: 

``` javascript
define({
  answer: 42
});
```

---

#### `export: value`

Export the defined value. The label can occur before a function declaration or a variable declaration. The function name or variable name is the identifier under which the value is exported.

Style: Labeled modules

Example:

``` javascript
export: var answer = 42;
export: function method(value) {
  // Do something
};

```

---

#### `require: "dependency"`

Make all exports from the dependency available in the current scope. The `require` label can occur before a string. The dependency must export values with the `export` label. CommonJs or AMD modules cannot be consumed.

Style: Labeled modules

Example:

``` javascript
// in dependency
export: var answer = 42;
export: function method(value) {
  // Do something
};
```

``` javascript
require: "dependency";
method(answer);
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

Multiple requires to the same module result in only one module execution and only one export. Therefore a cache in the runtime exists. Removing values from this cache cause new module execution and a new export. This is only needed in rar cases (for compatibility!).

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

#### `require.ensure(dependencies: String[], callback: function([require]), [chunkName: String])`

Download additional dependencies on demand. The `dependencies` array lists modules that should be available. When they are, `callback` is called. If the callback is a function expression, dependencies in that source part are extracted and also loaded on demand. A single request is fired to the server, if not all modules are already available.

This creates a chunk. The chunk can be named. If a chunk with this name already exists, the dependecies are merged into that chunk and that chunks is used.

Style: CommonJs

Example:

``` javascript
// in file.js
var a = require("a");
require.ensure(["b"], function(require) {
  var c = require("c");
});
require.ensure(["d"], function() {
  var e = require("e");
}, "my chunk");
require.ensure([], function() {
  var f = require("f");
}, "my chunk");
/* This results in:
   * entry chunk
     - file.js
     - a
   * anonymous chunk
     - b
     - c
   * "my chunk"
     - d
     - e
     - f
*/
```

---

#### `require(dependencies: String[], [callback: function(...)])`

Behaves similar to `require.ensure`, but the callback is called with the exports of each dependency in the `dependencies` array. There is no option to provide a chunk name.

Style: AMD

Example:

``` javascript
// in file.js
var a = require("a");
require(["b"], function(b) {
  var c = require("c");
});
/* This results in:
   * entry chunk
     - file.js
     - a
   * anonymous chunk
     - b
     - c
*/
```

---

#### `require.include(dependency: String)`

Ensures that the dependency is available, but don't execute it. This can be use for optimizing the position of a module in the chunks.

Style: webpack

Example:

``` javascript
// in file.js
require.include("a");
require.ensure(["a", "b"], function(require) {
  // Do something
});
require.ensure(["a", "c"], function(require) {
  // Do something
});
/* This results in:
   * entry chunk
     - file.js
     - a
   * anonymous chunk
     - b
   * anonymous chunk
     - c
Without require.include "a" would be in both anonymous chunks.
The runtime behavior isn't changed.
*/
```

---

#### `module.loaded`

Style: node.js (for compatibility!)

---

#### `module.hot`

Style: webpack

---

#### `global`

Style: node.js

---

#### `process`

Style: node.js

---

#### `__dirname`

Style: node.js (for compatibility!)

---

#### `__filename`

Style: node.js (for compatibility!)

---

#### `__resourceQuery`

Style: webpack

---

#### `__webpack_public_path__`

Style: webpack

---

#### `__webpack_require__`

Style: webpack

---

#### `__webpack_chunk_load__`

Style: webpack

---

#### `__webpack_modules__`

Style: webpack

---

#### `DEBUG`

Style: webpack