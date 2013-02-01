# amd

``` javascript
define(["./dependency1", "./dependency2"], function(dep1, dep2) {
  // dep1 and dep2 are the exports of the files dependency1 and dependency1

  // more dependencies
  var dep3 = require("./dependency3");
  // you can require more dependencies with a sync require.
  // (This is internally acutually threaded like commonjs require.)

  // more dependencies on demand
  require(["./dependency4"], function(dep4) {
    // the async version of require load more dependencies asynchronously.
    // see more in capter chunks
    // dep4 is the export of dependency4
  });

  // the returned value is exported
  return function doStuff() {};
});

define(function(require, exports, module) {
  // the commonjs wrapping style is also supported
});

define(["exports", "require", "./dependency"], function(exports, require, dep) {
  // the strings "exports", "module" and "require" have special meaning in AMD define/require
});

require(["./file"]);
// without function is possible to just load the files.

define("name", ...)
// named defines are allowed and will behave like anon defines. The name is ignored.
```