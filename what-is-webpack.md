**webpack** is a **module bundler**.

This means webpack takes modules with dependencies and emit static assets representing that modules.

![modules with dependencies ---webpack---> static assets](http://webpack.github.io/assets/what-is-webpack.png)

## Why another module bundler?

Existing module bundlers are not well suited for big projects (big single page application). The most pressing reason for developing another module bundler was [[Code Splitting]]. Another one was that other static assets should fit seamless in the modularization.

I tried to extend existing module bundlers, but it wasn't possible to archive all goals.

## Goals

* Split the dependency tree into chunks, that are loaded on demand
* Every static asset should be able to be a module
* Keep initial loading time low
* Ability to integrate 3rd-party libraries as modules
* Ability to customize nearly every part of the module bundler
* Suited for big projects
