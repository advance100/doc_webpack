The webpack-dev-server is a little node.js express server, which uses the [[webpack-dev-middleware]] to serve a webpack bundle. It also has a little runtime which is connected to the server via socket.io. The server emit information about the compilation state to the client, which reacts on that events.

It serves static assets from the current directory. If the file isn't found a empty HTML page is generated whichs references the corresponding javascript file. If `/webpack-dev-server/` is prefixed to the path, it serves a small runtime which connects to the server with socket.io and automatically updates the page.

The webpack-dev-server has a CLI and a node.js API.

All [[webpack CLI options | webpack detailed usage]] and [[webpack API options | webpack options]] are valid for the dev-server too. Read [[webpack-dev-middleware]] for more information. CLI: A `webpack.config.js` is accepted too.

## CLI

There are some additional options:

* `--content-base <file/directory/url>`: Base path for the content
* `--quiet`: Don't output anything to the console
* `--no-info`: Suppress boring information
* `--port <number>`

## API

``` javascript
var WebpackDevServer = require("webpack-dev-server");
var webpack = require("webpack");

var server = new Server(webpack({
  // webpack options
}), {
  // webpack-dev-server options
  contentBase: "/path/to/direcotry",
  // or: contentBase: "http://localhost/",
  
  // webpack-dev-middleware options
  quite: false,
  noInfo: false,
  lazy: true,
  watchDelay: 300,
  publicPath: "/assets/",
  headers: { "X-Custom-Header": "yes" },
  stats: { colors: true }
}
```

See [[webpack-dev-middleware]] for documentation on middleware options.