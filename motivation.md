Today websites are evolving into web apps:

* More and more javascript is in a page.
* You can do more stuff in modern browsers.
* Fewer full page reloads → even more code in a page.

As a result there is **much** code on client side!

A big code base need to be organized. Module systems offer the option to split your code base into modules. There care for dependencies and exports of the module.

## Module system styles

There are multiple standards how to define dependencies and export values:

* `<script>`-tag style (without a module system)
* CommonJs
* AMD and some dialects of it
* ES6 modules
* and more...

### `<script>`-tag style

This is the way you would care with a modulized code base if you don't use a module system.

``` html
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libaryA.js"></script>
<script src="module3.js"></script>
```

Modules export an interface to the global object, i. e. the `window` object. Modules can access the interface of dependencies over the global object.

##### Common problems

* Conflicts in the global object.
* Order of loading is important.
* Developer have to resolve dependencies of modules/libraries.
* In big projects the list can get really long and difficult to manage.

### CommonJs: synchron `require`

This style uses a synchron `require` method to load a dependency (it returns the exported interface). A module can specify exports by adding properties to the `exports` object or setting the value of `module.exports`.

``` javascript
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```

It's used on server-side by [node.js](http://nodejs.org).

##### Pro

* Server-side modules can be reused
* There are already many modules in this style (npm)
* very simple and easy to use.

##### Contra

* blocking calls do not aplly well on networks. Requests are asynchronous.
* No parallel require of multiple modules

### AMD: asynchronous require

[`Asynchronous Module Definition`](https://github.com/amdjs/amdjs-api/wiki/AMD)

Other module systems (for the browser) had problems with the synchronous `require` (CommonJs) and introduced an asynchronous version (and a way to define modules and exporting values):

``` javascript
require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
```

##### Pro

* Fits to the asynchronous request style in networks.
* Parallel loading of multiple modules.

##### Contra

* Coding overhead. More difficult to read and write.
* Seems to be some kind of workaround.

