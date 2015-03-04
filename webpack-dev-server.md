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

Using this config `webpack-dev-server` will serve the static files in your `build` folder and watch your source files for changes. When changes are made the bundle will be recompiled. This modified bundle is served from memory at the relative path specified in `publicPath` (see [API](#API)). It will not be written to your configured output directory. Where a bundle already exists at the same url path the bundle in memory will take precedence.
 
To load your bundled files, you will need to create an `index.html` file. e.g.

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

Now run:

```sh
$ webpack-dev-server --content-base build/ --hot
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

```sh
$ webpack-dev-server --content-base build/ --hot
```


## CLI

``` sh
$ webpack-dev-server <entry>
```

All [[CLI]] options are valid for the dev-server too, but there is no `<output>` default argument. For the [[CLI]] a `webpack.config.js` is accepted too.

There are some additional options:

* `--content-base <file/directory/url>`: Base path for the content
* `--quiet`: Don't output anything to the console
* `--colors`: Add some colors to the output
* `--no-info`: Suppress boring information
* `--host <hostname/ip>`
* `--port <number>`
* `--inline`: embed the webpack-dev-server runtime into the bundle
* `--hot`: adds the `HotModuleReplacementPlugin` and switch the server to hot mode. (Note: make sure you don't add `HotModuleReplacementPlugin` twice)
* `--hot --inline` also adds the `webpack/hot/dev-server` entry.
* `--https`: serves dev-server over HTTPS Protocol. Includes a self-signed certificate that is used when serving the requests.

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
  // Note: this does _not_ add the `HotModuleReplacementPlugin` like the CLI option does. 
  
  // webpack-dev-middleware options
  quiet: false,
  noInfo: false,
  lazy: true,
  filename: "bundle.js",
  watchDelay: 300,
  publicPath: "/assets/",
  headers: { "X-Custom-Header": "yes" },
  stats: { colors: true },

  // set this as true if you want to access dev server from arbitrary url
  // this is handy if you are using a html5 router
  historyApiFallback: false,
});
server.listen(8080, "localhost", function() {});
```



See [[webpack-dev-middleware]] for documentation on middleware options.

## Combining with an existing server

There are cases when you want to run the a backend server or a mock of it in development. You should **not** abuse the webpack-dev-server as backend. It's only intended to serve static (webpacked) assets.

You should run two server side-by-side: The webpack-dev-server and your backend server.

In this case you need to teach the webpack-generated assets to make requests to the webpack-dev-server even when running on a HTML-page sent by the backend server. On the other side you need to teach your backend server to generate HTML pages that include script tags that point to assets on the webpack-dev-server. In addition to that you need a connection between the webpack-dev-server and the webpack-dev-server runtime to trigger reloads on recompilation.

To teach webpack to make requests (for chunk loading or HMR) to the webpack-dev-server you need to provide **a full URL in the `output.publicPath`** option.

To make a connection between webpack-dev-server and its runtime best use the inline mode with `--inline`. The webpack-dev-server CLI automatically includes an entry point which establishs a WebSocket connection. (You can also use the iframe mode if you point `--content-base` of the webpack-dev-server to your backend server.

When you use the inline mode just open the backend server URL in your web browsers. (If you use the iframe mode open the `/webpack-dev-server/` prefixed URL of the webpack-dev-server.)

Summary and example:

* webpack-dev-server on port 9090.
* backend server on port 8080.
* generate HTML pages with `<script src="http://localhost:9090/assets/bundle.js">`
* webpack configuration `output.publicPath = "http://localhost:9090/assets/"`
* inline mode
  * `--inline`
  * open `http://localhost:8080`
* or iframe mode
  * webpack-dev-server `contentBase = "http://localhost:8080/"` (`--content-base`)
  * open `http://localhost:9090/webpack-dev-server/`