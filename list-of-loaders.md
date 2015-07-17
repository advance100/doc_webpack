## basic

* [`json`](https://github.com/webpack/json-loader): Loads file as JSON
* [`hson`](https://github.com/kentcdodds/hson-loader): Loads HanSON file (JSON for Humans) as JSON object
* [`raw`](https://github.com/webpack/raw-loader): Loads raw content of a file (as utf-8)
* [`val`](https://github.com/webpack/val-loader): Executes code as module and consider exports as JavaScript code
* ['to-string'](https://github.com/gajus/to-string): Executes code as a module, casts output to a string and exports it
* [`imports`](https://github.com/webpack/imports-loader): Imports stuff to the module
* [`exports`](https://github.com/webpack/exports-loader): Exports stuff from the module
* [`expose`](https://github.com/webpack/expose-loader): Expose exports from a module to the global context
* [`script`](https://github.com/webpack/script-loader): Executes a JavaScript file once in global context (like in script tag), requires are not parsed.
* [`legacy`](https://github.com/peerigon/legacy-loader): Prevents scripts from extending the window object
* [`source-map`](https://github.com/webpack/source-map-loader): Extract `sourceMappingURL` comments from modules and offer it to webpack
* [`checksum`](https://github.com/naturalatlas/checksum-loader): Computes the checksum of a file
* [`null`](https://github.com/webpack/null-loader): Emits an empty module.
* [`cowsay`](https://github.com/nelix/cowsay-loader): Emits a module with a cowsay header.
* [`dsv`](https://github.com/wbkd/dsv-loader): Loads csv/tsv files.
* [`glsl`](https://github.com/makio64/shader-loader): Loads glsl files and support glsl-chunks.
* [`render-placement`](https://github.com/zackify/render-placement-loader): Adds React.render to your component for you (not very practical in most cases)
* [`xml`](https://github.com/gisikw/xml-loader): Loads XML as JSON.

## packaging

* [`file`](https://github.com/webpack/file-loader): Emits the file into the output folder and returns the (relative) url.
* [`url`](https://github.com/webpack/url-loader): The url loader works like the file loader, but can return a Data Url if the file is smaller than a limit.
* [`worker`](https://github.com/webpack/worker-loader): The worker loader creates a WebWorker for the provided file. The bundling of dependencies of the Worker is transparent.
* [`serviceworker`](https://github.com/markdalgleish/serviceworker-loader): Like the worker loader, but designed for [Service Workers](http://www.w3.org/TR/service-workers).
* [`bundle`](https://github.com/webpack/bundle-loader): Wraps request in a `require.ensure` block (callback)
* [`promise`](https://github.com/gaearon/promise-loader): Wraps request in a `require.ensure` block (promise)
* [`react-proxy`](https://github.com/webpack/react-proxy-loader): Code Splitting for react components.
* [`react-hot`](https://github.com/gaearon/react-hot-loader): Allows to live-edit React components while keeping them mounted and preserving their state.
* [`image`](https://github.com/tcoopman/image-webpack-loader): Compresses your images. Ideal to use together with `file` or `url`.
* [`img`](https://github.com/thetalecrafter/img-loader): Load and compress images with imagemin.
* [`svgo-loader`](https://github.com/pozadi/svgo-loader): Compresses SVG images using [svgo](https://github.com/svg/svgo) library
* [`baggage`](https://github.com/deepsweet/baggage-loader): Automatically require any resources related to the required one
* [`polymer-loader`](https://github.com/JonDum/polymer-loader): Process HTML & CSS with preprocessor of choice and `require()` Web Components like first-class modules.
* [`uglify-loader`](https://github.com/bestander/uglify-loader): Uglify contents of a module. Unlike uglify plugin you can minify with mangling only your application files and not the libraries
* [`html-minify-loader`](https://github.com/bestander/html-minify-loader): Minifies HTML using [minimize](https://github.com/Moveo/minimize)
* [`vue-loader`](https://github.com/vuejs/vue-loader): Load single-file Vue.js components as modules, with loader-support for preprocessors.


## dialects

* [`coffee`](https://github.com/webpack/coffee-loader): Loads coffee-script like JavaScript
* [`coffee-jsx`](https://github.com/jsifalda/coffee-jsx-loader): Loads coffee-script with JSX like JavaScript
* [`coffee-redux`](https://github.com/webpack/coffee-redux-loader): Loads coffee-script like JavaScript
* [`json5`](https://github.com/webpack/json5-loader): Like json, but not so strict.
* [`es6`](https://github.com/shama/es6-loader): Loads ES6 modules. (old)
* [`esnext`](https://github.com/conradz/esnext-loader): Transpile ES6 code using [esnext](https://github.com/esnext/esnext).
* [`babel`](https://github.com/babel/babel-loader): Turn ES6 code into vanilla ES5 using [Babel](https://github.com/babel/babel).
* [`regenerator`](https://github.com/pjeby/regenerator-loader): Use ES6 generators via Facebook's [Regenerator](http://facebook.github.io/regenerator/) module.
* [`livescript`](https://github.com/appedemic/livescript-loader): Loads LiveScript like JavaScript
* [`sweetjs`](https://github.com/jlongster/sweetjs-loader): Use sweetjs macros. 
* [`traceur`](https://github.com/jupl/traceur-loader): Use future JavaScript features with [Traceur](https://github.com/google/traceur-compiler).
* [`ts`](https://github.com/jbrantly/ts-loader): Loads TypeScript like JavaScript.
* [`typescript`](https://github.com/andreypopp/typescript-loader): Loads TypeScript like JavaScript.
* [`typescript-simple-loader`](https://github.com/blakeembrey/typescript-simple-loader): Loads TypeScript with syntactic and semantic errors.
* [`awesome-typescript-loader`](https://github.com/s-panferov/awesome-typescript-loader): Loads TypeScript like JavaScript with watching support. **Works with TypeScript 1.5-alfa**
* [`purs-loader`](https://www.npmjs.com/package/purs-loader): Loads [PureScript](http://www.purescript.org/) like JavaScript.
* [`oj`](https://github.com/DragonsInn/oj-loader): Loads [OJ](https://github.com/musictheory/oj) (an Objective-C like language) files and compiles them to plain JavaScript.
* [`ulmus`](https://github.com/unindented/ulmus-loader): Loads [Elm](http://elm-lang.org/) files and compiles them to plain JavaScript.

## templating

* [`html`](https://github.com/webpack/html-loader): Exports HTML as string, require references to static resources.
* [`riot`](https://github.com/esnunes/riotjs-loader): Load RiotJS tags and convert them to javascript.
* [`jade`](https://github.com/webpack/jade-loader): Loads jade template and returns a function
* [`jade-html`](https://github.com/bline/jade-html-loader): Loads jade template and returns generated HTML
* [`template-html`](https://github.com/jtangelder/template-html-loader): Loads any template with consolidate.js and returns generated HTML
* [`handlebars`](https://github.com/altano/handlebars-loader): Loads handlebars template and returns a function
* [`dust`](https://github.com/avaly/dust-loader): Loads dust template and returns a function
* [`ractive`](https://github.com/rstacruz/ractive-loader): Pre-compiles Ractive templates for interactive DOM manipulation
* [`jsx`](https://github.com/petehunt/jsx-loader): Transform jsx code for [React](http://facebook.github.io/react/) to js code.
* [`react-templates`](https://github.com/AlexanderPavlenko/react-templates-loader): Loads react-template and returns a function
* [`em`](https://github.com/yoshdog/emblem-loader): Compiles [Emblem](http://emblemjs.com/) to Handlebars.js
* [`ejs`](https://github.com/okonet/ejs-loader): Loads EJS ([underscore](http://underscorejs.org/#template)( templating engine) template and returns a pre-compiled function
* [`mustache`](https://github.com/deepsweet/mustache-loader): Pre-compiles Mustache templates with [Hogan.js](https://github.com/twitter/hogan.js) and returns a function
* [`yml`](https://github.com/okonet/yml-loader): Converts YAML to JSON
* [`markdown`](https://github.com/peerigon/markdown-loader): Compiles Markdown to HTML
* [`remarkable`](https://github.com/unindented/remarkable-loader): Compiles Markdown to HTML using the Remarkable parser
* [`markdown-it`](https://github.com/unindented/markdown-it-loader): Compiles Markdown to HTML using the markdown-it parser
* [`ng-cache`](https://github.com/teux/ng-cache-loader): Puts HTML partials in the Angular's $templateCache
* [`ngtemplate`](https://github.com/WearyMonkey/ngtemplate-loader): Bundles your AngularJS templates and Pre-loads the template cache.
* [`hamlc`](https://github.com/ericdfields/hamlc-loader): Compiles haml-coffee templates (.hamlc) and returns a function.
* [`haml`](https://github.com/AlexanderPavlenko/haml-loader): Renders haml-coffee templates (.html.hamlc) and returns a string.
* [`jinja`](https://github.com/pierreant-p/jinja-loader): Precompiles nunjucks and jinja2 templates
* [`soy`](https://github.com/bendman/soy-loader): Compiles Google Closure templates and returns the namespace with render functions
* [`smarty`](https://github.com/zhiyan/smarty-loader): Pre-compiles php smarty templates and returns a function

## styling
* [`bootstrap-sass`](https://github.com/justin808/bootstrap-sass-loader): Loads a configuration file for Twitter Bootstrap integration using Sass. Allows complete customization via Sass.
* [`style`](https://github.com/webpack/style-loader): Add exports of a module as style to DOM
* [`css`](https://github.com/webpack/css-loader): Loads css file with resolved imports and returns css code
* [`less`](https://github.com/webpack/less-loader): Loads and compiles a less file
* [`sass`](https://github.com/jtangelder/sass-loader): Loads and compiles a scss file
* [`stylus`](https://github.com/shama/stylus-loader): Loads and compiles a stylus file
* [`rework`](https://github.com/okonet/rework-loader): Post-process CSS with [Rework](https://github.com/reworkcss/rework) and returns CSS code
* [`postcss`](https://github.com/postcss/postcss-loader): Post-process CSS with Autoprefixer and [other PostCSS plugins](https://github.com/postcss/postcss#built-with-postcss)
* [`autoprefixer`](https://github.com/passy/autoprefixer-loader): Add vendor prefixes to CSS rules using values from Can I Use
* [`namespace-css`](https://github.com/jeffling/namespace-css-loader): Namespace your css with a given selector (for encapsulating all rules in one subset of your site)
* [`fontgen`](https://www.npmjs.com/package/fontgen-loader): Create your own webfont with proper CSS on-the-fly and include it into WebPack.

## translation

* [`po`](https://github.com/perchlayer/po-loader): Loads a PO gettext file and returns JSON
* [`format-message`](https://github.com/thetalecrafter/format-message-loader): Compiles translations to ICU Message Format strings in [`formatMessage`](https://github.com/thetalecrafter/format-message) calls
* [`jsxlate-loader`](https://github.com/drd/jsxlate-loader): Transform React source code for use with [`jsxlate`](https://github.com/drd/jsxlate)

## support

* [`mocha`](https://github.com/webpack/mocha-loader): do tests with mocha in browser or node.js
* [`coverjs`](https://github.com/webpack/coverjs-loader): PostLoader to code coverage with CoverJs
* [`istanbul-instrumenter`](https://github.com/deepsweet/istanbul-instrumenter-loader): [Istanbul](https://github.com/gotwarlost/istanbul) postLoader to code coverage with [karma-webpack](https://github.com/webpack/karma-webpack) and [karma-coverage](https://github.com/karma-runner/karma-coverage)
* [`isparta-instrumenter`](https://github.com/ColCh/isparta-instrumenter-loader): [Isparta](https://github.com/douglasduteil/isparta) preLoader to code coverage with [karma-webpack](https://github.com/webpack/karma-webpack) and [douglasduteil/karma-coverage#next](https://github.com/douglasduteil/karma-coverage)
* [`ibrik-instrumenter`](https://github.com/vectart/ibrik-instrumenter-loader): [Ibrik](https://github.com/Constellation/ibrik) preLoader to CoffeeScript code coverage with [karma-webpack](https://github.com/webpack/karma-webpack) and [douglasduteil/karma-coverage](https://github.com/douglasduteil/karma-coverage)
* [`eslint`](https://github.com/MoOx/eslint-loader): PreLoader for linting code using ESLint.
* [`jshint`](https://github.com/webpack/jshint-loader): PreLoader for linting code.
* [`jscs`](https://github.com/unindented/jscs-loader): PreLoader for style checking.
* [`injectable`](https://github.com/jauco/webpack-injectable): Allow to inject dependencies into modules
* [`transform`](https://github.com/webpack/transform-loader): Use browserify transforms as loader.
* [`image-size`](https://github.com/patcoll/image-size-loader): Loads an image and returns its dimensions and type
* [`csslint`](https://github.com/hyungjs/csslint-loader): PreLoader for linting code using CSSLint
* [`coffeelint-loader`](https://github.com/bline/coffeelint-loader): PreLoader for linting [CoffeeScript](http://coffeescript.org/).
* [`tslint-loader`](https://github.com/wbuchwalter/tslint-loader): PreLoader for linting TypeScript using [TSLint](https://github.com/palantir/tslint)
* [`parker`](https://github.com/tanem/parker-loader): Output a stylesheet analysis report using [parker](https://github.com/katiefenn/parker).
* [`sjsp`](https://github.com/3100/sjsp-loader): Inject some codes for profiling using [node-sjsp](https://github.com/45deg/node-sjsp).