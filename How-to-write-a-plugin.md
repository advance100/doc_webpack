Plugins expose the full potential of the Webpack engine to third-party developers. Using staged build callbacks, developers can introduce their own behaviors into the Webpack build process. Building plugins is a bit more advanced than building loaders, because you'll need to understand some of the Webpack low-level internals to hook into them. Be prepared to read some source code!

## Compiler and Compilation

Among the two most important resources while developing plugins are the `compiler` and `compilation` objects. Understanding their roles is an important first step in extended the Webpack engine.

- The `compiler` object represents the fully configured Webpack environment. This object is built once upon starting Webpack, and is configured with all operational settings including options, loaders, and plugins. When applying a plugin to the Webpack environment, the plugin will receive a reference to this compiler. Use the compiler to access the main Webpack environment.

- A `compilation` object represents a single build of versioned assets. While running Webpack development middleware, a new compilation will be created each time a file change is detected, thus generating a new set of compiled assets. A compilation surfaces information about the present state of module resources, compiled assets, changed files, and watched dependencies. The compilation also provides many callback points at which a plugin may choose to step in and perform actions.

These two components are an integral part of any Webpack plugin (especially the `compilation`), so developers will benefit by familiarizing themselves with these source files:

- [Compiler Source](https://github.com/webpack/webpack/blob/master/lib/Compiler.js)
- [Compilation Source](https://github.com/webpack/webpack/blob/master/lib/Compilation.js)

## Basic Plugin Architecture

Plugins are instanceable objects with an `apply` method on their prototype. This `apply` method is called once by the Webpack compiler while installing the plugin. The `apply` method is given a reference to the underlying Webpack compiler, which grants access to compiler callbacks. A simple plugin is structured as follows:

```javascript
function HelloWorldPlugin(options) {
  // Setup the plugin instance with options...
}

HelloWorldPlugin.prototype.apply = function(compiler) {
  compiler.plugin('done', function() {
    console.log('Hello World!'); 
  });
};

module.exports = HelloWorldPlugin;
```

Then to install the plugin, just include an instance of it in your Webpack config `plugins` array:

```javascript
var HelloWorldPlugin = require('hello-world');

var webpackConfig = {
  // ... config settings here ...
  plugins: [
    new HelloWorldPlugin({ ... options ... })
  ]
};
```

## Accessing the compilation

Using the compiler object, you may bind callbacks that provide a reference to each new compilation. These compilations provide callbacks for hooking into numerous steps within the build process.

```javascript
function HelloCompilationPlugin(options) {}

HelloCompilationPlugin.prototype.apply = function(compiler) {

  // Setup callback for accessing a compilation:
  compiler.plugin("compilation", function(compilation) {
    
    // Now setup callbacks for accessing compilation steps:
    compilation.plugin("optimize", function() {
      console.log("Assets are being optimized.");
    });
  });
});

module.exports = HelloCompilationPlugin;
```

For more information on what callbacks are available on the `compiler`, `compilation`, and other important objects, see the [[plugins API|plugins]] doc.

## Async compilation plugins

Some plugin callbacks are asynchronous, and pass a callback that must be invoked when your plugin is finished.

```javascript
function HelloAsyncPlugin(options) {}

HelloAsyncPlugin.prototype.apply = function(compiler) {

  compiler.plugin("emit", function(compilation, callback) {
    console.log("Doing something asynchronously...");

    setTimeout(function() {
      console.log("Done with async work...");
      callback();
    }, 1000);
  });
});

module.exports = HelloAsyncPlugin;
```