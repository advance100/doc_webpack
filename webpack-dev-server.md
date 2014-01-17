The webpack-dev-server is a little node.js express server, which uses the [[webpack-dev-middleware]] to serve a webpack bundle. It also has a little runtime which is connected to the server via socket.io. The server emit information about the compilation state to the client, which reacts on that events.

It serves static assets from the current directory. If the file isn't found a empty HTML page is generated whichs references the corresponding javascript file. If `/webpack-dev-server/` is prefixed to the path, it serves a small runtime which connects to the server with socket.io and automatically updates the page.

The webpack-dev-server has a CLI and a node.js API.

## CLI

``` sh
$ webpack-dev-server <entry>
```

All [[CLI]] options are valid for the dev-server too, but there is no `<output>` default argument. For the [[CLI]] is a `webpack.config.js` accepted too.

There are some additional options:

* `--content-base <file/directory/url>`: Base path for the content
* `--quiet`: Don't output anything to the console
* `--no-info`: Suppress boring information
* `--port <number>`

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
	quite: false,
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
* open `http://localhost:8090/webpack-dev-server/index.html`