# webpack detailed usage

# executable

```
webpack <entry> <entry> <entry> <output-file>
--config <file>
--context <directory>
--entry <string>, -e
--module-bind <string|string=string>
--module-bind-post <string|string=string>
--module-bind-pre <string|string=string>
--output-path <directory>
--output-file <string>
--output-chunk-file <string>
--output-named-chunk-file <string>
--output-source-map-file <string>
--output-public-path <string>
--output-jsonp-function <string>
--output-path-info
--output-library <string>
--output-library-target <var|window|this|commonjs|commonjs2>
--records-input-path <file>
--records-output-path <file>
--records-path <file>
--define <string=string>
--target <string>
--cache
--watch, -w
--watch-delay <number>
--hot, -h
--debug
--devtool <string>
--progress
--resolve-alias <string=string>
--resolve-loader-alias <string=string>
--optimize-max-chunks <number>
--optimize-minimize
--optimize-occurence-order
--optimize-dedupe
--prefetch <string>
--provide <string|string=string>
--plugin <file>
--bail
--profile

-d
-p

--json, -j
--colors, -c
--sort-modules-by <id|size|name>
--sort-chunks-by <id|size>
--sort-assets-by <name|size>
--display-chunks
--display-reasons, --verbose, -v
```

`<entry>` can be a require expression than it's added to the `main` entry chunk.

`<entry>` can be `<name>=<require expression>` than it's added to the `<name>` entry chunk.

See [[webpack options]] for the most arguments.

## `-d`

Development shortcut: `--debug` `--devtool source-map` `--output-pathinfo`

## `-p`

Production shortcut: `--optimize-minimize` `--optimize-occurence-order`

## `--progress`

Display a compilation progress to stderr.

## `--json`

Write JSON to stdout instead of a human readable format.

> Hint: Try to put the result into the [anaylse tool](http://webpack.github.com/analyse).

## `--colors`

Use colors to display the statistics.

## `--sort-modules-by`, `--sort-chunks-by`, `--sort-assets-by`

Sort the modules/chunks/assets list by a column.

## `--display-chunks`

Display the separation of the modules into chunks.

## `--display-reasons`

Show more information about the reasons why a module is included.