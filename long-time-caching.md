To effectively cache your files, they should have a hash or version in their url. You can emit or move the output files manually in a folder called `v1.3`. But this has several disadvantages: Extra work for the developer and not changed files are not loaded from cache.

Webpack can add hashes for the files to the filename. Loaders that emit files (worker-loader, file-loader) already do this. For the chunks you have to enable it. There a two levels:

1. Compute a hash of all chunks and add it.
2. Compute a hash per chunk and add it.

Option 1 is enabled by adding `[hash]` to the filename config options

`webpack ./entry output.[hash].bundle.js`

```
{
  output: {
    path: path.join(__dirname, "assets", "[hash]"),
    publicPath: "assets/[hash]/",
    filename: "output.[hash].bundle.js",
    chunkFilename: "[id].[hash].bundle.js"
  }
}
```

Option 2 is enabled by adding `[chunkhash]` to the chunk filename config option

`--output-chunk-file [chunkhash].js`

`output: { chunkFilename: "[chunkhash].bundle.js" }`

Note that you need to reference the entry chunk with it's hash in your HTML. You may want to extract the hash or the filename from the stats.

In combination with Hot Code Replacement your must use option 1, but not on the `publicPath` config option.
