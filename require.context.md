``` javascript
var dir = require.context("./dir");
// returns a context function, which behaves like a require
//  from this directory
require("./dir/file") === dir("./file");

// the context function has a property "keys" which is a function
//  returning all possible parameters.
// i.e. if "./dir" only contains "file.js" and a subdirectory "sub" with "subfile.js"
dir.keys()
// => ["./file.js", "./file", "./sub/subfile.js", "./sub/subfile"]

// the require.context function can take up to 3 parameters
// require.context(request, recursive, regExp)
// - recursive: boolean - should subdirectories be included
// - regExp: RegExp - create a function that only allows parameters
//    that matches this RegExp
require.context("./dir", false, /^\.\/[^\.]$/).keys()
// => ["./file"]
```

### webpack

``` javascript
// webpack automatically creates a "context" when using expressions in the require/define call.
var someFile = "file.js";
require("./dir/" + someFile);
// => is equal to require.context("./dir")("./" + someFile);

// It can transform complex expressions to reguar expressions
var x = "il";
require("./dir/f" + x + "e")
// => require.context("./dir", true, /^\.\/f.*e$/)("./f" + x + "e")

// It the case you assign the require function to an variable or use it in some unexpected way
//  webpack automatically creates a context for the current directory
var x = require; // => var x = require.context(".");
x("./dir/file");
```