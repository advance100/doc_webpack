# require.context

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