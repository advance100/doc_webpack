The webpack-dev-server is a little node.js express server, which uses the [[webpack-dev-middleware]] to serve a webpack bundle. It also has a little runtime which is connected to the server via socket.io. The server emits information about the compilation state to the client, which reacts to those events. You can choose between different modes, depending on your needs. So lets say you have the following config file:

```javascript
module.exports = {
  entry: {
    app: ['./app/main.js']
  },
  output: {
    path: './build',
    filename: 'bundle.js'
  }
};
```

You have an app folder with your initial entry point that webpack will bundle into a *bundle.js* file in the build folder.

## Inline mode
The webpack-dev-server will serve the files in the current directory, unless you configure a specific content base.

```sh
$ webpack-dev-server --content-base build/
```

Now webpack-dev-server will serve the files in your build folder. You will need to add an index.html that loads up your bundled files.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```

By default go to `localhost:8080/` to launch your app.

## Hot mode
By adding a script to your index.html file and a special entry point in your configuration you will be able to get live reloads when doing changes to your files. Change your index.html file to this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <!-- It is important that you point to the full url -->
  <script src="http://localhost:8080/webpack-dev-server.js"></script>
  <script src="bundle.js"></script>
</body>
</html>
```

And make sure you have the special `webpack/hot/dev-server` entry point in your configuration:

```javascript
module.exports = {
  entry: {
    app: ['webpack/hot/dev-server', './app/main.js']
  },
  output: {
    path: './build',
    filename: 'bundle.js'
  }
};
```
When you do changes to files the browser will automatically reload.

## Hot mode with indication
When running webpack-dev-server you can also head to `localhost:8080/webpack-dev-server`. Going to this URL will not only load your application, but also indicate when a rebundling is in progress. Using this url does not require you to insert the webpack-dev-server script. So using `localhost:8080/webpack-dev-server` with the current setup would require this index.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```

## CLI

``` sh
$ webpack-dev-server <entry>
```

All [[CLI]] options are valid for the dev-server too, but there is no `<output>` default argument. For the [[CLI]] a `webpack.config.js` is accepted too.

There are some additional options:

* `--content-base <file/directory/url>`: Base path for the content
* `--quiet`: Don't output anything to the console
* `--no-info`: Suppress boring information
* `--port <number>`
* `--inline`: embed the webpack-dev-server runtime into the bundle
* `--hot`: adds the `HotModuleReplacementPlugin` and switch the server to hot mode. (Note: make sure you don't add `HotModuleReplacementPlugin` twice)

## API

``` javascript
var WebpackDevServer = require("webpack-dev-server");
var webpack = require("webpack");

var compiler = webpack({
	// configuration
});
var server = new WebpackDevServer(compiler, {
	// webpack-dev-server options
	contentBase: "/path/to/directory",
	// or: contentBase: "http://localhost/",
	
	hot: true,
	// Enable special support for Hot Module Replacement
	// Page is no longer updated, but a "webpackHotUpdate" message is send to the content
	// Use "webpack/hot/dev-server" as additional module in your entry point

	// webpack-dev-middleware options
	quiet: false,
	noInfo: false,
	lazy: true,
	watchDelay: 300,
	publicPath: "/assets/",
	headers: { "X-Custom-Header": "yes" },
	stats: { colors: true }
});
server.listen(8080, "localhost", function() {});
```

See [[webpack-dev-middleware]] for documentation on middleware options.

## Combining with an existing server

You can run the webpack-dev-server parallel to the existing server. Just use the complete path to the dev server in the `<script>` and the `publicPath`. Pass the URL to the existing server as `contentBase`.

Example:

* webpack-dev-server on port 8090.
* existing server on port 8080.
* `<script src="http://localhost:8090/assets/bundle.js">`
* `publicPath = "http://localhost:8090/assets/"`
* `contentBase = "http://localhost:8080/"`
* open `http://localhost:8090/webpack-dev-server`