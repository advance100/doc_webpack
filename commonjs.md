``` javascript
// require a module
var dependency = require("./dependency");
// each module is only excuted once and the same
//  exports are returned
// require("./dependency") === require("./dependency")

// export stuff
exports.stuff = 42;
// exports is a object that is returned by the require
//  function when require this module.

// choose to not use the exports object
module.exports = function doStuff() {};
// the value of module.exports is returned by the require
//  function when require this module.

//// Advanded stuff ////

// unload module
var id = require.resolve("./dependency");
delete require.cache[id];
var newDependency = require("./dependency");
// newDependency !== dependency;

if(typeof require !== "function") {
  // This code is removed when bundled and minimized
}
```