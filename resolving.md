The resolving process have three entry points. Depending on the shape for the request it enters at one of these. 

* resolve an **absolute path** (shape: `/home/xyz` or `C:\Home\xyz`)
* resolve a **relative path** (shape: `./xyz` or `../xyz`)
* resolve a **module path** (shape: `xyz` or `xyz/abc`)

> Even while this description suggest a serial process, the process is running completly async and parallel.

## absolute path

``` text
path is a directory:
  directory contains a package description file (package.json):
    read the main file from the package description file and continue with this path.
  directory doesn't contain a package description file:
    continue with the "index" path,
try the append all possible extensions until one file exists
```

## relative path

``` text
resolve the relative path relative to the current directory
continue with the "absolute path" entry
```

## module path

``` text
find all possible paths where the module can be in
  1. root
  2. moduleDirectories starting in the current directory
  3. fallback
for each possible path:
  prepend the path to the request
  try to resolve the file with the "absolute path" entry
```

### alias

``` text
before trying to resolve a "module path":
  if the module name matches an alias:
    if the value is an module name: continue with this module name
    if the value resolves to an directory:
      concat the remaining path with the value
      continue with "relative path" or "absolute path" entry
    if the value resolves to an file and there is no remaining path
      continue with "relative path" or "absolute path" entry
```

## unsafeCache

``` text
before resolving anything:
  if request matches option value:
    lookup the directory request pair in the cache
after resolving anything:
  if request matches option value:
    store the directory request pair in the cache
```

## loaders

``` text
when trying to resolve a "module path":
  apply all moduleTemplates to the module name
  try the resolve all possible module names using the "module path" entry
```