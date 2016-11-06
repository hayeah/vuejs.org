---
title: 简介 (Introduction)
type: guide
order: 2
---

## 什么是 Vue.js ？ (What is Vue.js?)

Vue (pronounced /vjuː/, like **view**) is a **progressive framework** for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is very easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with [modern tooling](single-file-components.html) and [supporting libraries](https://github.com/vuejs/awesome-vue#libraries--plugins).
Vue（读作 /vju:/，跟 **view** 同音）是一个用于构建用户界面的**渐进式框架**。与其它庞大的框架不同，Vue 的设计完全就是为了让开发者可以增量式使用。Vue 的核心库专注于视图层，上手简单，开发者可以很容易地将它集成到其它库或者已有的项目中。另一方面，Vue 如果结合[现代化的工具](single-file-components.html)以及[支持库](https://github.com/vuejs/awesome-vue#libraries--plugins)一起使用，完全有能力支撑复杂的单页应用。

If you are an experienced frontend developer and want to know how Vue compares to other libraries/frameworks, check out the [Comparison with Other Frameworks](comparison.html).
如果你是资深的前端开发者，想了解 Vue 和其它框架或库的异同，可以查看教程的[与其它框架对比](comparison.html)一节。

## 上手 (Getting Started)

<p class="tip">The official guide assumes intermediate level front-end knowledge of HTML, CSS and JavaScript. If you are totally new to front-end development, it might not be the best idea to jump right into a framework as your first step - grasp the basics then come back! Prior experience with other frameworks helps, but is not required.<br>这个官方教程会假设你已经拥有 HTML，CSS 和 JavaScript 的中级水平。如果你是一个没有太多经验的前端开发者，你不应该选一个像 Vue 这样的框架来入门——先去学习基础知识，然后再回来吧！如果你有其它框架的使用经验，它们会有帮助，但对于学习 Vue 来说不是必需的。</p>

The easiest way to try out Vue.js is using the [JSFiddle Hello World example](https://jsfiddle.net/chrisvfritz/50wL7mdz/). Feel free to open it in another tab and follow along as we go through some basic examples. Or, you can simply create an `.html` file and include Vue with:
要尝试 Vue.js，最简单的方式就是使用 [JSFiddle 的 Hello World 示例](https://jsfiddle.net/chrisvfritz/50wL7mdz/)。你可以在另一个标签页打开这个页面，然后跟着本教程一步一步地学习一些基本示例。或者，你也可以简单地创建一个包含 Vue 的 `.html` 文件：

``` html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

The [Installation](installation.html) page provides more options of installing Vue. Note that we **do not** recommend beginners to start with `vue-cli`, especially if you are not yet familiar with Node.js-based build tools.
教程的[安装](installation.html)一节提供了更多安装 Vue 的选项。要注意，我们**不**推荐初学者使用 `vue-cli`，特别是那些不熟悉 Node.js 构建工具的开发者。

## 声明式渲染 (Declarative Rendering)

At the core of Vue.js is a system that enables us to declaratively render data to the DOM using straightforward template syntax:
Vue.js 的核心系统，让你可以用直观的模版语法，声明式地渲染数据：

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js'
  }
})
</script>
{% endraw %}

We have already created our very first Vue app! This looks pretty similar to just rendering a string template, but Vue has done a lot of work under the hood. The data and the DOM are now linked, and everything is now **reactive**. How do we know? Just open up your browser's JavaScript console and set `app.message` to a different value. You should see the rendered example above update accordingly.
这样就创建了我们的第一个 Vue 应用。虽然看起来只是渲染了一个字符串模版，不过 Vue 在底层做了很多工作。现在，Vue 已经将数据和 DOM 绑定到一起，所有数据都是**响应式**的。你怎么知道？打开你浏览器的 JavaScritp 控制台，然后将 `app.message` 设成另一个值。你应该能看到上面渲染的例子也相应地改变。

In addition to text interpolation, we can also bind element attributes like this:
除了通过文本插值的方式，我们也可以像这样绑定元素属性：

``` html
<div id="app-2">
  <span v-bind:title="message">
    鼠标在这里悬停几秒，你就能看到动态绑定的标题了。
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '加载这个页面的时间是：' + new Date()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    鼠标在这里悬停几秒，你就能看到动态绑定的标题了。
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '加载这个页面的时间是：' + new Date()
  }
})
</script>
{% endraw %}

Here we are encountering something new. The `v-bind` attribute you are seeing is called a **directive**. Directives are prefixed with `v-` to indicate that they are special attributes provided by Vue, and as you may have guessed, they apply special reactive behavior to the rendered DOM. Here it is basically saying "keep this element's `title` attribute up-to-date with the `message` property on the Vue instance."
这里我们碰到一个新概念。我们把这个 `v-bind` 属性称作一个**指令**。指令都有一个 `v-` 的前缀，表明它是 Vue 的特殊属性。而且，正如你可能猜测的那样，指令会将特殊的响应式行为应用到 DOM 上。这里的代码可以大致理解为，将元素的 `title` 属性与 Vue 实例的 `message` 属性绑定到一起。

If you open up your JavaScript console again and enter `app2.message = 'some new message'`, you'll once again see that the bound HTML - in this case the `title` attribute - has been updated.
如果你再次打开 JavaScript 控制台，然后输入 `app2.message = 'some new message'`，你会发现元素的 `title` 属性立刻更新。

## 条件和循环 (Conditionals and Loops)

It's quite simple to toggle the presence of an element, too:
要显示或隐藏一个元素也是很简单的：

``` html
<div id="app-3">
  <p v-if="seen">你现在能看到这个元素</p>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <p v-if="seen">你现在能看到这个元素</p>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Go ahead and enter `app3.seen = false` in the console. You should see the message disappear.
打开控制台输入 `app3.seen = false`。你会看到文本消失了。

This example demonstrates that we can bind data to not only text and attributes, but also the **structure** of the DOM. Moreover, Vue also provides a powerful transition effect system that can automatically apply [transition effects](transitions.html) when elements are inserted/updated/removed by Vue.
从这个例子看出，我们不只能够将数据绑定到文本和属性，还可以绑定到 DOM 的**结构**。而且，Vue 还提供了一个强大的过渡动画系统，在插入、更新或者删除元素时自动应用[过渡动画](transitions.html)。

There are quite a few other directives, each with its own special functionality. For example, the `v-for` directive can be used for displaying a list of items using the data from an Array:
还有好几个别的指令，每一个都有特殊的功能。例如，`v-for` 指令可以将数组显示成一个列表：

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '写一个很棒的应用' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '写一个很棒的应用' }
    ]
  }
})
</script>
{% endraw %}

In the console, enter `app4.todos.push({ text: 'New item' })`. You should see a new item appended to the list.
在控制台里输入 `app4.todos.push({ text: '新的列表项' })`。你会看到列表的最后多了一个新的列表项。

## 处理用户输入 (Handling User Input)

To let users interact with your app, we can use the `v-on` directive to attach event listeners that invoke methods on our Vue instances:
要让用户跟你的应用交互，我们可以使用 `v-on` 指令来添加事件侦听器，它们会调用 Vue 实例的方法：

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">将信息逆序打印</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">将信息逆序打印</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Note in the method we simply update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.
要注意，我们只是在方法里更新了应用的状态，而没有去修改 DOM——Vue 会处理所有的 DOM 操作，而你的代码只需要负责应用的逻辑。

Vue also provides the `v-model` directive that makes two-way binding between form input and app state a breeze:
Vue 还提供了 `v-model` 指令，它可以轻松地在输入控件和应用状态之间创造一个双向绑定：

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## 将组件组合起来 (Composing with Components)

The component system is another important concept in Vue, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:
组件系统是 Vue 的另一个重要概念，因为它提供了一个抽象，让我们可以通过组合小型的、自包含的、通常是可复用的组件来构建大型应用。仔细想想，几乎所有类型的应用界面都可以被抽象成组件树：

![Component Tree](/images/components.png)

In Vue, a component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:
在 Vue 中，组件就是带有预设选项的 Vue 实例。在 Vue 中注册组件是很直观的：

``` js
// Define a new component called todo-item
// 定义一个叫做 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是一个 todo</li>'
})
```

Now you can compose it in another component's template:
现在你可以在另一个组件的模版里使用 `todo-item`：

``` html
<ul>
  <!-- Create an instance of the todo-item component -->
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ul>
```

But this would render the same text for every todo, which is not super interesting. We should be able to pass data from the parent scope into child components. Let's modify the component definition to make it accept a [prop](components.html#Props):
不过这会将每一个 todo 都渲染成同样的文本，看起来不是一个特别有趣的应用场景。我们应该能够将数据从父级作用域传递到子组件。让我们修改一下组件的定义，让它接收一个[属性](components.html#Props)：

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  // todo-item 组件现在会接收一个属性，类似一个自定义属性。
  // 我们将这个属性命名为 todo。
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Now we can pass the todo into each repeated component using `v-bind`:
现在我们可以用 `v-bind` 来将 todo 传递给每一个重复组件：

``` html
<div id="app-7">
  <ol>
    <!-- Now we provide each todo-item with the todo object    -->
    <!-- it's representing, so that its content can be dynamic -->
    <!-- 现在我们给每一个 todo-item 一个 todo 对象，这样 todo-item 的内容就是动态的了 -->
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: '蔬菜' },
      { text: '芝士' },
      { text: '其它食物' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: '蔬菜' },
      { text: '芝士' },
      { text: '其它食物' }
    ]
  }
})
</script>
{% endraw %}

This is just a contrived example, but we have managed to separate our app into two smaller units, and the child is reasonably well-decoupled from the parent via the props interface. We can now further improve our `<todo-item>` component with more complex template and logic without affecting the parent app.
这只是一个虚构的示例，不过我们已经成功地将应用分离成两个小的单元，而子元素也通过属性接口，很好地与父组件分离开来。我们可以进一步完善 `<todo-item>` 部件，使用更复杂的模版和逻辑，而不需要担心对父组件产生影响。

In a large application, it is necessary to divide the whole app into components to make development manageable. We will talk a lot more about components [later in the guide](components.html), but here's an (imaginary) example of what an app's template might look like with components:
在一个大型应用中，要提高开发的可控性，将整个应用拆分成小的组件是非常必要的。我们会在教程[后面的章节](components.html)对组件进行详细讲解。下面是一个虚构的示例，它是一个使用了组件的应用模版：

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### 与自定义元素的关系 (Relation to Custom Elements)

You may have noticed that Vue components are very similar to **Custom Elements**, which are part of the [Web Components Spec](http://www.w3.org/wiki/WebComponents/). That's because Vue's component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:
你可能已经注意到，Vue 组件和 [Web Components 标准](http://www.w3.org/wiki/WebComponents/)中的**自定义组件**非常相似。这是因为，Vue 的组件语法参考了这份标准。例如，Vue 组件实现了 [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，还有 `is` 特殊属性。不过，Vue 组件和自定义组件有一些关键的差异：

1. The Web Components Spec is still in draft status, and is not natively implemented in every browser. In comparison, Vue components don't require any polyfills and work consistently in all supported browsers (IE9 and above). When needed, Vue components can also be wrapped inside a native custom element.
Web Component 标准依然处于草案状态，并不是所有的浏览器都原生实现了这个标准。而 Vue 并不需要任何的 polyfill 就可以在所有 IE9+ 的浏览器上运行。必要时，Vue 组件也可以包裹在原生的自定义元素中。

2. Vue components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and build tool integrations.
Vue 组件提供了普通自定义组件所没有的重要特性，包括部组件间数据流，自定义事件通信，以及构建工具集成。

## 想了解更多？(Ready for More?)

We've just briefly introduced the most basic features of Vue.js core - the rest of this guide will cover them and other advanced features with much finer details, so make sure to read through it all!
这篇文章只是简单介绍了 Vue.js 内核的基本特性，教程的其余部分会用更详尽的篇幅来解释基本特性和高级特性，请务必完整地阅读！
