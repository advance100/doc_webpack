``` javascript
// load stuff asynchronously
require.ensure(["./dependency1"], function(require) {
  // dependency1 is prepared to be required synchronously.
  // but dependency1 is not executed

  var dep2 = require("./dependency2");

  // in webpack dependency1 and dependency2 are placed in a chunk
  // see chapter chunks

  // in enhanced-require the file content of dependency1 is loaded into memory
  // to make the require call free of I/O
});
```