webpack is a module system / module bundler. 

It bundles preprocess and bundles files (modules) and generate output files (bundle).

The best features are:

### extendability

Almost everything can be added by plugins. In fact the most core features are added through internal plugins.

Loaders define proprocessors for files. So any file type can be processed by the bundler.

### support

A clever parser does static analysic on javascript to find relevant code fragments.

It has a good support of common module formats (and weird variations). By default CommonJS and AMD is supported. More formats can be added by plugins.

### performance

Complete system is designed asynchronous.

Multiple caching levels increase preformance for incremental compilations.

### request size

Various optimizations reduce output size: minimizing, paths are mangled to ids, deduplication.

Hashes can be added to the output files. This allows to cache files effectivly, which reduces request count.

It can generate multiple output bundles which can be loaded on demand. This reduces the initial request size.