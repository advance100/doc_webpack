Welcome to the **Getting Started** of webpack.

This is a wiki page, which isn't finished/polished. If you have read it **please** do enhance some parts of it. With your help a good guide can be created...

> Missing stuff:
> * Explanation

## Installing webpack

You need to have node.js installed.

``` text
npm install webpack -g
```

> This makes the `webpack` command available.

## Setup the compilation

Start with a empty directory.

Create a file `entry.js` with this content:

``` javascript
document.write("It works.");
```

And create a file `index.html` with this content:

``` html
<html>
  <head>
    <script type="text/javascript" src="bundle.js" charset="utf-8"></script>
  </head>
  <body></body>
</html>
```

Then run the following:

``` text
webpack ./entry.js bundle.js
```

It will compile your file and create a bundle file.

If successful it displays something like this:

``` text
Hash: 2337c86f7be0011cdd4815c5073bed80
Time: 27ms
    Asset  Size  Chunks  Chunk Names
bundle.js   982       0  main
   [0] ./entry.js 28 [built] {0}
```

Open `index.js` in your browser. It should display `It works.`

> webpack creates a javascript file `bundle.js` with your entry file

## The second file

We will move some of code into a extra file.

Create a file `content.js` with this content:

``` javascript
module.exports = "It works from content.js.";
```

Then modify `entry.js` to this content:

``` javascript
document.write(require("./content.js"));
```

And recompile with `webpack ./entry.js bundle.js`.

Update your browser window and you should see the text `It works from content.js.`.

> webpack will analyze your entry file for dependencies to other files. These files (called modules) are included in your `bundle.js` too. webpack give each module a unique id and save all modules accessible by id in the `bundle.js` file. Only the entry module is executed on startup. A small runtime provides the `require` function and execute the dependencies when required.

## The first loader

We want to add a css file to out application.

webpack can only handle js natively, so we need the `css-loader` to process css files. We also need to `style-loader` to apply the styles in the css file.

Create a empty `node_modules` folder.

Run `npm install css-loader style-loader` to install the loaders.

Create a file `style.css` with this content:

``` css
body {
  background: yellow;
}
```

Then modify `entry.js` to this content:

``` javascript
require("!style!css!./style.css");
document.write(require("./content.js"));
```

Recompile and update your browser to see your application with yellow background.

> By prefixing loaders to a module request, the module went through a loader pipeline. These loader transform the file content in specific ways. After these transformations are applied, the result is a javascript module.

## Binding loaders

We don't want to write such long requires `require("!style!css!./style.css");`.

We can bind file extensions to loaders so we just need to write in the `entry.js`:

``` javascript
require("./style.css");
document.write(require("./content.js"));
```

Run the compilation with:

``` text
webpack ./entry.js bundle.js --module-bind css=style!css
```

You should see the same result.

## A config file

We want to move the config options into a config file.

Create a file `webpack.config.js` with this content:

``` javascript
module.exports = {
  entry: "./entry.js",
  output: {
    filename: "bundle.js"
  },
  module: {
    loaders: [
      { test: /\.css$/, loader: "style!css" }
    ]
  }
}
```

Now we can just run: 

``` text
webpack
```

to compile.

> The webpack commandline try to load the file `webpack.config.js` in the current directory.

## A more pretty output

If the project grows the compilation may take a bit longer. So we want to display some kind of progress bar. And we want colors...

We can archive this with

``` text
webpack --progress --colors
```

## Watch mode

We don't want to manually recompile after every change...

``` text
webpack --progress --colors --watch
```

Webpack can cache unchanged modules between compilations. Just add `--cache` or insert it into your config file: 

``` javascript
module.exports = {
  cache: true,
  // ...
```

> When using watch mode, webpack installs file watchers to all files, which were used in the compilation process. If any change is detected, it'll run the compilation again. When caching is enabled, webpack keeps each module in memory and will reuse it if it isn't changed.

## Development server

Even better it is with the development server.

``` text
npm install webpack-dev-server -g
```

``` text
webpack-dev-server --progress --colors
```

It binds a small express server on [http://localhost:8080](http://localhost:8080) which hosts a content page and the bundle. It automatically updates the browser page when a bundle is recompiled (socket.io).

> The dev server uses webpack's watch mode. It also prevents webpack from emitting the result files to disk. Instead it keep and serve result files from memory.

## Using modules

TODO

> `require("module"); require("module/file");`