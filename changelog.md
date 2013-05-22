# changelog

# 0.10 (beta)

* node 0.10 support
* whole chunks can now be cached
* added `--devtool source-map` and `--devtool inline-source-map`
* added option `--optimize-occurence-order`

Small changes:

* assets are only emitted if they changed
* added `--profile`
* added `--prefetch` [experimental]
* added `BannerPlugin`
* added `[chunkhash]` [experimental]
* added `hashDigestLength`
* increased filesystem caching to 60s
* purge only changed files in watch mode
* purge all files on compiling in not-watch mode
* in-memory filesystem now supports windows-like paths too

# 0.9