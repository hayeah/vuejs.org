---
title: 生产环境部署 (Deploying For Production)
type: guide
order: 20
---

## 去除警告 (Stripping Out Warnings)

The minified standalone build of Vue has already stripped out all the warnings for you for a smaller file size, but when you are using tools like Webpack or Browserify, you will need some additional configuration to achieve this.
为了缩减文件大小，Vue 的最小化独立版本已经去除了所有警告。不过，如果你在使用 Webpack 或 Browserify 这样的工具，那就需要做一些额外的配置。

### Webpack

Use Webpack's [DefinePlugin](http://webpack.github.io/docs/list-of-plugins.html#defineplugin) to indicate a production environment, so that warning blocks can be automatically dropped by UglifyJS during minification. Example config:
用 Webpack 的 [DefinePlugin](http://webpack.github.io/docs/list-of-plugins.html#defineplugin) 来表明你处于生产环境，这样 UglifyJS 就会在压缩阶段自动去除警告块。配置的示例如下：

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    })
  ]
}
```

### Browserify

- Run your bundling command with `NODE_ENV` set to `"production"`. This tells `vueify` to avoid including hot-reload and development related code.
将 `NODE_ENV` 设成 `"production"`，然后运行打包命令。这会让 `vueify` 不打包热重载和开发相关的代码。

- Apply a global [envify](https://github.com/hughsk/envify) transform to your bundle. This allows the minifier to strip out all the warnings in Vue's source code wrapped in env variable conditional blocks. For example:
用 [envify](https://github.com/hughsk/envify) 对你的打包文件进行全局转换。这会让简化工具检查 Vue 源代码中的环境变量分支块，去除其中的所有警告。比如：
``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

- To extract styles to a separate css file use a extract-css plugin which is included in vueify.
用包含在 vueify 中的 extract-css 插件，将样式提取到独立的 CSS 文件。

``` bash
NODE_ENV=production browserify -g envify -p [ vueify/plugins/extract-css -o build.css ] -e main.js | uglifyjs -c -m > build.js
```

## 追踪运行时错误 (Tracking Runtime Errors)

If a runtime error occurs during a component's render, it will be passed to the global `Vue.config.errorHandler` config function if it has been set. It might be a good idea to leverage this hook together with an error-tracking service like [Sentry](https://sentry.io), which provides [an official integration](https://sentry.io/for/vue/) for Vue.
如果在组件渲染期间发生了运行时错误，这个错误会被传递到全局的 `Vue.config.errorHandler` 配置函数（如果已设置）。一个很好的做法是，将这个钩子函数和错误追踪服务（比如 [Sentry](https://sentry.io)）组合使用，Sentry 甚至还为 Vue 提供了 [一个官方插件](https://sentry.io/for/vue/)。

## 提取 CSS (Extracting CSS)

When using [Single-File Components](./single-file-components.html), the `<style>` tags are injected dynamically at runtime during development. In production you may want to extract the styles across all components into a single CSS file. For details on how to achieve this, consult the respective documentation for [vue-loader](http://vue-loader.vuejs.org/en/configurations/extract-css.html) and [vueify](https://github.com/vuejs/vueify#css-extraction).
在开发环境中，如果你使用[单文件组件](./single-file-components.html)，`<style>` 标签会在运行时被动态注入。而在生产环境中，你可能要将不同部件的样式提取出来，放到一个单独的 CSS 文件中。要实现这个功能，你可以分别参考 [vue-loader](http://vue-loader.vuejs.org/en/configurations/extract-css.html) 和 [vueify](https://github.com/vuejs/vueify#css-extraction) 的文档。

The official `webpack` template from `vue-cli` has this already configured out of the box.
`vue-cli` 的官方 `webpack` 模版已经帮你自动配置好这个功能。
