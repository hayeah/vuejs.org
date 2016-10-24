---
title: Server-Side Rendering
type: guide
order: 23
---

## Do You Need SSR?
## 你需要服务端渲染（SSR）吗？

Before diving into SSR, let's explore what it actually does for you and when you might need it.
在开始服务端渲染前，我们先看看它能给我们带来什么，以及什么时候需要用它。

### SEO （搜索引擎优化）

Google and Bing can index synchronous JavaScript applications just fine. _Synchronous_ being the key word there. If your app starts with a loading spinner, then fetches content via Ajax, the crawler will not wait for you to finish.
谷歌和Bing可以很好地索引同步的JavaScript应用。_同步_ 在这里是个关键词。如果应用启动时有一个加载动画，然后内容通过ajax获取，那爬虫不会等待他们加载完成。

This means if you have content fetched asynchronously on pages where SEO is important, SSR might be necessary.
这意味着在异步获取内容的页面上很需要进行搜索引擎优化的时候，服务端渲染就很重要。

### Clients with a Slow Internet
### 客户端的网络比较慢

Users might come to your site from a remote area with slow Internet - or just with a bad cell connection. In these cases, you'll want to minimize the number and size of requests necessary for users to see basic content.
用户可能在网络比较慢的情况下从远处访问网站 - 或者通过比较差的带宽。 这些情况下，尽量减少页面请求数量，来保证用户尽快看到基本的内容。

You can use [Webpack's code splitting](https://webpack.github.io/docs/code-splitting.html) to avoid forcing users to download your entire application to view a single page, but it still won't be as performant as downloading a single, pre-rendered HTML file.
可以用 [Webpack的代码拆分](https://webpack.github.io/docs/code-splitting.html) 避免强制用户下载整个单页面应用，但是，这样也远没有下载个单独的预先渲染过的HTML文件性能高。

### Clients with an Old (or Simply No) JavaScript Engine
### 客户端运行在老的(或者直接没有)JavaScript引擎上

For some demographics or areas of the world, using a computer from 1998 to access the Internet might be the only option. While Vue only works with IE9+, you may still want to deliver basic content to those on older browsers - or to hipster hackers using [Lynx](http://lynx.browser.org/) in the terminal.
对于世界上的一些地区人，可能只能用1998年产的电脑访问互联网的方式使用计算机。而Vue只能运行在IE9以上的浏览器，你可以也想为那些老式浏览器提供基础内容 - 或者是在命令行中使用 [Lynx](http://lynx.browser.org/) 的时髦的黑客。

### SSR vs Prerendering
### 服务端渲染（SSR） 对比 预渲染（Prerendering）

If you're only investigating SSR to improve the SEO of a handful of marketing pages (e.g. `/`, `/about`, `/contact`, etc), then you probably want __prerendering__ instead. Rather than using a web server to compile HTML on-the-fly, prerendering simply generates static HTML files for specific routes at build time. The advantage is setting up prerendering is much simpler and allows you to keep your frontend as a fully static site.
如果你只是用服务端渲染来改善一个少数的营销页面（如 首页，关于，联系 等等）的SEO，那你可以用_预渲染_替换。预渲染不像服务器渲染那样即时编译HTML,预渲染只是在构建时为了特定的路由生成特定的几个静态页面。其优势是预渲染的设置更加简单，可以保持前端是一个完整的静态站。

If you're using Webpack, you can easily add prerendering with the [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin). It's been extensively tested with Vue apps - and in fact, the creator is a member of the Vue core team.
你用webpack可以很简单地通过 [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin) 来添加预渲染，它被广泛地用在Vue应用上 - 事实上，创建者也是Vue核心团队成员之一。

## Hello World

If you've gotten this far, you're ready to see SSR in action. It sounds complex, but a simple node script demoing the feature requires only 3 steps:
准备在行动中体验服务端渲染吧。服务端渲染(即SSR)听起来很复杂，不过一个简单的Node脚本只需要3步就可以实现这个功能：

``` js
// Step 1: Create a Vue instance
var Vue = require('vue')
var app = new Vue({
  render: function (h) {
    return h('p', 'hello world')
  }
})

// Step 2: Create a renderer
var renderer = require('vue-server-renderer').createRenderer()

// Step 3: Render the Vue instance to HTML
renderer.renderToString(app, function (error, html) {
  if (error) throw error
  console.log(html)
  // => <p server-rendered="true">hello world</p>
})
```
``` js
// 步骤 1:创建一个Vue实例
var Vue = require('vue')
var app = new Vue({
  render: function (h) {
    return h('p', 'hello world')
  }
})

// 步骤 2: 创建一个渲染器
var renderer = require('vue-server-renderer').createRenderer()

// 步骤 3: 将 Vue实例 渲染成 HTML
renderer.renderToString(app, function (error, html) {
  if (error) throw error
  console.log(html)
  // => <p server-rendered="true">hello world</p>
})
```

Not so scary, right? Of course, this example is much simpler than most applications. We don't yet have to worry about:
这并不困难。当然这个示例比大部分应用都简单。我们不必担心：

- A Web Server
- Response Streaming
- Component Caching
- A Build Process
- Routing
- Vuex State Hydration

- 一个Web服务器
- 流式响应
- 组件缓存
- 一个构建过程
- 路由
- Vuex状态管理

In the rest of this guide, we'll walk through how to work with some of these features. Once you understand the basics, we'll then direct you to more detailed documentation and advanced examples to help you handle edge cases.
这个指南的其余部分，我们将探讨这些功能怎样运作。一旦你理解了基础，我们会提供更多细节和进一步的示例来帮助你解决意外情况。

## Simple SSR with the Express Web Server
## 通过Express Web服务器实现简单的服务端渲染

It's kind of a stretch to call it "server-side rendering" when we don't actually have a web server, so let's fix that. We'll build a very simple SSR app, using only ES5 and without any build step or Vue plugins.
如果没有一个Web服务器，很难说是“服务端渲染”，所以我们来补充它。我们将构建一个非常简单的服务端渲染应用，只用ES5，也不带其他构建步骤或Vue插件。

We'll start off with an app that just tells the user how many seconds they've been on the page:
启动一个应用告诉用户他们在一个页面上花了多少时间：

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
为了适应服务端渲染，我们需要进行一些修改，让它可以在浏览器和Node中渲染：

- When in the browser, add an instance of our app to the global context (i.e. `window`), so that we can mount it.
- When in node, export a factory function so that we can create a fresh instance of the app for every request.

- 在浏览器中，将我们的应用实例添加到全局上下文（window）上,我们可以安装它。
- 在Node中，导出一个工厂函数让我们可以为每个请求创建应用实例。

Accomplishing this requires a little boilerplate:
实现这个需要一点模板：

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
    return new Vue({
      template: '<div id="app">You have been here for {{ counter }} seconds.</div>',
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
  }
  if (typeof module !== 'undefined' && module.exports) {
    module.exports = createApp
  } else {
    this.app = createApp()
  }
}).call(this)
```
``` js
// assets/app.js
(function () { 'use strict'
  var createApp = function () {
    // ---------------------
    // 开始常用的应用代码
    // ---------------------
    // 主要的Vue实例必须返回，并且有一个根节点在id "app"上，这样客户端可以加载它。
    return new Vue({
      template: '<div id="app">你已经在这花了 {{ counter }} 秒。</div>',
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
    // 结束常用的应用代码
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
现在有了应用代码，接着加一个 `index.html` 文件。

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
主要引用 `assets` 文件夹中我们先前创建的 `app.js`，以及 `vue.js` 文件，我们就有了一个可以运行的单页面应用!

Then to get it working with server-side rendering, there's just one more step - the web server:
然后为了实现服务端渲染，在服务端需要加一个步骤：

``` js
// server.js
'use strict'

var fs = require('fs')
var path = require('path')

// Define global Vue for server-side app.js
global.Vue = require('vue')

// Get the HTML layout
var layout = fs.readFileSync('./index.html', 'utf8')

// Create a renderer
var renderer = require('vue-server-renderer').createRenderer()

// Create an express server
var express = require('express')
var server = express()

// Serve files from the assets directory
server.use('/assets', express.static(
  path.resolve(__dirname, 'assets')
))

// Handle all GET requests
server.get('*', function (request, response) {
  // Render our Vue app to a string
  renderer.renderToString(
    // Create an app instance
    require('./assets/app')(),
    // Handle the rendered result
    function (error, html) {
      // If an error occurred while rendering...
      if (error) {
        // Log the error in the console
        console.error(error)
        // Tell the client something went wrong
        return response
          .status(500)
          .send('Server Error')
      }
      // Send the layout with the rendered app's HTML
      response.send(layout.replace('<div id="app"></div>', html))
    }
  )
})

// Listen on port 5000
server.listen(5000, function (error) {
  if (error) throw error
  console.log('Server is running at localhost:5000')
})
```
``` js
// server.js
'use strict'

var fs = require('fs')
var path = require('path')

// 定义全局的Vue为了服务端的app.js
global.Vue = require('vue')

// 获取HTML布局
var layout = fs.readFileSync('./index.html', 'utf8')

// 创建一个渲染器
var renderer = require('vue-server-renderer').createRenderer()

// 创建一个Express服务器
var express = require('express')
var server = express()

// 部署静态文件夹为 "assets" 文件夹
server.use('/assets', express.static(
  path.resolve(__dirname, 'assets')
))

// 处理所有的Get请求
server.get('*', function (request, response) {
  // 渲染我们的Vue应用为一个字符串
  renderer.renderToString(
    // 创建一个应用实例
    require('./assets/app')(),
    // 处理渲染结果
    function (error, html) {
      // 如果渲染时发生了错误...
      if (error) {
        // 打印错误到控制台
        console.error(error)
        // 告诉客户端错误
        return response
          .status(500)
          .send('Server Error')
      }
      // // 发送布局和HTML文件
      response.send(layout.replace('<div id="app"></div>', html))
    }
  )
})

// 监听5000端口
server.listen(5000, function (error) {
  if (error) throw error
  console.log('Server is running at localhost:5000')
})
```

And that's it! Here's [the full application](https://github.com/chrisvfritz/vue-ssr-demo-simple), in case you'd like to clone it and experiment further. Once you have it running locally, you can confirm that server-side rendering really is working by right-clickig on the page and selecting `View Page Source` (or similar). You should see this in the body:
这样就完成了。[完整示例](https://github.com/chrisvfritz/vue-ssr-demo-simple)，克隆下来深度实验。一旦它在本地运行时，你可以通过在页面右击选择 `页面资源` （或类似操作）确认服务选渲染真的运行了。可以在body中看到：

``` html
<div id="app" server-rendered="true">You have been here for 0 seconds&period;</div>
```

instead of:

``` html
<div id="app"></div>
```

## Response Streaming
## 流式响应

Vue also supports rendering to a __stream__, which is preferred for web servers that support streaming. This allows HTML to be written to the response _as it's generated_, rather than all at once at the end. The result is requests are served faster, with no downsides!
Vue还支持_流式_渲染，优先选择适用于支持流的Web服务器。允许HTML一边_生成_一边写入相应流，而不是在最后一次全部写入。其结果是请求服务速度更快，没有缺点！

To adapt our app from the previous section for streaming, we can simply replace the `server.get('*', ...)` block with the following:
为了使上一节应用代码适用流式渲染，可以简单的替换 `server.get('*', ...)` 为下面的代码：

``` js
// Split the layout into two sections of HTML
var layoutSections = layout.split('<div id="app"></div>')
var preAppHTML = layoutSections[0]
var postAppHTML = layoutSections[1]

// Handle all GET requests
server.get('*', function (request, response) {
  // Render our Vue app to a stream
  var stream = renderer.renderToStream(require('./assets/app')())

  // Write the pre-app HTML to the response
  response.write(preAppHTML)

  // Whenever new chunks are rendered...
  stream.on('data', function (chunk) {
    // Write the chunk to the response
    response.write(chunk)
  })

  // When all chunks are rendered...
  stream.on('end', function () {
    // Write the post-app HTML to the response
    response.end(postAppHTML)
  })

  // If an error occurs while rendering...
  stream.on('error', function (error) {
    // Log the error in the console
    console.error(error)
    // Tell the client something went wrong
    return response
      .status(500)
      .send('Server Error')
  })
})
```
``` js
// 拆分布局成两段HTML
var layoutSections = layout.split('<div id="app"></div>')
var preAppHTML = layoutSections[0]
var postAppHTML = layoutSections[1]

// 处理所有的Get请求
server.get('*', function (request, response) {
  // 渲染我们的Vue实例作为流
  var stream = renderer.renderToStream(require('./assets/app')())

  // 将预先的HTML写入响应
  response.write(preAppHTML)

  // 每当新的块被渲染
  stream.on('data', function (chunk) {
    // 将块写入响应
    response.write(chunk)
  })

  // 当所有的块被渲染完成...
  stream.on('end', function () {
    // 将post-app HTML写入响应
    response.end(postAppHTML)
  })

  // 当渲染时发生错误...
  stream.on('error', function (error) {
    // 打印错误到控制台
    console.error(error)
    // 告诉客服端发生了错误
    return response
      .status(500)
      .send('Server Error')
  })
})
```

As you can see, it's not much more complicated than the previous version, even if streams may be conceptually new to you. We just:
正如您可以看到的，这不比之前的版本复杂，甚至这对你来说都不是个新概念。我们仅仅做了：

1. Set up the stream
2. Write the HTML that comes before the app to the response
3. Write the app HTML to the response as it becomes available
4. Write the HTML that comes after the app to the response and end it
5. Handle any errors

1. 建立流
2. 在应用响应前写入HTML
3. 在可获得时将应用HTML写入响应
4. 在响应最后写入HTML
5. 处理任何错误

## Component Caching
## 组件缓存

Vue's SSR is very fast by default, but you can further improve performance by caching rendered components. This should be considered an advanced feature however, as caching the wrong components (or the right components with the wrong key) could lead to misrendering your app. Specifically:
Vue的服务端渲染默认非常快，但是你可以通过缓存渲染好的组件进一步提高性能。这被认为是一种先进的功能，但是，如果缓存了错误的组件（或者正确的组件带有错误的内容）将导致应用渲染出错。特别注意：

<p class="tip">You should not cache a component containing child components that rely on global state (e.g. from a vuex store). If you do, those child components (and in fact, the entire sub-tree) will be cached as well. Be especially wary with components that accept slots/children.</p>
<p class="tip">不应该缓存组件包含子组件依赖全局状态（例如来自vuex的状态）。如果这么做，子组件（事实上是整个子树）也会被缓存。所以要特别注意带有slots片段或者子组件的情况。</p>

### Setup
### 设置

With that warning out of the way, here's how you cache components.
在警告情况之外的，我们可以用下面的方法缓存组件。

First, you'll need to provide your renderer with a [cache object](https://www.npmjs.com/package/vue-server-renderer#cache). Here's a simple example using [lru-cache](https://github.com/isaacs/node-lru-cache):
首先，你需要提供给渲染器一个 [缓存对象](https://www.npmjs.com/package/vue-server-renderer#cache) 对象。这有个简单的示例使用 [lru-cache](https://github.com/isaacs/node-lru-cache)：

``` js
var createRenderer = require('vue-server-renderer').createRenderer
var lru = require('lru-cache')

var renderer = createRenderer({
  cache: lru(1000)
})
```

That will cache up to 1000 unique renders. For other configurations that more closely align to memory usage, see [the lru-cache options](https://github.com/isaacs/node-lru-cache#options).
这将缓存高达1000个独立的渲染。对于更进一步缓存到内容中的配置，看 [lru-cache 设置](https://github.com/isaacs/node-lru-cache#options)。

Then for components you want to cache, you must provide them with:
然后对于你想缓存的组件，你可以为他们提供：

- a unique `name`
- a `serverCacheKey` function, returning a unique key scoped to the component

- 一个唯一的 `name`
- 一个 `serverCacheKey` 函数，返回一个唯一的组件作用域

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

### Ideal Components for Caching
### 理想状态下的组件缓存

Any "pure" component can be safely cached - that is, any component that is guaranteed to generate the same HTML given the same props. Common examples of these include:
任何纯组件可以被安全缓存 - 这是保证给任何组件传递一样的数据产生相同的HTML。这些场景的例子包括：

- Static components (i.e. they always generate the same HTML, so the `serverCacheKey` function can just return `true`)
- List item components (when part of large lists, caching these can significantly improve performance)
- Generic UI components (e.g. buttons, alerts, etc - at least those that accept content through props rather than slots/children)

- 静态的组件 (例如 总是尝试一样的HTML,所以 `serverCacheKey` 函数可以被返回 `true` )
- 列表组件（当有大量列表，缓存他们可以改善性能）
- 通用UI组件 (例如 buttons, alerts, 等等 - 至少他们通过props获取数据而不是 slots或者子组件)

## Build Process, Routing, and Vuex State Hydration
## 构建过程，路由，和Vuex状态管理

By now, you should understand the fundamental concepts behind server-side rendering. However, as you introduce a build process, routing, and vuex, each introduces its own considerations.
现在，应该理解服务端渲染背后的基本概念了。但是，构建过程、路由、Vuex每一个都有自己的注意事项。

To truly master server-side rendering in complex applications, we recommend a deep dive into the following resources:
要真正掌握复杂应用下的服务端渲染，我们推荐深度熟悉以下资源：

- [vue-server-renderer docs](https://www.npmjs.com/package/vue-server-renderer#api): more details on topics covered here, as well as documentation of more advanced topics, such as [preventing cross-request contamination](https://www.npmjs.com/package/vue-server-renderer#why-use-bundlerenderer) and [adding a separate server build](https://www.npmjs.com/package/vue-server-renderer#creating-the-server-bundle)
- [vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0): the definitive example of integrating all major Vue libraries and concepts in a single application

- [vue-server-renderer 文档](https://www.npmjs.com/package/vue-server-renderer#api) 文档:更多细节在这里，和更多先进的主题一起的文档。 例如 [防止跨站请求攻击](https://www.npmjs.com/package/vue-server-renderer#why-use-bundlerenderer) 和 [添加独立的服务构建](https://www.npmjs.com/package/vue-server-renderer#creating-the-server-bundle)
- [vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0): 明确整合了 所有主要的Vue库和概念在单个应用中
