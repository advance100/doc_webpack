A loader is a node module exporting a `function`.

This function is called when a resource should be transformed by this loader.

In the simple case, when only a single loader is applied to the resource, the loader is called with one parameter: the content of the resource file as string.

The loader can access the [[loader API | loaders]] on the `this` context in the function.

A sync loader that only want to give a one value can simply `return` it. In every other case the loader can give back any number of values with the `this.callback(err, values...)` function. Errors are passed to the `this.callback` function or throwed in a sync loader.

The loader is expected to give back one or two values. The first value is a resulting javascript code as string or buffer. The second optional value is a SourceMap as javascript object.

In the complex case, when multiple loaders are chained, only the last loader gets the resource file and only the first loader is expected to give back one or two values (javascript and SourceMap). Values that any other loader give back are passed to the previous loader.

## Examples

``` javascript
// Identity loader
module.exports = function(source) {
  return source;
};
```

``` javascript
// Identity loader with SourceMap support
module.exports = function(source, map) {
  this.callback(null, source, map);
};
```

## Guidelines

Loaders should

### do only a single task

Loaders can be chained. Create loaders for every step, instead of a loader that does everything at once.

This also means they should not convert to javascript if not necessary.

Example: Render HTML from a template file by applying the query parameters

I could write a loader that compiles the template from source, execute it and return a module that exports a string containing the HTML code. This is bad.

Instead I should write loaders for every task in this usecase and apply them all (pipeline):

* jade-loader: Convert template to a module that exports a function.
* apply-loader: Takes a function exporting module and returns raw result by applying query parameters.
* html-loader: Takes HTML and exports a string exporting module.

### flag itself cachable if possible

Most loaders are cachable, so they should flag itself as cachable.

Just call `cachable` in the loader.

``` javascript
// Cachable identity loader
module.exports = function(source) {
  this.cachable();
  return source;
};
```

### mark dependencies

If a loader uses external resources, they **must** tell about that. This information is used to invalidate cachable loaders and recompile in watch mode.

``` javascript
// Loader adding a header
var path = require("path");
module.exports = function(source) {
  this.cachable();
  var callback = this.async();
  var headerPath = path.resolve("header.js");
  this.dependency(headerPath);
  fs.readFile(headerPath, "utf-8", function(err, header) {
    if(err) return callback(err);
    callback(null, header + "\n" + source);
  });
};
```

### resolve dependencies

In many languages these is some schema to specify dependencies. i. e. in css there is `@import` and `url(...)`. These dependencies should be resolved by the module system.

There are two options to do this:

* Transform them to `require`s.
* Use the `this.resolve` function to resolve the path

If the language only accept relative urls (like css: `url(file)` always means `./file`), there is the `~`-convection to specify references to modules:

``` text
url(file) -> require("./file")
url(~module) -> require("module")
```

## Read more

Read more about [[loaders]].
