For big web apps it's not efficient to put all code into a single file. Especially if some blocks of code are only required under some circumstances. Webpack has a feature to split you codebase into so called chunks which are loaded on demand. This feature is also called "code splitting". Names for similar features in other bundlers: layers, rollups, fragments.

The splitting points are chosen by the developer. By default the AMD `require` and `require.ensure` functions set a split point.

For a demo see the [example-app](http://webpack.github.io/example-app/). Check Network in DevTools.

## chunk types

There are these types of chunks:

### entry chunk

This type of chunk contains the module system, a entry module and load the entry module on loading.

The file of this chunk should be included in the HTML page.

There can be multiple entry chunks, but only one is loaded per page.

### normal chunk

This type of chunk do not contain the module system, but use some mechanism to add additional modules to the already loaded entry chunk.

The file of this chunk should not be included in the HTML page, but it is loaded by the module system.

There is a good chance that there are multiple normal chunks but they are not required.

### named chunk

Entry chunks and normal chunks can optionally have names. If they have, they emit an additional file with a special filename.

The additional file can be included in the HTML page to inject additional modules, which are otherwise loaded on demand. If injected, these modules are requested with the page load and do not require an additional round trip.

## code splitting example

``` javascript
// entry.js
var a = require("a");
var b = require("b");
require(["b", "c"], function(b2, c) {
  var d = require("d");
});
```

This creates a entry chunks containing `entry.js`, `a` and `b`, and a normal chunk with `c` and `d`. Dependencies of `a` and `b` are included in the entry chunk and dependencies of `c` and `d` in the additional chunk.

## more complex example

``` javascript
// entry.js
require("x");
require.ensure(["y"], function() { // load chunk 1
  require("y");
});
```

``` javascript
// x
require("b");
require.ensure([], function() { // load chunk 2
  require("a");
});
```

``` javascript
// y
require("c");
require(["a"], function(a) {}); // load chunk 2
```

``` text
entry chunk
 - entry.js
 - x
 - b
chunk 1
 - c
 - y
chunk 2
 - a
```

## Optimizing the chunk count

If your codebase is split into too much chunks, there are options to merge multiple chunks into one file.

> @sokra: I recommend to set many split points and let webpack merge them, because it has a good overall view of the compilation (with the chunk contents and sizes).