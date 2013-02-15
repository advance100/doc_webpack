## Create a enhanced require function

``` javascript
var myRequire = require("enhanced-require")(module, {
	// options
});

// startup your application
myRequire("./startup");
```

## Usage

Than you can use them:

``` javascript
// use loaders
var fileContent = require("raw!"+__filename);

// use loaders automatically
var template = require("./my-template.jade");
// you need to pass this options: 
// { module: { loaders: [ { test: /\.jade$/, loader: "jade" } ] } }

var html = template({content: fileContent});

// use require.context
var directoryRequire = require.context("raw!./subdir");
var txtFile = directoryRequire("./aFile.txt");

// use require.ensure
require.ensure(["./someFile.js"], function(require) {
	var someFile = require("./someFile.js");
});

// use AMD define
require.define(["./aDep"], function(aDep) {
	aDep.run();
});

// use AMD require
require(["./bDep"], function(bDep) {
	bDep.doSomething();
});
```

## Hot Code Replacement

``` javascript
require("enhanced-require")(module, {
	hot: true, // enable hot code replacement
	watch: true // watch for changes
})("./startup");
```

For hot code reloading you need to follow the [hot code reloading spec](https://github.com/webpack/enhanced-require/wiki/HCR-Spec).

## Testing/Mocking

``` javascript
var er = require("enhanced-require");
it("should read the config option", function(done) {
	var subject = er(module, {
		substitutions: {
			// specify the exports of a module directly
			"../lib/config.json": {
				"test-option": { value: 1234 }
			}
		},
		substitutionFactories: {
			// specify lazy generated exports of a module
			"../lib/otherConfig.json": function(require) {
				// export the same object as "config.json"
				return require("../lib/config.json");
			}
		}
	})("../lib/subject");

	var result = subject.getConfigOption("test-option");
	should.exist(result);
	result.should.be.eql({ value: 1234 });
});
```