# What are loaders?

Loaders are transformations that are applied on a resource file of your app. They are functions (running in node.js) that take the source of a resource file are parameter and return new source.

## Loader features

* Loaders can be chained. They are applied in a pipeline to the resource. The final loader is expected to return javascript, the other can return arbitrary format (which is passed to the next loader)
* Loaders can be synchronous and asynchronous.
* Loaders run in node.js and can do everything thats possible there.
* Loaders accept query parameters. This can be used to pass configuration to the loader.
* Loaders can be bound to extension / RegExps in the configuration.
* Loaders can be published / installed through `npm`.
* Normal modules can export a loader in addition to the normal `main` via `package.json` `loader`.
* Loaders can access the configuration.
* Plugins can give loaders more features.
* Loaders can emit additional arbitrary files.
* [[etc. | loaders]]

If you are interested in some loader examples head of to the [[list of loaders]].

# Resolving loaders

Loaders are [[resolved similar to modules | resolving]]. A loader module is expected to export a function and to be written in node.js compatible javascript. In the common case you manage loaders with npm, but you can also have loaders as files in your app.

Module names with the suffix `-loader` are tried in addition to the exact name.

# Usage

There are multiple ways to use loaders in your app:

* explicit in the `require` statement
* configured via CLI
* configured via configuration

## loaders in `require`

It's possible to specify the loaders in the `require` statement (or `define`, `require.ensure`, etc.). Just separate loaders from resource with `!`. Each part is resolved relative to the current directory.

``` javascript
require("./loader!./dir/file.txt");
// => uses the file "loader.js" in the current directory to transform
//    "file.txt" in the folder "dir".

require("jade!./template.jade");
// => uses the "jade-loader" (that is installed from npm to "node_modules")
//    to transform the file "template.jade"

require("style!css!less!bootstrap/less/bootstrap.less");
// => the file "bootstrap.less" in the folder "less" in the "bootstrap"
//    module (that is installed from github to "node_modules") is
//    transformed by the "less-loader". The result is transformed by the
//    "css-loader" and than by the "style-loader".
```

## [[CLI]]

You can bind loaders to a extension via CLI:

``` sh
$ webpack --module-bind jade --module-bind css=style!css
```

This uses the loader "jade" for ".jade" files and the loaders "style" and "css" for ".css" files.

## [[Configuration]]

You can bind loaders to a RegExp via configuration:

``` javascript
{
	module: {
		loaders: [
			{ test: /\.jade$/, loader: "jade" },
			// => "jade" loader is used for ".jade" files

			{ test: /\.css$/, loader: "style!css" },
			// => "style" and "css" loader is used for ".css" files
			// Alternative syntax:
			{ test: /\.css$/, loaders: ["style", "css"] },
		]
	}
}
```

## Query parameters

Loader can be passed query parameters via a query string (just like in the web). The query string is appended to the loader with `?`. I. e. `url-loader?mimetype=image/png`.

Note: The format of the query string is up to the loader. See format in the loader documentation. The most loaders accept parameters in the normal query format (`?key=value&key2=value2`) and as JSON object (`?{"key":"value","key2":"value2"}`).

### in `require`

``` javascript
require("url-loader?mimetype=image/png!./file.png");
```

### CLI

``` sh
webpack --module-bind "png=url-loader?mimetype=image/png"
```

### Configuration

``` javascript
{ test: /\.png$/, loader: "url-loader?mimetype=image/png" }
```

or

``` javascript
{
	test: /\.png$/,
	loader: "url-loader"
	query: { mimetype: "image/png" }
}
```

