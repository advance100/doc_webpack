# require.include

``` javascript
require.include("./file");
```

`require.include` is threaded as dependency while compiling, but is removed from the output. So it hasn't any impact on runtime, but the module is included in the chunk.

`require.include` can be useful if a module is in multiple child chunks. A `require.include` in the parent would include the module and the instances of the modules in the chunk chunks would disappear.

The following codes are equivalent (except whitespace):

``` javascript
require.ensure(["./file"], function(require) {
  require("./file2");
});
```

``` javascript
require.ensure([], function(require) {
  require.include("./file");
  require("./file2");
});
```

Both result in the same chunks and generated code.