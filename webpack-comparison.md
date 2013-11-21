| Feature | webpack/webpack | jrburke/requirejs | substck/node-browserify |
|---------|-----------------|-------------------|-------------------------|
| CommonJs `require` | yes | only wrapping in `define` | yes |
| CommonJs `require.resolve` | yes | no | no |
| AMD `define` | yes | yes | [deamdify](https://github.com/jaredhanson/deamdify) |
| AMD `require` | yes | yes | no |
| AMD `require` loads on demand | yes | with manual configuration | no |
| generate a single bundle | yes | yes* | yes |
| load each file separate | no | no* | no |
| multiple bundles | yes | with manual configuration | with manual configuration |
| additional chunks are loaded on demand | yes | yes | no |
| multi pages build with common bundle | with manual configuration | yes | with manual configuration |
| concat in require `require("./fi" + "le")` | yes | no* | no |
| indirect require `var r = require; r("./file")` | warning (include too much) | no* | no |
| expressions in require (guided) `require("./templates/" + template)` | yes (all files matching included) | no* | no |
| expressions in require (free) `require(moduleName)` | with manual configuration | no* | no |
| requirable files | filessystem | web | filesystem |
| plugins | yes | yes* | yes |
| preprocessing | loaders, [transforms](https://github.com/webpack/transform-loader) | loaders | transforms |
| watch mode | yes | not required | yes |
| debugging support | SourceUrl, SourceMaps | not required | SourceMaps |
| node.js buildin libs `require("path")` | yes | no | yes |
| other node.js stuff | process, __dir/filename, global | - | process, __dir/filename, global |
| replacement for browser | `web_modules`, `.web.js`, package.json field, alias config option | alias option | package.json field, alias option
| minimizing | uglify | uglify, closure compiler | no |
| mangle path names | yes | no | partial |
| Runtime overhead | 243b + 20b per module + 4b per dependency | 14.7kb + 0b per module + (3b + X) per dependency | 415b + 25b per module + (6b + 2X) per dependency |

* in production mode (opposite in development mode)
X is the length of the path string