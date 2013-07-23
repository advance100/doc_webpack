# optimizing

## Minimize

To minimize your scripts (and your css, if you use the css-loader) webpack supports a simple option:

`--optimize-minimize` resp. `optimize: { minimize: true }`

That's a pretty simply but effective way to optimize your web app.

As you already know (if you've read the remaining docs) webpack give your modules and chunks ids to identify them. Webpack can vary the distribution of the ids to get the smallest id length for often used ids with a simple option:

`--optimize-occurence-order resp. `optimize: { occurenceOrder: true }`

The entry chunks have higher priority for file size.

## Deduplication

If you use some libraries with cool dependency trees, it may occur that some files are identical. Webpack can find these files and deduplicate them. This prevent to include duplicate code into your bundle and instead copy the function at runtime. It doesn't affect semantics. You can enable it with:

`--optimize-dedupe` resp. `optimize: { dedupe: true }`

The feature add some overhead to the entry chunk.

## Caching

To effectivly cache your files, they should have a hash or version in their url. You can emit or move the output files manually in a folder called `v1.3`. But this has several disadvantages: Extra work for the developer and not changed files are not loaded from cache.

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

## Chunks

While writing your code you have already added many code split points to load stuff on demand. After compiling you may notice that there are too many of them and your chunks are so small that the HTTP overhead is big. That's no problem. Webpack can postprocess your chunks by merging them cleverly. You can provide two options:

* Limit the maximum chunk count with `--optimize-max-chunks 15` `optimize: { maxChunks: 15 }`
* Limit the minimum chunk size with `--optimize-min-chunk-size 10000` `optimize: { minChunkSize: 10000 }`

Webpack will take care of it by merging chunks (It will prefer merging chunk which have duplicate modules). Nothing will be merged into the entry chunk to not impact initial page loading time.

## Single-Page-App

A Single-Page-App is the web app type webpack is designed and optimized for.

You may have splitted the app into multiple chunks, which are loaded at your router. The entry chunk only contains the router and some libraries, but no content. This works great while your user is navigating through your app, but for initial page load you need 2 round trips: One for the router and one for the current content page.

If you use the HTML5 History API to reflect the current content page in the URL, your server can know which content page will be requested by the client code. To save roud trips the server can include the content chunk in the response: This is possible by just adding it as script tag. The browser will load both chunks parallel.

``` html
<script src="entry-chunk.js" type="text/javascript" charset="utf-8"></script>
<script src="3.chunk.js" type="text/javascript" charset="utf-8"></script>
```

You can extract the chunk filename from the stats.

## Multi-Page-App

When you compile a (real) multi page app, you want to share common code between the pages. In fact this is really easy with webpack: Just compile with multiple entry points:

`webpack p1=./page1 p2=./page2 p3=./page3 [name].entry-chunk.js`

``` javascript
module.exports = {
  entry: {
    p1: "./page1",
    p2: "./page2",
    p3: "./page3"
  },
  output: {
    filename: "[name].entry.chunk.js"
  }
}
```

This will generate multiple entry chunks: `p1.entry.chunk.js`, `p2.entry.chunk.js` and `p3.entry.chunk.js`. But additional chunks can be shared by them.