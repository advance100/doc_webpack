## Installation

``` sh
$ npm install webpack -g
```

The `webpack` command is now available globally.




## Pure CLI

``` sh
webpack <entry> <output>
```



### `entry`

Pass a file or a request string. You can pass multiple entries (every entry is loaded on startup).

If you pass a pair in the form `<name>=<request>` you can create an additional entry point.

It will be mapped to the configuration option `entry`.



### `output`

Pass a path to a file.

It will be mapped to the configuration options `output.path` and `output.filename`.



### configuration options

Many configuration options are mapped from CLI options. I. e. `--debug` maps to `debug: true`, or `--output-library-target` to `output.libraryTarget`.

You see a list of all options, if you don't pass any option.



### plugins

Some plugins are mapped to CLI options. I. e. `--define <string>=<string>` maps to the `DefinePlugin`.

You see a list of all options, if you don't pass any option.



### Development shortcut `-d`

Equals to `--debug` `--devtool source-map` `--output-pathinfo`



### Production shortcut `-p`

Equals to `--optimize-minimize` `--optimize-occurence-order`



### Watch mode `--watch`

Watches all dependencies and recompile on change.

> Note: It's recommended to combine it with `--cache`.



### display options

#### `--progress`

Display a compilation progress to stderr.

#### `--json`

Write JSON to stdout instead of a human readable format.

> Hint: Try to put the result into the [anaylse tool](http://webpack.github.com/analyse).

#### `--colors`

Use colors to display the statistics.

#### `--sort-modules-by`, `--sort-chunks-by`, `--sort-assets-by`

Sort the modules/chunks/assets list by a column.

#### `--display-chunks`

Display the separation of the modules into chunks.

#### `--display-reasons`

Show more information about the reasons why a module is included.