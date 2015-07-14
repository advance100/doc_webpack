## Built-in plugins

Plugins are included in your module by using the plugins property in the webpack config.

``` javascript
// webpack should be in the node_modules directory, install if not.
var webpack = require("webpack");

module.exports = {
	plugins: [
		new webpack.ResolverPlugin([
			new webpack.ResolverPlugin.DirectoryDescriptionFilePlugin("bower.json", ["main"])
		], ["normal", "loader"])
	]
};
```

## Other plugins

Plugins that are not built-in may be installed via npm if published there, or by other means if not:

``` text
npm install component-webpack-plugin
```

which can then be used as follows:

``` javascript
var ComponentPlugin = require("component-webpack-plugin");
module.exports = {
	plugins: [
		new ComponentPlugin()
	]
}
```

## See also

* [[list of plugins]]