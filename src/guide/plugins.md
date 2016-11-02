---
title: 插件 (Plugins)
type: guide
order: 18
---

## 开发插件 (Writing a Plugin)

Plugins usually add global-level functionality to Vue. There is no strictly defined scope for a plugin - there are typically several types of plugins you can write:
插件通常会为 Vue 添加全局功能。插件的范围没有严格的限制——通常是下面几种：

1. Add some global methods or properties. e.g. [vue-element](https://github.com/vuejs/vue-element)
添加全局方法或属性，如 [vue-element](https://github.com/vuejs/vue-element)
2. Add one or more global assets: directives/filters/transitions etc. e.g. [vue-touch](https://github.com/vuejs/vue-touch)
添加全局资源：指令/过滤器/过渡等，如 [vue-touch](https://github.com/vuejs/vue-touch)
3. Add some component options by global mixin. e.g. [vuex](https://github.com/vuejs/vuex)
添加组件对象，通过全局继承，如 [vuex](https://github.com/vuejs/vuex)
4. Add some Vue instance methods by attaching them to Vue.prototype.
添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。
5. A library that provides an API of its own, while at the same time injecting some combination of the above. e.g. [vue-router](https://github.com/vuejs/vue-router)
一个库，提供自己的 API，同时提供上面提到的一个或多个功能，如 [vue-router](https://github.com/vuejs/vue-router)

A Vue.js plugin should expose an `install` method. The method will be called with the `Vue` constructor as the first argument, along with possible options:
Vue.js 的插件应当有一个公开方法 install 。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象：

``` js
MyPlugin.install = function (Vue, options) {
  // 1. add global method or property
  //添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // something logic ...
    // 逻辑代码
  }

  // 2. add a global asset
 // 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // something logic ...
      // 逻辑代码
    }
    ...
  })

  // 3. inject some component options
 // 添加组件对象
  Vue.mixin({
    created: function () {
      // something logic ...
      // 逻辑代码
    }
    ...
  })

  // 4. add an instance method
       // 添加实例方法
  Vue.prototype.$myMethod = function (options) {
    // something logic ...
    // 逻辑代码
  }
}
```

## 使用插件 (Using a Plugin)

Use plugins by calling the `Vue.use()` global method:
通过 Vue.use() 全局方法使用插件：

``` js
// calls `MyPlugin.install(Vue)`
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)
```

You can optionally pass in some options:
你可以传入一个选项对象：

``` js
Vue.use(MyPlugin, { someOption: true })
```

`Vue.use` automatically prevents you from using the same plugin more than once, so calling it multiple times on the same plugin will install the plugin only once.
`Vue.use` 自动阻止你使用相同的插件超过一次，所以当你重复的使用这个插件时，你只能安装一次。

Some plugins provided by Vue.js official plugins such as `vue-router` automatically calls `Vue.use()` if `Vue` is available as a global variable. However in a module environment such as CommonJS, you always need to call `Vue.use()` explicitly:
如果 Vue 加载成为全局变量，那么某些官方插件，比如 vue-router， 会自动调用 Vue.use。但是在一个模块运行环境，比如 CommonJS，你必须明文调用 Vue.use()：

``` js
// When using CommonJS via Browserify or Webpack
// 通过 Browserify 或 Webpack 使用 CommonJS 兼容模块
var Vue = require('vue')
var VueRouter = require('vue-router')

// Don't forget to call this
// 不要忘了调用此方法
Vue.use(VueRouter)
```

Checkout [awesome-vue](https://github.com/vuejs/awesome-vue#libraries--plugins) for a huge collection of community-contributed plugins and libraries.
访问 [awesome-vue](https://github.com/vuejs/awesome-vue#libraries--plugins) 获得大量社区贡献的插件和资料。



