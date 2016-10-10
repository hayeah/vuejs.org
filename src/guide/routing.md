---
title: Routing
type: guide
order: 20
---

## Official Router

## <font color="#FF6347">官方路由</font>


For most Single Page Applications, it's recommended to use the officially-supported [vue-router library](https://github.com/vuejs/vue-router). For more details, see vue-router's [documentation](http://vuejs.github.io/vue-router/).

<font color="#FF6347">
一般情况下，建议使用官方提供的路由[vue-router](https://github.com/vuejs/vue-router)。有关详细信息，请参阅[文档](http://vuejs.github.io/vue-router/)。
</font>

## Simple Routing From Scratch
<font color="#FF6347">
## 开始建立简单的路由
</font>

If you just need very simple routing, you can do so by dynamically rendering a page-level component like this:

<font color="#FF6347">
如果只需要一个简单的路由，可以这样写：
</font>

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

<font color="#FF6347">
结合HTML5 History API，可以构建一个小而全的客户端路由。查看实例，[点这里](https://github.com/chrisvfritz/vue-2.0-simple-routing-example).
</font>

## Integrating 3rd-Party Routers

## <font color="#FF6347">整合第三方路由</font>


If there's a 3rd-party router you prefer to use, such as [Page.js](https://github.com/visionmedia/page.js) or [Director](https://github.com/flatiron/director), integration is [similarly easy](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs). Here's a [complete example](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs) using Page.js.

<font color="#FF6347">
若要改用第三方路由，比如[Page.js](https://github.com/visionmedia/page.js) 或者 [Director](https://github.com/flatiron/director), 可以参考这段[示例](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs)。 下面是使用page.js实现路由的一个[完整实例](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs)。
</font>
