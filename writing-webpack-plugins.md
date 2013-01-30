# writing webpack plugins

``` javascript
// options apply
after-plugins
after-resolvers

// compiler
run
watch-run
compilation
normal-module-factory
context-module-factory
compile
make
after-compile
emit
after-emit
done

// context-module-factory
before-resolve
after-resolve
alternatives

// normal-module-factory
before-resolve
after-resolve

// parser
call <identifier>
expression <identifier>
evaluate <expression type>
evaluate typeof <identifier>
typeof <identifier>
statement if

// compilation
seal
optimize
optimize-modules
after-optimize-modules
optimize-chunks
after-optimize-chunks
optimize-chunk-assets
optimize-assets
after-optimize-assets

// modules
build-module
succeed-module
failed-module
```