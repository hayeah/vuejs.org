---
title: 单文件组件 (Single File Components)
type: guide
order: 19
---

## 介绍 (Introduction)

In many Vue projects, global components will be defined using `Vue.component`, followed by `new Vue({ el: '#container '})` to target a container element in the body of every page.
在很多 Vue 项目中，全局组件都是通过 `Vue.component` 来定义的，然后用 `new Vue({ el: '#container '})` 在每一页指定一个容器元素。

This can work very well for small to medium-sized projects, where JavaScript is only used to enhance certain views. In more complex projects however, or when your frontend is entirely driven by JavaScript, these disadvantages become apparent:
这种方式在中小型项目中没有问题，因为在这些项目中 JavaScript 只是用来增强某些视图而已。不过在更复杂的项目中，或者当你的整个前端都是由 JavaScript 驱动时，以下缺点就会变得很明显：

- **Global definitions** force unique names for every component
- **全局定义**要求组件不能重名；
- **String templates** lack syntax highlighting and require ugly slashes for multiline HTML
- **字符串模版**没有语法高亮，多行 HTML 需要使用丑陋的 `\`；
- **No CSS support** means that while HTML and JavaScript are modularized into components, CSS is conspicuously left out
- **没有 CSS 支持**，也就是说虽然 HTML 和 JavaScript 通过组件模块化了，CSS 却被丢到一边；
- **No build step** restricts us to HTML and ES5 JavaScript, rather than preprocessors like Pug (formerly Jade) and Babel
- **没有构建步骤**，这样我们就只能使用 HTML 和 ES5，而没办法用上 Pug（以前的 Jade） 和 Babel 这样的预编译器。

All of these are solved by **single-file components** with a `.vue` extension, made possible with build tools such as Webpack or Browserify.
通过使用带有 `.vue` 扩展名的**单文件组件**，以及 Webpack 和 Browserify 这样的构建工具，我们可以解决上面的问题。

Here's a simple example of a file we'll call `Hello.vue`:
这是一个文件名为 `Hello.vue` 的简单示例：

<img src="/images/vue-component.png" style="display: block; margin: 30px auto">

Now we get:
现在我们就拥有了：

- [Complete syntax highlighting](https://github.com/vuejs/awesome-vue#syntax-highlighting)
- [完整的语法高亮](https://github.com/vuejs/awesome-vue#syntax-highlighting)
- [CommonJS modules](https://webpack.github.io/docs/commonjs.html)
- [CommonJS 模块](https://webpack.github.io/docs/commonjs.html)
- [Component-scoped CSS](https://github.com/vuejs/vue-loader/blob/master/docs/en/features/scoped-css.md)
- [组件域 CSS](https://github.com/vuejs/vue-loader/blob/master/docs/en/features/scoped-css.md)

As promised, we can also use preprocessors such as Jade, Babel (with ES2015 modules), and Stylus for cleaner and more feature-rich components.
就像之前说的，我们可以使用 Jade，Babel（和它的 ES2015 模块），以及 Stylus 这样的预编译器，以此构建更优雅功能更丰富的组件。

<img src="/images/vue-component-with-preprocessors.png" style="display: block; margin: 30px auto">

These specific languages are just examples. You could just as easily use Buble, TypeScript, SCSS, PostCSS - or whatever other preprocessors that help you be productive.
这些具体的语言只是示例。你可以同样轻松地用上 Buble，TypeScript，SCSS，PostCSS，或者任何能提高你生产效率的预编译器。

<!-- TODO: include CSS modules once it's supported in vue-loader 9.x -->

## 上手 (Getting Started)

### 如果你是 JavaScript 模块构建系统的新手 (For Users New to Module Build Systems in JavaScript)

With `.vue` components, we're entering the realm of advanced JavaScript applications. That means learning to use a few additional tools if you haven't already:
使用 `.vue` 组件，我们已经进入了高级 JavaScript 应用的领域。这意味着你需要学习使用一些额外的工具：

- **Node Package Manager (NPM)**: Read the [Getting Started guide](https://docs.npmjs.com/getting-started/what-is-npm) through section _10: Uninstalling global packages_.
- **Node Package Manager (NPM)**: 从头阅读[上手指南](https://docs.npmjs.com/getting-started/what-is-npm) 直到 _10: Uninstalling global packages_；

- **Modern JavaScript with ES2015/16**: Read through Babel's [Learn ES2015 guide](https://babeljs.io/docs/learn-es2015/). You don't have to memorize every feature right now, but keep this page as a reference you can come back to.
- **Modern JavaScript with ES2015/16**: 阅读 Babel 的 [ES2015 教程](https://babeljs.io/docs/learn-es2015/)。你不需要记得每一个特性，但可以留着这个页面供日后参考。

After you've taken a day to dive into these resources, we recommend checking out the [webpack-simple](https://github.com/vuejs-templates/webpack-simple) template. Follow the instructions and you should have a Vue project with `.vue` components, ES2015 and hot-reloading running in no time!
在你花上一天钻研这些资源以后，我们推荐你看一下 [webpack-simple](https://github.com/vuejs-templates/webpack-simple) 模版。按照上面的指引，你很快就可以创建一个支持 `.vue` 组件，ES2015 以及热加载的项目！

The template uses [Webpack](https://webpack.github.io/), a module bundler that takes a number of "modules" and then bundle them into your final application. To learn more about Webpack itself, [this video](https://www.youtube.com/watch?v=WQue1AN93YU) offers a good intro. Once you get past the basics, you might also want to check out [this advanced Webpack course on Egghead.io](https://egghead.io/courses/using-webpack-for-production-javascript-applications).
这个模版使用了 [Webpack](https://webpack.github.io/)，它是一个模块打包器，会将多个“模块”打包成最终的应用。要更多地了解 Webpack，可以看[这个视频](https://www.youtube.com/watch?v=WQue1AN93YU)。掌握了基础之后，你还可以看一下 [Egghead.io 上面的进阶 Webpack 课程](https://egghead.io/courses/using-webpack-for-production-javascript-applications)。

In Webpack, each module can be transformed by a "loader" before being included in the bundle, and Vue offers the [vue-loader](https://github.com/vuejs/vue-loader) plugin to take care of translating `.vue` single-file components. The [webpack-simple](https://github.com/vuejs-templates/webpack-simple) template has already set up everything for you, but if you'd like to learn more about how `.vue` components work with Webpack, you can read [the docs for vue-loader](https://vue-loader.vuejs.org).
在 Webpack 中，每一个模块在被打包前都会通过“加载器”来转换，而 Vue 提供了一个 [vue-loader](https://github.com/vuejs/vue-loader) 插件来负责转译 `.vue` 单文件组件。[webpack-simple](https://github.com/vuejs-templates/webpack-simple) 模版已经帮你配置好所有东西，不过如果你想要更多地了解 `.vue` 和 Webpack 是如何配合的，你可以阅读 [vue-loader 的文档](https://vue-loader.vuejs.org)。

### 如果你是老司机 (For Advanced Users)

Whether you prefer Webpack or Browserify, we have documented templates for both simple and more complex projects. We recommend browsing [github.com/vuejs-templates](https://github.com/vuejs-templates), picking a template that's right for you, then following the instructions in the README to generate a new project with [vue-cli](https://github.com/vuejs/vue-cli).
无论你喜欢 Webpack 还是 Browserify，我们都对简单或复杂的项目提供带有文档的模版。你可以浏览 [github.com/vuejs-templates](https://github.com/vuejs-templates)，挑一个适合你的模版，然后按照 README 中的指引，用 [vue-cli](https://github.com/vuejs/vue-cli) 生成一个新项目。
