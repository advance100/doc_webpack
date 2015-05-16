The _webpack-dev-server_ is a little node.js [Express](http://expressjs.com/) server, which uses the _[[webpack-dev-middleware]]_ to serve a _webpack bundle_. It also has a little runtime which is connected to the server via [Socket.IO](http://socket.io/). The server emits information about the compilation state to the client, which reacts to those events. You can choose between different modes, depending on your needs. So lets say you have the following config file:

```javascript
module.exports = {
  entry: {
    app: ['./app/main.js']
  },
  output: {
    path: './build',
    publicPath: "/assets/",
    filename: 'bundle.js'
  }
};
```

You have an `app` folder with your initial entry point that _webpack_ will bundle into a `bundle.js` file in the `build` folder.

## Inline mode
The _webpack-dev-server_ will serve the files in the current directory, unless you configure a specific content base. 

```sh
$ webpack-dev-server --content-base build/
```

Using this config _webpack-dev-server_ will serve the static files in your `build` folder and watch your source files for changes. When changes are made the _bundle_ will be recompiled. This modified _bundle_ is served from memory at the relative path specified in `publicPath` (see [API](#API)). It will not be written to your configured output directory. Where a _bundle_ already exists at the same url path the _bundle_ in memory will take precedence.

For example with the configuration above your _bundle_ will be available at `localhost:8080/assets/bundle.js`
 
To load your bundled files, you will need to create an `index.html` file. e.g:

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
By adding a script to your `index.html` file and a special entry point in your configuration you will be able to get live reloads when doing changes to your files. Change your `index.html` file to this:

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
When running _webpack-dev-server_ you can also head to `localhost:8080/webpack-dev-server/`. Going to this URL will not only load your application, but also indicate when a rebundling is in progress. Using this url does not require you to insert the `webpack-dev-server` script. So using `localhost:8080/webpack-dev-server/` with the current setup would require this `index.html`:

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


## webpack-dev-server CLI

``` sh
$ webpack-dev-server <entry>
```

All _webpack_ [[CLI|cli]] options are valid for the _webpack-dev-server_ CLI too, but there is no `<output>` default argument. For the _webpack-dev-server_ CLI a `webpack.config.js` (or the file passed by the `--config` option) is accepted as well.

There are some additional options:

* `--content-base <file/directory/url>`: base path for the content;
* `--quiet`: don't output anything to the console;
* `--colors`: add some colors to the output;
* `--no-info`: suppress boring information;
* `--host <hostname/ip>`: hostname or IP;
* `--port <number>`: port;
* `--inline`: embed the _webpack-dev-server_ runtime into the _bundle_;
* `--hot`: adds the `HotModuleReplacementPlugin` and switch the server to _hot mode_. Note: make sure you don't add `HotModuleReplacementPlugin` twice;
* `--hot --inline` also adds the `webpack/hot/dev-server` entry;
* `--https`: serves _webpack-dev-server_ over HTTPS Protocol. Includes a self-signed certificate that is used when serving the requests.

The additional options above can be set in `devServer` option in `webpack.config.js`. For example:

```javascript
module.exports = {
    // ... webpack.config.js stuff ...
    devServer: {
        contentBase: "./build",
        info: false, //  --no-info option
        hot: true,
        inline: true
    }
}
```

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

  // Set this as true if you want to access dev server from arbitrary url.
  // This is handy if you are using a html5 router.
  historyApiFallback: false,

  // Set this if you want webpack-dev-server to delegate a single path to an arbitrary server.
  // Use "*" to proxy all paths to the specified server.
  // This is useful if you want to get rid of 'http://localhost:8080/' in script[src],
  // and has many other use cases (see https://github.com/webpack/webpack-dev-server/pull/127 ).
  proxy: {
    "*": "http://localhost:9090"
  }
});
server.listen(8080, "localhost", function() {});
```


See _[[webpack-dev-middleware]]_ for documentation on middleware options.

Notice that _webpack configuration_ is not passed to `WebpackDevServer` API, thus `devServer` option in webpack configuration is not used in this case. Also, there is no _inline mode_ for `WebpackDevServer` API. `<script src="http://localhost:8080/webpack-dev-server.js"></script>` should be inserted to HTML page manually.

## Combining with an existing server

You may want to run a backend server or a mock of it in development. You should **not** use the _webpack-dev-server_ as a backend. Its only purpose is to serve static (webpacked) assets.

You can run two servers side-by-side: The _webpack-dev-server_ and your backend server.

In this case you need to teach the webpack-generated assets to make requests to the _webpack-dev-server_ even when running on a HTML-page sent by the backend server. On the other side you need to teach your backend server to generate HTML pages that include `script` tags that point to assets on the _webpack-dev-server_. In addition to that you need a connection between the _webpack-dev-server_ and the _webpack-dev-server_ runtime to trigger reloads on recompilation.

To teach _webpack_ to make requests (for chunk loading or HMR) to the _webpack-dev-server_ you need to provide **a full URL in the `output.publicPath`** option.

To make a connection between _webpack-dev-server_ and its runtime best use the _inline mode_ with `--inline`. The _webpack-dev-server_ CLI automatically includes an entry point which establishs a WebSocet connection. (You can also use the _iframe_ mode if you point `--content-base` of the _webpack-dev-server_ to your backend server.

When you use the _inline mode_ just open the backend server URL in your web browsers. (If you use the _iframe mode_ open the `/webpack-dev-server/` prefixed URL of the _webpack-dev-server_.)

Summary and example:

* _webpack-dev-server_ on port `9090`;
* backend server on port `8080`;
* generate HTML pages with `<script src="http://localhost:9090/assets/bundle.js">`;
* webpack configuration with `output.publicPath = "http://localhost:9090/assets/"`;
* _inline mode_:
  * `--inline`;
  * open `http://localhost:8080`.
* or _iframe mode_:
  * _webpack-dev-server_ `contentBase = "http://localhost:8080/"` (`--content-base`);
  * open `http://localhost:9090/webpack-dev-server/`.