| Feature | webpack/webpack | jrburke/requirejs | substack/node-browserify | jspm/jspm-cli |
|---------|-----------------|-------------------|--------------------------|---------------|
| CommonJs `require` | **yes** | only wrapping in `define` | **yes** | yes |
| CommonJs `require.resolve` | **yes** | no | no | no |
| CommonJs `exports` | **yes** | only wrapping in `define` | **yes** | yes |
| AMD `define` | **yes** | **yes** | [deamdify](https://github.com/jaredhanson/deamdify) | yes |
| AMD `require` | **yes** | **yes** | no | yes |
| AMD `require` loads on demand | **yes** | with manual configuration | no | yes |
| Generate a single bundle | **yes** | yes♦ | yes | yes |
| Load each file separate | no | yes | no | yes |
| Multiple bundles | **yes** | with manual configuration | with manual configuration | yes |
| Additional chunks are loaded on demand | **yes** | **yes** | no | [System.import](https://github.com/systemjs/systemjs/blob/master/docs/system-api.md#systemimportmodulename--normalizedparentname---promisemodule) |
| Multi pages build with common bundle | with manual configuration | **yes** | with manual configuration | with bundle arithmetic |
| Concat in require `require("./fi" + "le")` | **yes** | no♦ | no | no |
| Indirect require `var r = require; r("./file")` | **yes** | no♦ | no | no |
| Expressions in require (guided) `require("./templates/" + template)` | **yes (all files matching included)** | no♦ | no | no |
| Expressions in require (free) `require(moduleName)` | with manual configuration | no♦ | no | no |
| Requirable files | file system | **web** | file system | through plugins |
| Plugins | **yes** | yes | **yes** | yes |
| Preprocessing | **loaders, [transforms](https://github.com/webpack/transform-loader)** | loaders | transforms | plugin translate |
| Watch mode | yes | not required | yes | not needed in dev |
| Debugging support | **SourceUrl, SourceMaps** | not required | SourceMaps | SourceUrl, SourceMaps |
| Node.js built-in libs `require("path")` | **yes** | no | **yes** | **yes** |
| Other Node.js stuff | process, __dir/filename, global | - | process, __dir/filename, global | process, __dir/filename, global for cjs |
| Replacement for browser | `web_modules`, `.web.js`, package.json field, alias config option | alias option | package.json field, alias option | package.json, alias option |
| Minimizing | uglify | uglify, closure compiler | [uglifyify](https://github.com/hughsk/uglifyify) | yes |
| Mangle path names | **yes** | no | partial | yes |
| Runtime overhead | **243B + 20B per module + 4B per dependency** | 14.7kB + 0B per module + (3B + X) per dependency | 415B + 25B per module + (6B + 2X) per dependency | 5.5kB for self-executing bundles, 38kB for full loader and polyfill, 0 plain modules, 293B CJS, 139B ES6 System.register before gzip |
| Dependencies | 19MB / 127 packages | 11MB / 118 packages | **1.2MB / 1 package** | 26MB / 131 packages |

♦ in production mode (opposite in development mode)

X is the length of the path string



