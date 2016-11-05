---
title: 安装 (Installation)
type: guide
order: 1
vue_version: 2.0.5
dev_size: "194.65"
min_size: "64.28"
gz_size: "23.55"
ro_gz_size: "undefined"
---

### 兼容性 (Compatibility Note)

Vue does **not** support IE8 and below, because it uses ECMAScript 5 features that are un-shimmable in IE8. However it supports all [ECMAScript 5 compliant browsers](http://caniuse.com/#feat=es5).
Vue.js **不支持** IE8 及其以下版本，因为 Vue.js 使用了 IE8 无法实现的 ECMAScript 5 特性。 Vue.js 支持所有[兼容 ECMAScript 5 的浏览器]((http://caniuse.com/#feat=es5)。

### 更新日志 (Release Notes)

Detailed release notes for each version are available on [GitHub](https://github.com/vuejs/vue/releases).
每个版本的更新日志见 [GitHub](https://github.com/vuejs/vue/releases)。

## 独立版本 (Standalone)

Simply download and include with a script tag. `Vue` will be registered as a global variable.
直接下载并用 `<script>` 标签引入。`Vue` 会被注册为一个全局变量。

<p class="tip">Don't use the minified version during development. You will miss out all the nice warnings for common mistakes!
重要提示：开发时请用开发版本。它会对常见错误给出友好的警告。</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Development Version</a><span class="light info">With full warnings and debug mode</span>
<a class="button" href="/js/vue.js" download>开发版本</a><span class="light info">包含完整的警告和调试模式</span>

<a class="button" href="/js/vue.min.js" download>Production Version</a><span class="light info">Warnings stripped, {{gz_size}}kb min+gzip</span>
<a class="button" href="/js/vue.min.js" download>生产版本</a><span class="light info">删除了警告，{{gz_size}}kb min+gzip</span>
</div>

### CDN

Recommended: [unpkg](https://unpkg.com/vue/dist/vue.js), which will reflect the latest version as soon as it is published to npm. You can also browse the source of the npm package at [unpkg.com/vue/](https://unpkg.com/vue/).
推荐使用 [unpkg](https://unpkg.com/vue/dist/vue.min.js)，它是 npm 上的最新版本。你也可以在 [unpkg.com/vue/](https://unpkg.com/vue/) 上浏览 npm 包的源代码。

Also available on [jsdelivr](//cdn.jsdelivr.net/vue/{{vue_version}}/vue.js) or [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js), but these two services take some time to sync so the latest release may not be available yet.
你还可以从 [jsdelivr](//cdn.jsdelivr.net/vue/{{vue_version}}/vue.min.js) 或 [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.min.js) 获取，但这两个服务的版本更新可能略滞后。

## NPM

NPM is the recommended installation method when building large scale applications with Vue. It pairs nicely with module bundlers such as [Webpack](http://webpack.github.io/) or [Browserify](http://browserify.org/). Vue also provides accompanying tools for authoring [Single File Components](single-file-components.html).
当用 Vue 来构建大型应用时，我们推荐使用 NPM 来安装 Vue。它跟 [Webpack](http://webpack.github.io/) 或者 [Browserify](http://browserify.org/) 这样的模块打包器可以和很好地配合。Vue 同时还提供配套工具来开发[单文件组件](single-file-components.html)。

``` bash
# latest stable
# 最新稳定版本
$ npm install vue
```

### 独立版本和运行时版本 (Standalone vs. Runtime-only Build)

There are two builds available, the standalone build and the runtime-only build. The difference being that the former includes the **template compiler** and the latter does not.
Vue.js 有两个版本：独立版本和运行时版本。它们的区别在于，独立版本包含**模版编译器**，而运行时版本不包含。

The template compiler is responsible for compiling Vue template strings into pure JavaScript render functions. If you want to use the `template` option, then you need the compiler.
模版编译器负责将 Vue 的模版字符串编译成纯 JavaScript 渲染函数。如果你想使用 `template` 选项，那么你就需要模版编译器。

- The standalone build includes the compiler and supports the `template` option. **It also relies on the presence of browser APIs so you cannot use it for server-side rendering.**
独立版本包含了模版编译器，支持 `tempate` 选项。**它依赖浏览器的 API，所以你不能在服务器端渲染中使用它。**

- The runtime-only build does not include the template compiler, and does not support the `template` option. You can only use the `render` option when using the runtime-only build, but it works with single-file components, because single-file components' templates are pre-compiled into `render` functions during the build step. The runtime-only build is roughly 30% lighter-weight than the standalone build, weighing only {{ro_gz_size}}kb min+gzip.
运行时版本不包含模版编译器，也不支持 `template` 选项。在使用运行时版本时，你只能使用 `render` 选项。不过它可以和单文件组件一起使用，因为在构建阶段，单文件组件的模版已经被预编译进 `render` 函数了。运行时版本的文件大小大概是独立版本的 70%，也就是约 {{ro_gz_size}}kb min+gzip。

By default, the NPM package exports the **runtime-only** build. To use the standalone build, add the following alias to your webpack config:
NPM  包默认导出运行时版本。如果要使用独立版本，你需要在 webpack 的配置文件里加上：

``` js
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.js'
  }
}
```

For Browserify, you can use [aliasify](https://github.com/benbria/aliasify) to achieve the same.
如果你使用 Browserify，你可以用 [aliasify](https://github.com/benbria/aliasify) 来达到同样的效果。

<p class="tip">Do NOT do `import Vue from 'vue/dist/vue'` - since some tools or 3rd party libraries may import vue as well, this may cause the app to load both the runtime and standalone builds at the same time and lead to errors.<br>不要写成 `import Vue from 'vue/dist/vue'` —— 因为某些工具或者第三方库也可能导入 vue，这可能会让应用同时导入运行时版本和独立版本，导致错误发生。</p>

### CSP 环境 (CSP environments)

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The standalone build depends on this feature to compile templates, so is unusable in these environments.
有些环境，例如 Google Chrome Apps，会强制应用内容安全策略（CSP），这会让用来对表达式求值的 `new Function()` 无法使用。独立版本依赖于这个特性来编译模版，所以在这些环境中你无法使用独立版本。

On the other hand, the runtime-only build is fully CSP-compliant. When using the runtime-only build with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), your templates will be precompiled into `render` functions which work perfectly in CSP environments.
另外，运行时版本是完美兼容 CSP 的。当你将运行时版本和 [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple-2.0) 或者 [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple-2.0) 一起使用时，你的模版会被预编译到 `render` 函数中，而这在 CSP 环境中是完全没有问题的。

## 命令行工具 (CLI)

Vue.js provides an [official CLI](https://github.com/vuejs/vue-cli) for quickly scaffolding ambitious Single Page Applications. It provides batteries-included build setups for a modern frontend workflow. It takes only a few minutes to get up and running with hot-reload, lint-on-save, and production-ready builds:
Vue.js 提供了一个 [官方命令行工具](https://github.com/vuejs/vue-cli)，用来快速构建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需一分钟即可启动带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：

``` bash
# install vue-cli
# 安装 vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
# 使用 `webpack` 模版来构建一个新项目
$ vue init webpack my-project
# install dependencies and go!
# 安装依赖，开始开发！
$ cd my-project
$ npm install
$ npm run dev
```

<p class="tip">The CLI assumes prior knowledge of Node.js and the associated build tools. If you are new to Vue or front-end build tools, we strongly suggest going through the <a href="/guide">guide</a> without any build tools before using the CLI.<br>要使用命令行工具，你需要了解 Node.js 以及相关的构建工具。如果你对 Vue 或者前端构建工具还不太了解，我们强烈建议你在使用命令行工具之前，先阅读不使用任何构建工具的 <a href="/guide">教程</a>。</p>

## 开发版本 (Dev Build)

**Important**: the built files in GitHub's `/dist` folder are only checked-in during releases. To use Vue from the latest source code on GitHub, you will have to build it yourself!
**重点**：只有在版本发布时，我们才会将构建好的文件放在 Github 的 `dist` 目录里。所以，如果要使用 Github 上的最新源码，你需要自己构建！

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

``` bash
# latest stable
# 最新稳定版本
$ bower install vue
```

## AMD 模块加载器 (AMD Module Loaders)

The standalone downloads or versions installed via Bower are wrapped with UMD so they can be used directly as an AMD module.
独立版本或者通过 Bower 安装的版本已用 UMD 包装，因此它们可以被当作 AMD 模块来直接使用。
