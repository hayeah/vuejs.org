---
title: 从 Vue Router 0.7.x 迁移 (Migration from Vue Router 0.7.x)
type: guide
order: 26
---

> Only Vue Router 2 is compatible with Vue 2, so if you're updating Vue, you'll have to update Vue Router as well. That's why we've included details on the migration path here in the main docs. For a complete guide on using the new Vue Router, see the [Vue Router docs](http://router.vuejs.org/en/).
> 只有 Vue Router 2 才跟 Vue 2 兼容，所以如果要更新 Vue 的版本，你也同样需要更新 Vue Router。这就是为什么我们要在主文档这里提供迁移的详细方法。如果你想查看新 Vue Router 的完整教程，你可以参考 [Vue Roter 的文档](http://router.vuejs.org/en/)。

## 路由器初始化 (Router Initialization)

### `router.start` <sup>弃用 (deprecated)</sup>

There is no longer a special API to initialize an app with Vue Router. That means instead of:
要用 Vue Router 初始化一个应用，你不再需要使用特殊的 API 。这是之前的代码：

``` js
router.start({
  template: '<router-view></router-view>'
}, '#app')
```

You'll just pass a router property to a Vue instance:
现在，你只需要给 Vue 实例传递一个 `router` 属性：

``` js
new Vue({
  el: '#app',
  router: router,
  template: '<router-view></router-view>'
})
```

Or, if you're using the runtime-only build of Vue:
或者，如果你在使用 Vue 的运行时版本：

``` js
new Vue({
  el: '#app',
  router: router,
  render: h => h('router-view')
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.start</code> being called.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到调用 <code>router.start</code> 的代码。</p>
</div>
{% endraw %}

## 路由定义 (Route Definitions)

### `router.map` <sup>弃用 (deprecated)</sup>

Routes are now defined as an array on a [`routes` option](http://router.vuejs.org/en/essentials/getting-started.html#javascript) at router instantiation. So these routes for example:
现在，路由表会通过 [`routes`](http://router.vuejs.org/en/essentials/getting-started.html#javascript) 选项上的一个数组，在路由实例化时定义。所以，下面的代码：

``` js
router.map({
  '/foo': {
    component: Foo
  },
  '/bar': {
    component: Bar
  }
})
```

Will instead be defined with:
应该改成：

``` js
var router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
})
```

The array syntax allows more predictable route matching, since iterating over an object is not guaranteed to use the same key order across browsers.
旧的写法会对一个对象进行迭代，在不同的浏览器上键的迭代顺序可能会不一样，而新的数组语法提高了路由匹配的可预测性。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.map</code> being called.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到调用 <code>router.map</code> 的代码。</p>
</div>
{% endraw %}

### `router.on` <sup>弃用 (deprecated)</sup>

If you need to programmatically generate routes when starting up your app, you can do so by dynamically pushing definitions to a routes array. For example:
如果你要在启动应用时程序化地生成路由，你可以将路由定义动态添加到路由数组。例如：

``` js
// Normal base routes
// 普通的基本路由
var routes = [
  // ...
]

// Dynamically generated routes
// 动态生成的路由
marketingPages.forEach(function (page) {
  routes.push({
    path: '/marketing/' + page.slug
    component: {
      extends: MarketingComponent
      data: function () {
        return { page: page }
      }
    }
  })
})

var router = new Router({
  routes: routes
})
```

If you need to add new routes after the router has been instantiated, you can replace the router's matcher with a new one that includes the route you'd like to add:
如果你需要在路由器实例化以后添加新的路由，你可以用一个新的路由匹配器来取代旧的匹配器，这个新的匹配器包含你想要添加的路由：

``` js
router.match = createMatcher(
  [{
    path: '/my/new/path',
    component: MyComponent
  }].concat(router.options.routes)
)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.on</code> being called.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到调用 <code>router.on</code> 的代码。</p>
</div>
{% endraw %}

### `subRoutes` <sup>弃用 (deprecated)</sup>

[Renamed to `children`](http://router.vuejs.org/en/essentials/nested-routes.html) for consistency within Vue and with other routing libraries.
[重命名为 `children`](http://router.vuejs.org/en/essentials/nested-routes.html)，这是为了与 Vue 的内部代码以及其它的路由库保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>subRoutes</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>subRoutes</code> 选项的代码。</p>
</div>
{% endraw %}

### `router.redirect` <sup>弃用 (deprecated)</sup>

This is now an [option on route definitions](http://router.vuejs.org/en/essentials/redirect-and-alias.html). So for example, you will update:
现在变成了[路由定义的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。例如，下面的代码：

``` js
router.redirect({
  '/tos': '/terms-of-service'
})
```

to a definition like below in your `routes` configuration:
应该改成像这样的路由配置：

``` js
{
  path: '/tos',
  redirect: '/terms-of-service'
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.redirect</code> being called.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到调用 <code>router.redirect</code> 的代码。</p>
</div>
{% endraw %}

### `router.alias` <sup>弃用 (deprecated)</sup>

This is now an [option on the definition for the route](http://router.vuejs.org/en/essentials/redirect-and-alias.html) you'd like to alias to. So for example, you will update:
现在变成了[路由定义上的选项](http://router.vuejs.org/en/essentials/redirect-and-alias.html)。例如，下面的代码：

``` js
router.alias({
  '/manage': '/admin'
})
```

to a definition like below in your `routes` configuration:
应该改成像这样的路由配置：

``` js
{
  path: '/admin',
  component: AdminPanel,
  alias: '/manage'
}
```

If you need multiple aliases, you can also use an array syntax:
如果你需要多个别名，你可以使用数组语法：

``` js
alias: ['/manage', '/administer', '/administrate']
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.alias</code> being called.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到调用 <code>router.alias</code> 的代码。</p>
</div>
{% endraw %}

### 自定义路由属性 (Arbitrary Route Properties)

Arbitrary route properties must now be scoped under the new meta property, to avoid conflicts with future features. So for example, if you had defined:
现在，自定义路由属性应该在新的 `meta` 属性下定义，避免与未来的新特性产生冲突。例如，下面的代码：

``` js
'/admin': {
  component: AdminPanel,
  requiresAuth: true
}
```

Then you would now update it to:
应该改成这样：

``` js
{
  path: '/admin',
  component: AdminPanel,
  meta: {
    requiresAuth: true
  }
}
```

Then when later accessing this property on a route, you will still go through meta. For example:
之后要访问这个属性时，你还是要通过 `meta` 属性。例如：

``` js
if (route.meta.requiresAuth) {
  // ...
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of arbitrary route properties not scoped under meta.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到不是在 `meta` 下定义的自定义路由属性。</p>
</div>
{% endraw %}

## 路由匹配 (Route Matching)

Route matching now uses [path-to-regexp](https://github.com/pillarjs/path-to-regexp) under the hood, making it much more flexible than previously.
路由匹配在底层使用了 [path-to-regexp](https://github.com/pillarjs/path-to-regexp)，比以前更为灵活。

### 一个或多个具名参数 (One or More Named Parameters)

The syntax has changed slightly, so `/category/*tags` for example, should be updated to `/category/:tags+`.
语法有轻微改动，例如，`/category/*tags` 要改成 `/category/:tags+`。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated route syntax.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含弃用路由语法的代码。</p>
</div>
{% endraw %}

## 链接 (Links)

### `v-link` <sup>弃用 (deprecated)</sup>

The `v-link` directive has been replaced with a new [`<router-link>` component](http://router.vuejs.org/en/api/router-link.html), as this sort of job is now solely the responsibility of components in Vue 2. That means whenever wherever you have a link like this:
`v-link` 指令被新的 [`<router-link>` 组件](http://router.vuejs.org/en/api/router-link.html) 取代，因为在 Vue 2 中，它的作用属于组件的职责范围。这意味着，像这样的链接：

``` html
<a v-link="'/about'">About</a>
```

You'll need to update it like this:
应该修改成：

``` html
<router-link to="/about">About</router-link>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link</code> directive.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>v-link</code> 指令的代码。</p>
</div>
{% endraw %}

### `v-link-active` <sup>弃用 (deprecated)</sup>

The `v-link-active` directive has also been deprecated in favor of specifying a separate tag on [the `<router-link>` component](http://router.vuejs.org/en/api/router-link.html). So for example, you'll update this:
`v-link-active` 已经被弃用，现在你应该在 [`<router-link>` 组件上](http://router.vuejs.org/en/api/router-link.html) 定义 `tag` 属性。例如，下面的代码：

``` html
<li v-link-active>
  <a v-link="'/about'">About</a>
</li>
```

to this:
修改成：

``` html
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

The `<a>` will be the actual link (and will get the correct href), but the active class will be applied to the outer `<li>`.
`<a>` 会是最终的链接（并且有正确的 `href`），不过 active 伪类会应用在外部的 `<li>` 上。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link-active</code> directive.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>v-link-active</code> 指令的代码。</p>
</div>
{% endraw %}

## 程序化的导航 (Programmatic Navigation)

### `router.go`

For consistency with the [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API), `router.go` is now only used for [back/forward navigation](https://router.vuejs.org/en/essentials/navigation.html#routergon), while [`router.push`](http://router.vuejs.org/en/essentials/navigation.html#routerpushlocation) is used to navigate to a specific page.
为了与 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 保持一致，`router.go` 现在只用于[后退/前进的导航](https://router.vuejs.org/en/essentials/navigation.html#routergon)，而 [`router.push`](http://router.vuejs.org/en/essentials/navigation.html#routerpushlocation) 则被用来跳转到指定页面。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.go</code> being used where <code>router.push</code> should be used instead.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，将 <code>router.go</code> 修改为 <code>router.push</code>。</p>
</div>
{% endraw %}

## 路由选项：模式 (Router Options: Modes)

### `hashbang: false` <sup>弃用 (deprecated)</sup>

Hashbangs are no longer required for Google to crawl a URL, so they are no longer the default (or even an option) for the hash strategy.
要让 Google 搜索引擎爬取 URL，hashbang 不再是必需的，所以它们不再是哈希策略的默认选项（甚至已经不再是选项了）。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>hashbang: false</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>hashbang: false</code> 选项的代码。</p>
</div>
{% endraw %}

### `history: true` <sup>弃用 (deprecated)</sup>

All routing mode options have been condensed into a single [`mode` option](http://router.vuejs.org/en/api/options.html#mode). Update:
所有的路由模式选项都被浓缩成一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。下面的代码：

``` js
var router = new VueRouter({
  history: 'true'
})
```

to:
要改成：

``` js
var router = new VueRouter({
  mode: 'history'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>history: true</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>history: true</code> 选项的代码。</p>
</div>
{% endraw %}

### `abstract: true` <sup>弃用 (deprecated)</sup>

All routing mode options have been condensed into a single [`mode` option](http://router.vuejs.org/en/api/options.html#mode). Update:
所有的路由模式选项都被浓缩成一个单独的 [`mode` 选项](http://router.vuejs.org/en/api/options.html#mode)。下面的代码：

``` js
var router = new VueRouter({
  abstract: 'true'
})
```

to:
要改成：

``` js
var router = new VueRouter({
  mode: 'abstract'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>abstract: true</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>abstract: true</code> 选项的代码。</p>
</div>
{% endraw %}

## 路由选项：其它 (Route Options: Misc)

### `saveScrollPosition` <sup>弃用 (deprecated)</sup>

This has been replaced with a [`scrollBehavior` option](http://router.vuejs.org/en/advanced/scroll-behavior.html) that accepts a function, so that the scroll behavior is completely customizable - even per route. This opens many new possibilities, but to simply replicate the old behavior of:
被 [`scrollBehavior` 选项](http://router.vuejs.org/en/advanced/scroll-behavior.html) 取代，这个选项接收一个函数，这样可以彻底自定义滑动行为，甚至给每个路由单独定义。这提供了很多新的可能性，不过如果你只是要使用旧的功能，只需要将下面的代码：

``` js
saveScrollPosition: true
```

You can replace it with:
改成：

``` js
scrollBehavior: function (to, from, savedPosition) {
  return savedPosition || { x: 0, y: 0 }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>saveScrollPosition: true</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>saveScrollPosition: true</code> 选项的代码。</p>
</div>
{% endraw %}

### `root` <sup>弃用 (deprecated)</sup>

Renamed to `base` for consistency with [the HTML `<base>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base).
重命名为 `base`，这是为了和 [HTML 的 `<base>` 元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base) 保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>root</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>root</code> 选项的代码。</p>
</div>
{% endraw %}

### `transitionOnLoad` <sup>弃用 (deprecated)</sup>

This option is no longer necessary now that Vue's transition system has explicit [`appear` transition control](transitions.html#Transitions-on-Initial-Render).
不再需要这个选项，因为 Vue 的过渡效果系统现在有了显式的 [`appear` 过渡效果控制](transitions.html#Transitions-on-Initial-Render)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transitionOnLoad: true</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>transitionOnLoad: true</code> 选项的代码。</p>
</div>
{% endraw %}

### `suppressTransitionError` <sup>弃用 (deprecated)</sup>

Removed due to hooks simplification. If you really must suppress transition errors, you can use [`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) instead.
为了简化钩子而移除。如果你确实要屏蔽过渡效果的报错，你可以用 [`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) 取代这个选项。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>suppressTransitionError: true</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>suppressTransitionError: true</code> 选项的代码。</p>
</div>
{% endraw %}

## 路由钩子 (Route Hooks)

### `activate` <sup>弃用 (deprecated)</sup>

Use [`beforeRouteEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.
在组件中用 [`beforeRouteEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>beforeRouteEnter</code> hook.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>beforeRouteEnter</code> 钩子的代码。</p>
</div>
{% endraw %}

### `canActivate` <sup>弃用 (deprecated)</sup>

Use [`beforeEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) in the route instead.
在路由中用 [`beforeEnter`](http://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) 取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canActivate</code> hook.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>canActivate</code> 钩子的代码。</p>
</div>
{% endraw %}

### `deactivate` <sup>弃用 (deprecated)</sup>

Use the component's [`beforeDestroy`](/api/#beforeDestroy) or [`destroyed`](/api/#destroyed) hooks instead.
用组件的 [`beforeDestroy`](/api/#beforeDestroy) 或 [`destroyed`](/api/#destroyed) 钩子取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>deactivate</code> hook.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>deactivate</code> 钩子的代码。</p>
</div>
{% endraw %}

### `canDeactivate` <sup>弃用 (deprecated)</sup>

Use [`beforeRouteLeave`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.
在组件中用 [`beforeRouteLeave`](http://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) 来取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canDeactivate</code> hook.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>canDeactivate</code> 钩子的代码。</p>
</div>
{% endraw %}

### `canReuse: false` <sup>弃用 (deprecated)</sup>

There's no longer a use case for this in the new Vue Router.
在新的 Vue Router 中不再有这个使用场景。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canReuse: false</code> option.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>canReuse: false</code> 选项的代码。</p>
</div>
{% endraw %}

### `data` <sup>弃用 (deprecated)</sup>

The `$route` property is reactive, so you can just use a watcher to react to route changes, like this:
`$route` 属性是响应式的，所以你只要用一个监视器来对路由变化作出响应，像这样：

``` js
watch: {
  '$route': 'fetchData'
},
methods: {
  fetchData: function () {
    // ...
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>data</code> hook.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>data</code> 钩子的代码。</p>
</div>
{% endraw %}

### `$loadingRouteData` <sup>弃用 (deprecated)</sup>

Define your own property (e.g. `isLoading`), then update the loading state in a watcher on the route. For example, if fetching data with [axios](https://github.com/mzabriskie/axios):
自定义一个属性（例如 `isLoading`），然后在路由的监视器中更新加载状态。例如，如果你用 [axios](https://github.com/mzabriskie/axios) 获取数据：

``` js
data: function () {
  return {
    posts: [],
    isLoading: false,
    fetchError: null
  }
},
watch: {
  '$route': function () {
    var self = this
    self.isLoading = true
    self.fetchData().then(function () {
      self.isLoading = false
    })
  }
},
methods: {
  fetchData: function () {
    var self = this
    return axios.get('/api/posts')
      .then(function (response) {
        self.posts = response.data.posts
      })
      .catch(function (error) {
        self.fetchError = error
      })
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>$loadingRouteData</code> meta property.</p>
  <p>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>$loadingRouteData</code> 元属性的代码。</p>
</div>
{% endraw %}
