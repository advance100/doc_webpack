# webpack usage

* executable (node.js)
* as module in node.js
* [with grunt](https://github.com/webpack/grunt-webpack)

## executable

``` text
webpack <entry> <output>
```

`entry` must be a string you would normally write as expression into a `require`.

`output` must be a path to a file. The bundle stored here. There may be emitted more files into this directory.

For more options see [[webpack detailed usage]]

The executable tries to read a file `webpack.config.js` in the current directory. More [[webpack options]] are extracted from this file.

## as module in node.js

``` javascript
var webpack = require("webpack");
webpack({ /* webpack options */ }, function(err, stats) {
  // ...
});
```