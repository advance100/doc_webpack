## `npm link`ed modules doesn't find their dependencies

The node.js module resolving algorithm is pretty simple: module dependencies are looked up in `node_modules` folders in every parent directory of the requiring module. When you `npm link` modules with peer dependencies that are not in your root directory, modules can no longer be found. (You propably want to consider `peerDependencies` with `npm link` as broken by design in node.js.) Note that a dependency to the application (even if this is not the perfect design) is also a kind of peerDependency even if it's not listed as such in the module's `package.json`.

But you can easily workaround that in webpack: Add the `node_modules` folder of your application to the resolve paths. There are two config options for this: `resolve.fallback` and `resolveLoader.fallback`.

Here is a config example:

``` js
module.exports = {
  resolve: { fallback: path.join(__dirname, "node_modules") },
  resolveLoader: { fallback: path.join(__dirname, "node_modules") }
};
```