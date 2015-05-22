| Feature | webpack/webpack | jrburke/requirejs | substack/node-browserify | jspm/jspm-cli |
|---------|-----------------|-------------------|--------------------------|---------------|
| CommonJs `require` | **yes** | only wrapping in `define` | **yes** | yes |
| CommonJs `require.resolve` | **yes** | no | no | no |
| CommonJs `exports` | **yes** | only wrapping in `define` | **yes** | yes |
| AMD `define` | **yes** | **yes** | [deamdify](https://github.com/jaredhanson/deamdify) | yes |
| AMD `require` | **yes** | **yes** | no | yes |
| AMD `require` loads on demand | **yes** | with manual configuration | no | yes |
| generate a single bundle | **yes** | yes♦ | yes | yes |
| load each file separate | no | yes | no | yes |
| multiple bundles | **yes** | with manual configuration | with manual configuration | yes |
| additional chunks are loaded on demand | **yes** | **yes** | no | with bundle arithmetic |
| multi pages build with common bundle | with manual configuration | **yes** | with manual configuration | with bundle arithmetic |
| concat in require `require("./fi" + "le")` | **yes** | no♦ | no | no |
| indirect require `var r = require; r("./file")` | **yes** | no♦ | no | no |
| expressions in require (guided) `require("./templates/" + template)` | **yes (all files matching included)** | no♦ | no | no |
| expressions in require (free) `require(moduleName)` | with manual configuration | no♦ | no | no |
| requirable files | file system | **web** | file system | through plugins |
| plugins | **yes** | yes | **yes** | yes |
| preprocessing | **loaders, [transforms](https://github.com/webpack/transform-loader)** | loaders | transforms | plugin translate |
| watch mode | yes | not required | yes | not needed in dev |
| debugging support | **SourceUrl, SourceMaps** | not required | SourceMaps | SourceUrl, SourceMaps |
| node.js built-in libs `require("path")` | **yes** | no | **yes** | **yes** |
| other node.js stuff | process, __dir/filename, global | - | process, __dir/filename, global | process, __dir/filename, global for cjs |
| replacement for browser | `web_modules`, `.web.js`, package.json field, alias config option | alias option | package.json field, alias option | package.json, alias option |
| minimizing | uglify | uglify, closure compiler | [uglifyify](https://github.com/hughsk/uglifyify) | yes |
| mangle path names | **yes** | no | partial | no |
| Runtime overhead | **243b + 20b per module + 4b per dependency** | 14.7kb + 0b per module + (3b + X) per dependency | 415b + 25b per module + (6b + 2X) per dependency | 5.5KB for self-executing bundles, 38KB for full loader and polyfill, 0 plain modules, 293b CJS, 139b ES6 System.register before gzip |

♦ in production mode (opposite in development mode)

X is the length of the path string




