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

## webpack

``` javascript
// webpack automatically creates a "context" when using expressions in the require/define call.
var someFile = "file.js";
require("./dir/" + someFile);
// => is equal to require.context("./dir")("./" + someFile);

// It can transform complex expressions to regular expressions
var x = "il";
require("./dir/f" + x + "e")
// => require.context("./dir", true, /^\.\/f.*e$/)("./f" + x + "e")

// In case you assign the require function to an variable or use it in some unexpected way
//  webpack automatically creates a context for the current directory
var x = require; // => var x = require.context(".");
x("./dir/file");
```

### Internals

webpack reads the directory content and filter it by the provided RegExp. All possible request strings are calculated and resolved like normal requires. As result we have mappings from "possible request" to "module id". webpack generates a module (called context module) that contains that map and exports a require-like function.

Example:

We start with `require("./template/" + templateName + ".jade"`.

While parsing we extract the directory "/some/path/template" and the RegExp `/^\.\/.*\.jade$/`.

The directory is read and filtered by RegExp. As result we get these three possible request strings: `"./templateA.jade" "./templateB.jade" "./more/templateC.jade"`.

After resolving these modules we have a map:

``` javascript
{
    "./templateA.jade": 23,
    "./templateB.jade": 24,
    "./more/templateC.jade": 25
}
// 23, 24, 25 are module ids
```

Given that the context module get the id `22`. The original require is rewritten to:

``` javascript
require(22)("./" + templateName + ".jade")
```

See [full example here](https://github.com/webpack/webpack/tree/master/examples/require.context#examplejs).