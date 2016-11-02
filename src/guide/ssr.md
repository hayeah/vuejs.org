---
title: 服务器端渲染 (Server-Side Rendering)
type: guide
order: 24
---

## 你需要服务器端渲染吗？(Do You Need SSR?)

Before diving into SSR, let's explore what it actually does for you and when you might need it.
在深入了解服务器端渲染前，我们先看一下它的用途，以及什么时候需要用它。

### 搜索引擎优化 (SEO)

Google and Bing can index synchronous JavaScript applications just fine. _Synchronous_ being the key word there. If your app starts with a loading spinner, then fetches content via Ajax, the crawler will not wait for you to finish.
谷歌和必应搜索引擎都可以很好地收录同步的 JavaScript 应用。注意，这里的关键词是_同步_。如果应用启动时有一个加载动画，然后通过 Ajax  获取内容，那么爬虫不会等到应用完成加载。

This means if you have content fetched asynchronously on pages where SEO is important, SSR might be necessary.
这意味，如果你的页面有异步获取的内容，而这些页面的搜索引擎优化又很重要，这时候你就需要服务器端渲染了。

### 网络较慢的客户端 (Clients with a Slow Internet)

Users might come to your site from a remote area with slow Internet - or just with a bad cell connection. In these cases, you'll want to minimize the number and size of requests necessary for users to see basic content.
用户可能在网络比较慢的偏远地方访问你的网站 - 也有只是它的网速比较渣。这些情况下，你应该尽量减少页面请求的必要数量和文件大小，保证用户可以看到基本的内容。

You can use [Webpack's code splitting](https://webpack.github.io/docs/code-splitting.html) to avoid forcing users to download your entire application to view a single page, but it still won't be as performant as downloading a single, pre-rendered HTML file.
要避免用户下载全部应用代码，只为了看一个页面，你可以用 [Webpack 的代码拆分功能](https://webpack.github.io/docs/code-splitting.html)。不过，这样做的效果，也还是比不上下载单个预渲染的 HTML 文件。

### 运行老式（或者甚至没有）JavaScript 引擎的客户端 (Clients with an Old (or Simply No) JavaScript Engine)

For some demographics or areas of the world, using a computer from 1998 to access the Internet might be the only option. While Vue only works with IE9+, you may still want to deliver basic content to those on older browsers - or to hipster hackers using [Lynx](http://lynx.browser.org/) in the terminal.
对于某些地区某些人群，他们只能用很多年前生产的老式电脑访问互联网。虽然 Vue 只能运行在 IE9+ 的浏览器，你可能仍然希望给那些使用老式浏览器的用户提供基础内容 - 或者是在命令行中使用 [Lynx](http://lynx.browser.org/) 的时髦的黑客。

### 服务器端渲染和预渲染 (SSR vs Prerendering)

If you're only investigating SSR to improve the SEO of a handful of marketing pages (e.g. `/`, `/about`, `/contact`, etc), then you probably want __prerendering__ instead. Rather than using a web server to compile HTML on-the-fly, prerendering simply generates static HTML files for specific routes at build time. The advantage is setting up prerendering is much simpler and allows you to keep your frontend as a fully static site.
如果你在了解无服务器端渲染，只是为了几个营销页面（比如首页，关于，联系方式等页面）的搜索引擎优化，那你应该选择_预渲染_。预渲染不像服务器端渲染那样即时编译 HTML，它只是在构建时为特定的路由生成静态页面。预渲染的优点是设置更加简单，而且保证你的前端是一个完整的静态站。

If you're using Webpack, you can easily add prerendering with the [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin). It's been extensively tested with Vue apps - and in fact, the creator is a member of the Vue core team.
如果你使用 Webpack，可以通过 [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin) 轻松地添加预渲染功能。它被广泛地用在 Vue 应用上 - 事实上，它的作者也是 Vue 核心团队的成员。

## Hello World

If you've gotten this far, you're ready to see SSR in action. It sounds complex, but a simple node script demoing the feature requires only 3 steps:
如果你看到这里，你应该已经准备好在实战中使用服务器端渲染了。服务器端渲染听起来很复杂，不过一个简单的 Node.js 脚本只需要三步就可以展示这个特性：

``` js
// Step 1: Create a Vue instance
// 步骤 1:创建一个Vue实例
var Vue = require('vue')
var app = new Vue({
  render: function (h) {
    return h('p', 'hello world')
  }
})

// Step 2: Create a renderer
// 步骤 2: 创建一个渲染器
var renderer = require('vue-server-renderer').createRenderer()

// Step 3: Render the Vue instance to HTML
// 步骤 3: 将 Vue实例 渲染成 HTML
renderer.renderToString(app, function (error, html) {
  if (error) throw error
  console.log(html)
  // => <p server-rendered="true">hello world</p>
})
```

Not so scary, right? Of course, this example is much simpler than most applications. We don't yet have to worry about:
看起来不难吧？当然，这个示例比大部分应用都简单。因为我们还不用担心一下问题：

- A Web Server
Web 服务器
- Response Streaming
流式响应
- Component Caching
组件缓存
- A Build Process
构建过程
- Routing
路由
- Vuex State Hydration
Vuex 状态重现

In the rest of this guide, we'll walk through how to work with some of these features. Once you understand the basics, we'll then direct you to more detailed documentation and advanced examples to help you handle edge cases.
在这个教程剩下的部分，我们会引导你解决其中一部分问题。在你掌握了基础过后，我们会将你指向更详细的文档以及进阶实例，它们可以帮助你处理边缘场景。

## 用 Express Web 服务器实现简单的服务器端渲染 (Simple SSR with the Express Web Server)

It's kind of a stretch to call it "server-side rendering" when we don't actually have a web server, so let's fix that. We'll build a very simple SSR app, using only ES5 and without any build step or Vue plugins.
如果没有一个 Web 服务器，很难把我们的实现称为“服务器端渲染”，所以我们会先来解决这个问题。我们将构建一个非常简单的服务器端渲染应用，只用ES5，没有任何构建步骤或 Vue 插件。

We'll start off with an app that just tells the user how many seconds they've been on the page:
我们先来写一个应用，它会告诉用户在这个页面上停留的时间：

``` js
new Vue({
  template: '<div>You have been here for {{ counter }} seconds.</div>',
  data: {
    counter: 0
  },
  created: function () {
    var vm = this
    setInterval(function () {
      vm.counter += 1
    }, 1000)
  }
})
```

To adapt this for SSR, there are a few modifications we'll have to make, so that it will work both in the browser and within node:
为了适应服务器端渲染，我们需要进行一些修改，让代码可以在浏览器和 Node.js 中运行：

- When in the browser, add an instance of our app to the global context (i.e. `window`), so that we can mount it.
在浏览器中，将应用实例添加到全局上下文（`window`），这样我们可以挂载它；
- When in node, export a factory function so that we can create a fresh instance of the app for every request.
在 Node.js 中，导出一个工厂函数，这样可以为每个请求创建一个新的应用实例。

Accomplishing this requires a little boilerplate:
实现这个需要添加一点模版代码：

``` js
// assets/app.js
(function () { 'use strict'
  var createApp = function () {
    // ---------------------
    // BEGIN NORMAL APP CODE
    // ---------------------

    // Main Vue instance must be returned and have a root
    // node with the id "app", so that the client-side
    // version can take over once it loads.
    // ---------------------
    // 应用代码开始
    // ---------------------
    // 必须返回主 Vue 实例，并且有 id 为 "app" 的根节点，这样才能够在客户端版本加载后挂载到网页文档中。
    return new Vue({
      template: '<div id="app">你已经在这花了 {{ counter }} 秒。(You have been here for {{ counter }} seconds.)</div>',
      data: {
        counter: 0
      },
      created: function () {
        var vm = this
        setInterval(function () {
          vm.counter += 1
        }, 1000)
      }
    })

    // -------------------
    // END NORMAL APP CODE
    // -------------------
    // -------------------
    // 应用代码结束
    // -------------------
  }
  if (typeof module !== 'undefined' && module.exports) {
    module.exports = createApp
  } else {
    this.app = createApp()
  }
}).call(this)
```

Now that we have our application code, let's put together an `index.html` file:
写好了应用代码，我们接着编写 `index.html` 文件：

``` html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>My Vue App</title>
  <script src="/assets/vue.js"></script>
</head>
<body>
  <div id="app"></div>
  <script src="/assets/app.js"></script>
  <script>app.$mount('#app')</script>
</body>
</html>
```

As long as the referenced `assets` directory contains the `app.js` file we created earlier, as well as a `vue.js` file with Vue, we should now have a working single-page application!
只要引用的 `assets` 文件夹中包含了我们先前创建的 `app.js`，以及带有 Vue 的 `vue.js` 文件，我们就有了一个可以运行的单页面应用!

Then to get it working with server-side rendering, there's just one more step - the web server:
只要再完成一个步骤，我们就可以实现服务器端渲染了 —— 那就是 web 服务器：

``` js
// server.js
'use strict'

var fs = require('fs')
var path = require('path')

// Define global Vue for server-side app.js
// 服务器端的 app.js 定义全局的 Vue
global.Vue = require('vue')

// Get the HTML layout
// 获取 HTML 布局
var layout = fs.readFileSync('./index.html', 'utf8')

// Create a renderer
// 创建一个渲染器
var renderer = require('vue-server-renderer').createRenderer()

// Create an express server
// 创建一个 Express 服务器
var express = require('express')
var server = express()

// Serve files from the assets directory
// 部署 assets 目录里的静态文件
server.use('/assets', express.static(
  path.resolve(__dirname, 'assets')
))

// Handle all GET requests
// 处理所有的Get请求
server.get('*', function (request, response) {
  // Render our Vue app to a string
  // 将 Vue 应用渲染为一个字符串
  renderer.renderToString(
    // Create an app instance
    // 创建一个应用实例
    require('./assets/app')(),
    // Handle the rendered result
    // 处理渲染结果
    function (error, html) {
      // If an error occurred while rendering...
      // 如果渲染时发生了错误...
      if (error) {
        // Log the error in the console
        // 打印错误到控制台
        console.error(error)
        // Tell the client something went wrong
        // 将错误报告给客户端
        return response
          .status(500)
          .send('Server Error')
      }
      // Send the layout with the rendered app's HTML
      // 发送布局和渲染好的应用的 HTML
      response.send(layout.replace('<div id="app"></div>', html))
    }
  )
})

// Listen on port 5000
// 监听5000端口
server.listen(5000, function (error) {
  if (error) throw error
  console.log('Server is running at localhost:5000')
})
```

And that's it! Here's [the full application](https://github.com/chrisvfritz/vue-ssr-demo-simple), in case you'd like to clone it and experiment further. Once you have it running locally, you can confirm that server-side rendering really is working by right-clickig on the page and selecting `View Page Source` (or similar). You should see this in the body:
这样就完成了。完整的应用代码[在这里](https://github.com/chrisvfritz/vue-ssr-demo-simple)，你可以克隆下来进一步试验。当代码在本地运行起来后，你可以右键点击页面，选择 `显示网页源代码` （或类似操作），以此来确认服务器端渲染真的在运行。你应该能在 body 标签中看到：

``` html
<div id="app" server-rendered="true">You have been here for 0 seconds&period;</div>
```

instead of:
而不是：

``` html
<div id="app"></div>
```

## 流式响应 (Response Streaming)

Vue also supports rendering to a __stream__, which is preferred for web servers that support streaming. This allows HTML to be written to the response _as it's generated_, rather than all at once at the end. The result is requests are served faster, with no downsides!
Vue 还支持_流式_渲染，如果你的 web 服务器支持流的话，你应该优先选择它。它让你可以一边_生成 HTML_一边写入响应流，而不是在最后一次全部写入。它的效果是更快地处理请求，没有缺点！

To adapt our app from the previous section for streaming, we can simply replace the `server.get('*', ...)` block with the following:
要修改前面的代码来适应流式渲染，只要用下面的代码取代 `server.get('*', ...)` 代码块：

``` js
// Split the layout into two sections of HTML
// 把布局拆分成两段 HTML
var layoutSections = layout.split('<div id="app"></div>')
var preAppHTML = layoutSections[0]
var postAppHTML = layoutSections[1]

// Handle all GET requests
// 处理所有的Get请求
server.get('*', function (request, response) {
  // Render our Vue app to a stream
  // 把 Vue 应用渲染到流
  var stream = renderer.renderToStream(require('./assets/app')())

  // Write the pre-app HTML to the response
  // 将应用代码之前的 HTML 写入响应
  response.write(preAppHTML)

  // Whenever new chunks are rendered...
  // 每当新的块被渲染
  stream.on('data', function (chunk) {
    // Write the chunk to the response
    // 将块写入响应
    response.write(chunk)
  })

  // When all chunks are rendered...
  // 当所有的块被渲染完成...
  stream.on('end', function () {
    // Write the post-app HTML to the response
    // 将应用代码之后的 HTML 写入响应
    response.end(postAppHTML)
  })

  // If an error occurs while rendering...
  // 如果渲染时发生错误...
  stream.on('error', function (error) {
    // Log the error in the console
    // 打印错误到控制台
    console.error(error)
    // Tell the client something went wrong
    // 将错误报告给客户端
    return response
      .status(500)
      .send('Server Error')
  })
})
```

As you can see, it's not much more complicated than the previous version, even if streams may be conceptually new to you. We just:
如你所见，这段代码不会比之前的复杂太多，虽然流这个概念对你来说可能比较陌生。在这段代码里我们只是：

1. Set up the stream
建立流；
2. Write the HTML that comes before the app to the response
将应用代码之前的 HTML 写入响应；
3. Write the app HTML to the response as it becomes available
每当应用代码渲染好，就写入响应
4. Write the HTML that comes after the app to the response and end it
将应用代码之后的 HTML 写入响应，并且结束响应
5. Handle any errors
处理出现的错误

## 组件缓存 (Component Caching)

Vue's SSR is very fast by default, but you can further improve performance by caching rendered components. This should be considered an advanced feature however, as caching the wrong components (or the right components with the wrong key) could lead to misrendering your app. Specifically:
Vue 的服务器端渲染默认就非常快，但是你可以通过缓存渲染好的组件进一步提高性能。这属于进阶特性，如果缓存了错误的组件（或者用错误的键缓存了正确的组件），会导致渲染出错。特别是：

<p class="tip">You should not cache a component containing child components that rely on global state (e.g. from a vuex store). If you do, those child components (and in fact, the entire sub-tree) will be cached as well. Be especially wary with components that accept slots/children.
如果组件包含了依赖全局状态（比如一个 vuex 的 store）的子组件，那么你不应该缓存该组件。如果这么做，子组件（事实上是整个子树）也会被缓存。特别要留意接收插槽和子组件的组件。</p>

### 设置 (Setup)

With that warning out of the way, here's how you cache components.
先不管上面的提醒，我们可以用以下方式来缓存组件。

First, you'll need to provide your renderer with a [cache object](https://www.npmjs.com/package/vue-server-renderer#cache). Here's a simple example using [lru-cache](https://github.com/isaacs/node-lru-cache):
首先，你需要给渲染器提供一个[缓存对象](https://www.npmjs.com/package/vue-server-renderer#cache) 。下面的简单示例使用了 [lru-cache](https://github.com/isaacs/node-lru-cache)：

``` js
var createRenderer = require('vue-server-renderer').createRenderer
var lru = require('lru-cache')

var renderer = createRenderer({
  cache: lru(1000)
})
```

That will cache up to 1000 unique renders. For other configurations that more closely align to memory usage, see [the lru-cache options](https://github.com/isaacs/node-lru-cache#options).
这将缓存高达 1000 个独立的渲染器。如果要进一步针对内存用量来配置，请参考 [lru-cache 选项](https://github.com/isaacs/node-lru-cache#options)。

Then for components you want to cache, you must provide them with:
然后对于你想缓存的组件，你需要分别给它们：

- a unique `name`
一个唯一的 `name`
- a `serverCacheKey` function, returning a unique key scoped to the component
一个 `serverCacheKey` 函数，这个函数返回一个组件域内唯一的键

For example:
例如：

``` js
Vue.component({
  name: 'list-item',
  template: '<li>{{ item.name }}</li>',
  props: ['item'],
  serverCacheKey: function (props) {
    return props.item.type + '::' + props.item.id
  }
})
```

### 理想的缓存组件 (Ideal Components for Caching)

Any "pure" component can be safely cached - that is, any component that is guaranteed to generate the same HTML given the same props. Common examples of these include:
任何“纯”组件 —— 相同属性保证渲染出相同 HTML 的组件 —— 都可以放心缓存。一些常见的例子包括：

- Static components (i.e. they always generate the same HTML, so the `serverCacheKey` function can just return `true`)
静态组件 (它们总是生成同样的 HTML，所以 `serverCacheKey` 函数只要返回 `true` )；
- List item components (when part of large lists, caching these can significantly improve performance)
列表项组件（如果它们存在于很大的列表中，对它们进行缓存可以大大提高性能）；
- Generic UI components (e.g. buttons, alerts, etc - at least those that accept content through props rather than slots/children)
通用 UI 组件 (例如按钮，提示框等等 —— 至少是那些只通过属性接受内容，而不接受插槽/子元素的组件)。

## 构建过程，路由，和 Vuex 状态重现 (Build Process, Routing, and Vuex State Hydration)

By now, you should understand the fundamental concepts behind server-side rendering. However, as you introduce a build process, routing, and vuex, each introduces its own considerations.
现在，你应该理解服务器端渲染的基本概念了。但是，因为你引入了构建过程，路由，和 vuex，所以你还要考虑这些技术带来的问题。

To truly master server-side rendering in complex applications, we recommend a deep dive into the following resources:
要真正掌握复杂应用中的服务器端渲染，你应该深入学习以下资源：

- [vue-server-renderer docs](https://www.npmjs.com/package/vue-server-renderer#api): more details on topics covered here, as well as documentation of more advanced topics, such as [preventing cross-request contamination](https://www.npmjs.com/package/vue-server-renderer#why-use-bundlerenderer) and [adding a separate server build](https://www.npmjs.com/package/vue-server-renderer#creating-the-server-bundle)
[vue-server-renderer 文档](https://www.npmjs.com/package/vue-server-renderer#api)：这里有相关主题的更多细节，以及进阶主题的文档，例如 [防止跨站请求攻击](https://www.npmjs.com/package/vue-server-renderer#why-use-bundlerenderer) 和 [添加独立的服务器构建版本](https://www.npmjs.com/package/vue-server-renderer#creating-the-server-bundle)；
- [vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0): the definitive example of integrating all major Vue libraries and concepts in a single application
[vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0)：在一个应用中整合所有主要的 Vue 库和概念的权威示例。
