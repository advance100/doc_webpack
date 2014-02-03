Today websites are evolving into web apps:

* More and more javascript is in a page.
* You can do more stuff in modern browsers.
* Fewer full page reloads → even more code in a page.

As a result there is **much** code on client side!

A big code base need to be organized. Module systems offer the option to split your code base into modules. There care for dependencies and exports of the module.

# Module system styles

There are multiple standards how to define dependencies and export values:

* `<script>`-tag style (without a module system)
* CommonJs
* AMD and some dialects of it
* ES6 modules
* and more...

## `<script>`-tag style

This is the way you would care with a modulized code base if you don't use a module system.

``` html
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libaryA.js"></script>
<script src="module3.js"></script>
```

Modules export an interface to the global object, i. e. the `window` object. Modules can access the interface of dependencies over the global object.

#### Common problems

* Conflicts in the global object.
* Order of loading is important.
* Developer have to resolve dependencies of modules/libraries.
* In big projects the list can get really long and difficult to manage.

## CommonJs: synchron `require`

This style uses a synchron `require` method to load a dependency (it returns the exported interface). A module can specify exports by adding properties to the `exports` object or setting the value of `module.exports`.

``` javascript
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```

It's used on server-side by [node.js](http://nodejs.org).

#### Pro

* Server-side modules can be reused
* There are already many modules in this style (npm)
* very simple and easy to use.

#### Contra

* blocking calls do not aplly well on networks. Requests are asynchronous.
* No parallel require of multiple modules

#### Implementations

* [node.js](http://nodejs.org/) - server-side
* [browserify](https://github.com/substack/node-browserify) - compile to one bundle
* [modules-webmake](https://github.com/medikoo/modules-webmake) - compile to one bundle
* [wreq](https://github.com/substack/wreq) - client-side

## AMD: asynchronous require

[`Asynchronous Module Definition`](https://github.com/amdjs/amdjs-api/wiki/AMD)

Other module systems (for the browser) had problems with the synchronous `require` (CommonJs) and introduced an asynchronous version (and a way to define modules and exporting values):

``` javascript
require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
```

#### Pro

* Fits to the asynchronous request style in networks.
* Parallel loading of multiple modules.

#### Contra

* Coding overhead. More difficult to read and write.
* Seems to be some kind of workaround.

#### Implementations

* [require.js](http://requirejs.org/) - client-side
* [curl](https://github.com/cujojs/curl) - client-side

Read more about [[CommonJs]] and [[AMD]].

---

# Transferring

Modules should be executed on the client, so they must be transferred from the server to the browser.

There are two extremas how to transfer modules:

* 1 request per module (i. e. require.js)
* all modules in one request (i. e. browserify and require.js when compiled)

Both are used in the wild, but both are suboptimal:

* 1 request per module
    * Pro: only required modules are transferred
    * Contra: many requests means much overhead
    * Contra: slow application startup, because of request latency
* all modules in one request
    * Pro: less request overhead, less latency
    * Contra: not (yet) required modules are transferred too

## Chunked transferring

A more flexible transferring is be better. A compromise between the extremas is better in most cases.

→ While compiling all modules: Split the set of modules into multiple smaller batches (chunks).

We get multiple smaller requests. Chunks with modules that are not required initially are only requested on demand. The initial request doesn't contain your complete code base and is smaller.

The "split points" are up to the developer and optional.

→ A big code base is possible!

Note: The [idea is from Google's GWT](https://developers.google.com/web-toolkit/doc/latest/DevGuideCodeSplitting). 

Read more about [[Code Splitting]].

# Why only javascript?

Why should a module system only help the developer with javascript? There are many other static resources that need to the handled:

* stylesheets
* images
* webfonts
* html for templating
* etc.

And also:

* coffeescript → javascript
* less stylesheets → css stylesheets
* jade templates → javascript which generates html
* i18n files → something
* etc.

This should be as easy as:

``` javascript
require("./style.css");
```

``` javascript
require("./style.less");
require("./template.jade");
require("./image.png");
```

Read more about [[Using loaders]] and [[Loaders]].

---

# Static analysis

When compiling all the modules a static analysis tries to find dependencies.

Traditionally this could only find simple stuff without expression, but i. e. `require("./template/" + templateName + ".jade")` is a common construct.

