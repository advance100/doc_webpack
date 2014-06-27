WIP

* `entry` configuration

``` javascript
{
	entry: {
		a: "./a",
		b: "./b",
		c: ["./c", "./d"]
	},
	output: {
		path: path.join(__dirname, "dist"),
		filename: "[name].entry.js"
	}
}
```

Examples:

* [multiple-entry-points](https://github.com/webpack/webpack/tree/master/examples/multiple-entry-points)
* [multi-part-library](https://github.com/webpack/webpack/tree/master/examples/multi-part-library)
* [multiple-commons-chunks](https://github.com/webpack/webpack/tree/master/examples/multiple-commons-chunks)