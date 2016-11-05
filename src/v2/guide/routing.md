---
title: 路由 (Routing)
type: guide
order: 21
---

## 官方路由 (Official Router)

For most Single Page Applications, it's recommended to use the officially-supported [vue-router library](https://github.com/vuejs/vue-router). For more details, see vue-router's [documentation](http://vuejs.github.io/vue-router/).
对于大多数单页应用，建议使用官方提供的路由库 [vue-router](https://github.com/vuejs/vue-router)。详情请参考 vue-router 的[文档](http://vuejs.github.io/vue-router/)。

## 开始建立简单的路由 (Simple Routing From Scratch)

If you just need very simple routing and do not wish to involve a full-featured router library, you can do so by dynamically rendering a page-level component like this:
如果你只需要一个简单的路由，而不想引入一个全功能的路有库，可以像这样动态渲染一个页级组件：

``` js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

Combined with the HTML5 History API, you can build a very basic but fully-functional client-side router. To see that in practice, check out [this example app](https://github.com/chrisvfritz/vue-2.0-simple-routing-example).
结合 HTML5 History API，可以构建一个基本但功能齐全的客户端路由。可以查看这个[示例应用](https://github.com/chrisvfritz/vue-2.0-simple-routing-example)。

## 整合第三方路由 (Integrating 3rd-Party Routers)

If there's a 3rd-party router you prefer to use, such as [Page.js](https://github.com/visionmedia/page.js) or [Director](https://github.com/flatiron/director), integration is [similarly easy](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs). Here's a [complete example](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs) using Page.js.
若要改用第三方路由，比如 [Page.js](https://github.com/visionmedia/page.js) 或者 [Director](https://github.com/flatiron/director), 整合过程也是[同样简单](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs)。这里有一个使用 page.js 实现路由的[完整示例](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs)。
