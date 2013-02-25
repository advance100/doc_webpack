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
--output-public-path <string>
--output-path-info
--output-library <string>
--output-library-target <var|window|this|commonjs|commonjs2>
--console
--cache
--watch, -w
--watch-delay <number>
--debug, -d
--devtool eval
--progress
--resolve-alias <string=string>
--resolve-loader-alias <string=string>
--optimize-max-chunks <number>
--optimize-minimize
--plugin <file>
--bail

--json, -j
--colors, -c
--sort-modules-by <id|size|name>
--sort-chunks-by <id|size>
--sort-assets-by <name|size>
--display-chunks
--display-reasons, --verbose, -v
```

See [[webpack options]] for the most arguments.

## `--progress`

Display a compilation progress to stderr.

## `--json`

Write JSON to stdout instead of a human readable format.

## `--colors`

Use colors to display the statistics.

## `--sort-modules-by`, `--sort-chunks-by`, `--sort-assets-by`

Sort the modules/chunks/assets list by a column.

## `--display-chunks`

Display the separation of the modules into chunks.

## `--display-reasons`

Show more information about the reasons why a module is included.