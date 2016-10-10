---
title: Installation
type: guide
order: 1
vue_version: 2.0.0-rc.6
dev_size: "184"
min_size: "64"
gz_size: "22"
---
---
title: Installation
type: guide
order: 1
vue_version: 2.0.0-rc.6
dev_size: "184"
min_size: "64"
gz_size: "22"
---

### Compatibility Note
### 兼容性

Vue does **not** support IE8 and below, because it uses ECMAScript 5 features that are un-shimmable in IE8. However it supports all [ECMAScript 5 compliant browsers](http://caniuse.com/#feat=es5).
Vue.js **不支持** IE8 及其以下版本，因为 Vue.js 使用了 IE8 无法实现的 ECMAScript 5 特性。 Vue.js 支持所有[兼容 ECMAScript 5 的浏览器]((http://caniuse.com/#feat=es5))。

### Release Notes
### 更新日志

Detailed release notes for each version are available on [GitHub](https://github.com/vuejs/vue/releases).
每个版本的更新日志见 [GitHub](https://github.com/vuejs/vue/releases)。

## Standalone
## 独立版本

Simply download and include with a script tag. `Vue` will be registered as a global variable.
直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量。

<p class="tip">Don't use the minified version during development. You will miss out all the nice warnings for common mistakes!</p>
<p class="tip">重要提示：在开发时请用开发版本，遇到常见错误它会给出友好的警告。</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Development Version</a><span class="light info">With full warnings and debug mode</span>

<a class="button" href="/js/vue.min.js" download>Production Version</a><span class="light info">Warnings stripped, {{gz_size}}kb min+gzip</span>
</div>
<div id="downloads">
<a class="button" href="/js/vue.js" download>开发版本</a><span class="light info">包含完整的警告和调试模式</span>

<a class="button" href="/js/vue.min.js" download>生产版本</a><span class="light info">删除了警告，{{gz_size}}kb min+gzip</span>
</div>

### CDN
### CDN

Available on [jsdelivr](//cdn.jsdelivr.net/vue/{{vue_version}}/vue.min.js) or [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.min.js) (takes some time to sync so the latest version might not be available yet).
可以从 [jsdelivr](//cdn.jsdelivr.net/vue/{{vue_version}}/vue.min.js) 或 [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.min.js) 获取（版本更新可能略滞后）。

Also available on [unpkg](https://unpkg.com/vue/dist/vue.min.js), which will reflect the latest version as soon as it is published to npm. You can also browse the source of the npm package at [unpkg.com/vue/](https://unpkg.com/vue/).
也可以使用 [unpkg](https://unpkg.com/vue/dist/vue.min.js)，这个链接指向 npm 上的最新版本。你还可以在 [unpkg.com/vue/](https://unpkg.com/vue/) 上浏览 npm 包的源代码。

### CSP environments
### CSP 环境

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The standalone build depends on this feature to compile templates, so is unusable in these environments.
有些环境，例如 Google Chrome Apps，会强制应用内容安全策略（CSP），这会让用来对表达式求值的 `new Function()` 无法使用。独立版本依赖于这个特性来编译模版，所以在这些环境中你无法使用独立版本。

There _is_ a solution however. When using Vue in a build system with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple-2.0) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple-2.0), your templates will be precompiled into `render` functions which work perfectly in CSP environments.
不过这里 _有_ 一个解决方案。当你在一个应用 [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple-2.0) 或者 [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple-2.0) 的构建系统中使用 Vue，你的模版会被预编译到 `render` 函数中，而这在 CSP 环境中是完全没有问题的。

## NPM
## NPM

NPM is the recommended installation method when building large scale applications with Vue. It pairs nicely with module bundlers such as [Webpack](http://webpack.github.io/) or [Browserify](http://browserify.org/). Vue also provides accompanying tools for authoring [Single File Components](application.html#Single-File-Components).
当使用 Vue 来构建大型应用时，我们推荐使用 NPM 来安装 Vue。它跟像 [Webpack](http://webpack.github.io/) 或者 [Browserify](http://browserify.org/) 这样的模块打包器配合得很好。Vue 同时还提供配套工具来开发 [单文件组件](application.html#Single-File-Components)。

``` bash
# latest stable
$ npm install vue@next
```
``` bash
# 最新稳定版本
$ npm install vue@next
```

### Note on NPM Builds
### NPM 版本的注意事项

Because Single File Components pre-compile the templates into render functions at build time, the default export of the `vue` NPM package is the **runtime-only build**, which does not support the `template` option. If you still wish to use the `template` option, you will need to configure your bundler to alias `vue` to the standalone build.
因为单文件组件在构建时会将模版预编译进 render 函数，所以 `vue` NPM 包的默认导出值是 **只用于运行时的版本**，这个版本并不支持 `template` 选项。如果你仍然想要使用 `template` 选项，你会需要配置你的打包器来设置别名 `vue`，让它指向独立版本。

With webpack, add the following alias to your webpack config:
如果你使用 Webpack，添加以下别名到 Webpack 配置：

``` js
resolve: {
  alias: {
    vue: 'vue/dist/vue.js'
  }
}
```
``` js
resolve: {
  alias: {
    vue: 'vue/dist/vue.js'
  }
}
```

For Browserify, you can use [aliasify](https://github.com/benbria/aliasify) for the same effect.
如果你使用 Browserify，你可以使用 [aliasify](https://github.com/benbria/aliasify) 来达到同样的效果。

<p class="tip">Do NOT do `import Vue from 'vue/dist/vue'` - since some tools or 3rd party libraries may import vue as well, this may cause the app to load both the runtime and standalone builds at the same time and lead to errors.</p>
<p class="tip">不要写成 `import Vue from 'vue/dist/vue'` —— 因为某些工具或者第三方库也可能导入 vue，这可能会导致应用同时倒入运行时版本和独立版本，导致错误发生。</p>

## CLI
## 命令行工具

Vue.js provides an [official CLI](https://github.com/vuejs/vue-cli) for quickly scaffolding ambitious Single Page Applications. It provides batteries-included build setups for a modern frontend workflow. It takes only a few minutes to get up and running with hot-reload, lint-on-save, and production-ready builds:
Vue.js 提供了一个 [官方命令行工具](https://github.com/vuejs/vue-cli)，用来快速构建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需一分钟即可启动带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：

``` bash
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project
# install dependencies and go!
$ cd my-project
$ npm install
$ npm run dev
```
``` bash
# 安装 vue-cli
$ npm install --global vue-cli
# 使用 `webpack` 模版来构建一个新项目
$ vue init webpack my-project
# 安装依赖，走起！
$ cd my-project
$ npm install
$ npm run dev
```

## Dev Build
## 开发版本

**Important**: the CommonJS bundle distributed on NPM (`vue.common.js`) is only checked in during releases on the `next` branch. To use Vue from the latest source code on GitHub, you will have to build it yourself!
**重点**：发布到 NPM 上的 CommonJS 包 (`vue.common.js`) 只在发布新版本时签入 `next` 分支。要使用 Github 上的最新源码，你需要自己构建：

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
git checkout next
npm install
npm run build
```
``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
git checkout next
npm install
npm run build
```

## Bower
## Bower

``` bash
# latest stable
$ bower install vue#next
```
``` bash
# 最新稳定版本
$ bower install vue#next
```

## AMD Module Loaders
## AMD 模块加载器

The standalone downloads or versions installed via Bower are wrapped with UMD so they can be used directly as an AMD module.
独立版本或者通过 Bower 安装的版本已用 UMD 包装，因此它们可以被当作 AMD 模块来直接使用。
