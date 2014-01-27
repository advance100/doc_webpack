TODO

## Loaders should

#### do only a single task

This means they should not convert to javascript if not necessary.

Example: Render HTML from a template file by applying the query parameters

I could write a loader that compiles the template from source, execute it and return a module that exports a string containing the HTML code. This is bad.

Instead I should write loaders for every task in this usecase and apply them all (pipeline):

jade-loader: Convert template to a module that exports a function.
apply-loader: Takes a function exporting module and returns raw result.
html-loader: Takes HTML and exports a string exporting module.