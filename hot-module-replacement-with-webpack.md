To use hot code replacement with webpack you need to four things:

* records (`--records-path`, `recordsPath: ...`)
* global enable hot code replacement (`--hot`, `hot: true`)
* hot replacement code in your code `module.hot.accept`
* hot replacement management code in your code `module.hot.check`, `module.hot.apply`

A small testcase:

``` css
/* style.css */
body {
  background: red;
}
```

``` javascript
/* entry.js */
require("./style.css");
document.write("<input type='text' />");
```

That's enough to use hot code replacement with the dev-server.

``` sh
npm install webpack webpack-dev-server -g
npm install webpack css-loader style-loader
webpack-dev-server webpack/hot/dev-server ./entry --hot --module-bind "css=style!css"
```

The dev server provides in memory records, which is good for development.

There is special management code for the dev-server at `webpack/hot/dev-server`.

The `style-loader` already includes hot replacement code.

If you visit [http://localhost:8080](http://localhost:8080) you should see the page with a red background and a input box. Type some text into the input box and edit `style.css` to have another background color. 

Voilà... The background updates but without full page refresh. Text and selection in the input box should stay.

Read more about how to write you own hot replacement (management) code: [[hot code replacement]]

Check the [example-app](http://webpack.github.io/example-app/) for a demo without coding