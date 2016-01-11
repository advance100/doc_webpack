如今的网站正在向wepapp快速发展：
* 网页中出现了越来越多的javascript.
* 在现代的浏览器你可以往里面塞进更多的代码.
* 整页的刷新变少了 ，更多的代码被留在同一页面执行.

其结果就是在你的客户端有了一堆的代码！！！

一个庞大的代码库需要被管理，而模块系统提供了将你的代码库归类到模块中去的机会。

# 模块系统风格

这里有多种如何定义依赖和输出(exports)的标准：
* `<script>`-标签风格 (非模块系统)
* CommonJs
* AMD以及其中的一些方言
* ES6模块
* 还有其他的...

## `<script>`-标签风格

如果你并没有使用模块化系统，这个将是你用来处理模块化的代码库的方式
``` html
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```
每个模块暴露向全局对象暴露出一个接口，例如window对象。其他模块可以通过全局对象过去其依赖的模块

#### 普遍问题

* 在全局对象中的命名冲突问题.
* 加载的顺序很重要.
* 开发者们不得不解决modules/libraries间的依赖问题.
* 在大项目中这个script列表将会很长并且很难去管理。

## CommonJs: 同步加载方式

这种风格是使用一个同步的require方法来加载依赖然后再返回输出接口。模块能够通过给exports对象添加属性或给module.exports赋值来暴露接口指定输出接口。

``` javascript
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```

这种方式通常被使用在服务端 [node.js](http://nodejs.org).

#### Pros

* 服务端模块能够被复用
* 已经有很多模块使用这种风格了(npm)
* 简单易用.

#### Cons

* 阻塞调用的方式在网络上应用并不好。网络请求都是异步的。
* 多模块间没有并行加载。

#### Implementations

* [node.js](http://nodejs.org/) - 服务端
* [browserify](https://github.com/substack/node-browserify)
* [modules-webmake](https://github.com/medikoo/modules-webmake) - 编译成bundle
* [wreq](https://github.com/substack/wreq) - 客户端

## AMD: 异步加载方式

[`Asynchronous Module Definition`](https://github.com/amdjs/amdjs-api/wiki/AMD)

其他模块系统（对浏览器而言）都有着异步加载这方面的问题(CommonJs)，因此这个实现了异步(还有定义module和exports)的版本就被推出了台面：

``` javascript
require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
```

#### Pros

* 网络中很适合这种异步请求的方式。
* 能够并行加载多个模块.

#### Cons

* 代码开销大，可读性以及编程方面都比较难。
* 看起来像是某种变通方式(workaround)。

#### Implementations

* [require.js](http://requirejs.org/) - 客户端
* [curl](https://github.com/cujojs/curl) - 客户端

Read more about [[CommonJs]] and [[AMD]].

## ES6 modules

EcmaScript6 adds some language constructs to JavaScript, which form another module system.

``` javascript
import "jquery";
export function doStuff() {}
module "localModule" {}
```

#### Pros

* 容易静态分析
* 作为ES标准，不会过早过时

#### Cons

* 浏览器的内置支持还需要时间
* 目前很少模块使用这种方式

## 折衷方案

给开发者选择模块方式的机会，允许现有代码正常运行。想办法使得定制模块方式更容易。

---

# 网络传输

模块应该是在客户端被执行的，所以它们必须从服务器传送到浏览器

以下是关于如何传输模块的两种极端：

* 一个模块一次请求。
* 所有的模块只用一次请求。

这两种方案应用都很广泛。但都不是最佳选择：

* 一个模块一次请求。
    * 优点: 只有用到的模块才会被传输。
    * 缺点:过多的请求以为这更大的系统开销。
    * 缺点: 以为请求的延迟，拖慢了应用的启动。
* 所有模块一次请求
    * 优点: 更小的请求带来的开销以及更小的延迟。
    * 缺点: 不需要的模块也被传送了。

## 分块传输

这是一种更好的比较灵活的变通方案。在通常情况下，这种在两个极端中折衷的方案更好。

→ 当编译所有的模块时: 将一批模块分到更小的代码块中。

我们会得到多个较小的请求。那些初始化时没有被请求、包含着多个模块的代码块(chunk)只有在需要的时候才会被请求. 初始化请求中并不会包含你全部的代码，因此请求也就更小。

代码分块的"分割点"很随意，这取决于开发者。

→ 一个大型的代码库成为了可能。

提示: The [idea is from Google's GWT](https://developers.google.com/web-toolkit/doc/latest/DevGuideCodeSplitting). 

Read more about [[Code Splitting]].

---

# 为什么只是javascript?

为什么一个模块系统只是帮助开发者管理javascript? 有很多的静态资源也是需要处理的:

* 样式表
* 图片
* 字体
* html模板
* etc.

还有:

* coffeescript → javascript
* elm → javascript
* less stylesheets → css样式表
* jade templates → 生成html的javascript
* i18n files → something
* etc.

它们应该像下面一样被轻松使用:

``` javascript
require("./style.css");
```

``` javascript
require("./style.less");
require("./template.jade");
require("./image.png");
```

Read more about [[Using loaders]] and [[Loaders]].

---

# 静态分析

当我们在编译模块时，会通过静态分析发现它们的依赖关系。

通常来讲它无法使用表达式，只能发现一些简单的依赖关系，然而像`require("./template/" + templateName + ".jade")` 这种语法很普遍.

许多类库被写成多种不同的风格，它们有些看起来很奇怪...

## 策略

需要一种更智能的解析器来允许现有的代码运行。如果开发者干了些奇怪的事，也要尝试用最兼容的方式解决