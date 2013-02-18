# hot code replacement

"Hot Code Replacement" (HCR) is an opt-in feature, so you need to put some code at choosen points of your application. The dependencies are handled by the module system.

I. e. you place your hot replacement code in module A. Module A requires module B and B requires C. If module C is updated, and module B cannot handle the update, modules B and C become outdated. Module A can handle the update and new modules B and C are injected.

## Example 1: hot replace request handler of http server

``` javascript
var requestHandler = require("./handler.js");
var server = require("http").createServer();
server.on("request", requestHandler);
server.listen(8080);

// check if HCR is enabled
if(module.hot) {
  // accept update of dependency
  module.hot.accept("./handler.js", function() {
    // replace request handler of server
    server.removeListener("request", requestHandler);
    requestHandler = require("./handler.js");
    server.on("request", requestHandler);
  });
}
```

## Example 2: hot replace css

``` javascript
// addStyleTag(css: string) => HTMLStyleElement
var addStyleTag = require("./addStyleTag");

var element = addStyleTag(".rule { attr: name }");
module.exports = null;

// check if HCR is enabled
if(module.hot) {

  // accept itself
  module.hot.accept();

  // removeStyleTag(element: HTMLStyleElement) => void
  var removeStyleTag = require("./removeStyleTag");

  // dispose handler
  module.hot.dispose(function() {
    // revoke the side effect
    removeStyleTag(element);
  });
}
```

## API

If HCR is enabled for a module `module.hot` is an object containing these properties:

### `accept(dependencies: string[], callback: (updatedDependencies) => void) => void`

Accept code updates for the specified dependencies. The callback is called when dependencies were replaced.

### `accept(dependency: string, callback: () => void) => void`

See above.

### `accept() => void`

Accept code updates for this module without notification of parents. This should only be used if the module doesn't export anything.

### `decline(dependencies: string[]) => void`

Do not accept updates for the specified dependencies. If any dependencies is updated, the code update fails with code `"decline"`.

### `decline(dependency: string) => void`

See above.

### `decline() => void`

Flag the current module as not updateable. If updated the update code would fail with code `"decline"`.

### `dispose/addDisposeHandler(callback: (data: object) => void) => void`

Add a one time handler, which is executed when the current module code is replaced. Here you should destroy/remove any persistant resource you have claimed/created. If you want to transfer state to the new module, add it to `data` object. The `data` will be availible at `module.hot.data` on the new module.

### `removeDisposeHandler(callback: (data: object) => void) => void`

Remove a handler.

This can useful to add a temporary dispose handler. You could i. e. replace code while in the middle of a multi-step async function.


## Management API

Also on the `module.hot` object.

### `setApplyOnUpdate(flag: boolean) => void`

`flag == false` means you have to have to manually call `apply()` if an update is ready.

`flag == true` means that it is called for you.

Default should be `true`.

### `check(callback: (err: Error, outdatedModules: Module[]) => void`

Throws an exceptions if `status()` is not `idle`.

Check all currently loaded modules for updates and apply updates if found.

If no update where found, the callback is called with `null`.

If applyOnUpdate is set the callback will be called with all modules that were disposed.

If applyOnUpdate is not set the callback will be called with all modules that will be disposed on `apply()`.

### `apply(callback: (err: Error, outdatedModules: Module[]) => void`

If `status() != "ready"` it throws an error.

Continue the update process.

### `status() => string`

Return one of `idle`, `check`, `watch`, `watch-delay`, `prepare`, `ready`, `dispose`, `apply`, `abort` or `fail`.

`idle`

The HCR is waiting for your call the `check()`. When you call it the status will change to `check`.

`check`

The HCR is checking for updates. If it doesn't find updates it will change back to `idle`.

If updates where found it will go through the steps `prepare`, `dispose` and `apply`. Than back to `idle`.

`watch`

The HCR is in watch mode and will automatically be notified about changes. After the first change it will change to `watch-delay` and wait for a specified time to start the update process. Any change will reset the timeout, to accumulate more changes. When the update process is started it will go through the steps `prepare`, `dispose` and `apply`. Than back to `watch` or `watch-delay` if changes where detected while updating.

`prepare`

The HCR is prepare stuff for the update. This may means that it's downloading something.

`ready`

An update is availible and prepared. Call `apply()` to continue.

`dispose`

The HCR is calling the dispose handlers of modules that will be replaced.

`apply`

The HCR is calling the accept handlers of the parents of replaced modules, than it requires the self accepted modules.

`abort`

A update cannot apply, but the system is still in a (old) consistent state.

`fail`

A update has throwed an exception in the middle of the process, and the system is (maybe) in a inconsistent state. The system should be restarted.

### `status/addStatusHandler(callback: (status: string) => void) => void`

Register a callback on status change.

### `removeStatusHandler(callbkac: (status: string) => void) => void`

Remove a registered status change handler.


## How to care with ...

### ... a module without side effects (the standard case)

Nothing to do in the module. Any parent can accept it.

### ... a module with side effects

The module needs a dispose handler, than any parent can accept it.

### ... a module with only side effects and no exports

The module needs a dispose handler and can accept itself. No action is required in the parent.

If the module's code is not in your hand, the parent can accept the module with some custom dispose logic.

### ... the application entry module

As it doesn't export it can accept itself. A dispose handler can pass the application state on replacement.

### ... external module with not handleable side effects

In the nearest parent you decline the dependency. This makes your application throw on update. But as it's an external module, an update is very rar.