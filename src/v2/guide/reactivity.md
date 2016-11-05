---
title: 深入响应式原理 (Reactivity in Depth)
type: guide
order: 12
---

We've covered most of the basics - now it's time to take a deep dive! One of Vue's most distinct features is the unobtrusive reactivity system. Models are just plain JavaScript objects. When you modify them, the view updates. It makes state management very simple and intuitive, but it's also important to understand how it works to avoid some common gotchas. In this section, we are going to dig into some of the lower-level details of Vue's reactivity system.
前面的章节中，我们已经覆盖了大部分基础内容，是时候来点有深度的了。Vue 最具特色的一个功能是响应式系统。模型只作为普通对象存在。当你修改它们，视图会随之更新。这让状态管理非常简单且直观，不过，理解它的工作原理对于避免踩一些常见的坑来说，也是很重要的。在本节中，我们将探究一些 Vue 的响应式系统的底层细节。

## 如何追踪变化 (How Changes Are Tracked)

When you pass a plain JavaScript object to a Vue instance as its `data` option, Vue will walk through all of its properties and convert them to getter/setters using [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty). This is an ES5-only and un-shimmable feature, which is why Vue doesn't support IE8 and below.
把一个普通对象传给 Vue 实例作为它的 `data` 选项，Vue 将遍历它的属性，并通过 [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 将它们转为 getter/setter。这是 ES5 特性，不能通过打补丁实现，这也是为什么 Vue 不支持 IE8 及更低版本。

The getter/setters are invisible to the user, but under the hood they enable Vue to perform dependency-tracking and change-notification when properties are accessed or modified. One caveat is that browser consoles format getter/setters differently when converted data objects are logged, so you may want to install [vue-devtools](https://github.com/vuejs/vue-devtools) for a more inspection-friendly interface.
getter/setters 对用户来说是透明的，但是它们让 Vue 有机会追踪依赖，并在属性被访问和修改时得到更变通知。一个需要注意的点是，在浏览器控制台打印数据对象时对于 getter/setter 的格式处理方式不尽相同，你可以考虑使用 [vue-devtools](https://github.com/vuejs/vue-devtools) 来得到更为方便追踪数据（inspection-friendly）的界面。

Every component instance has a corresponding **watcher** instance, which records any properties "touched" during the component's render as dependencies. Later on when a dependency's setter is triggered, it notifies the watcher, which in turn causes the component to re-render.
每个组件实例拥有一个对应的 **watcher** 实例，这个 **watcher** 会记录任何在组件渲染时使用到的属性作为依赖。之后，当依赖属性的 setter 被调用时，会通知到 watcher，从而触发组件更新。

![Reactivity Cycle](/images/data.png)

## 变化检测注意事项 (Change Detection Caveats)

Due to the limitations of modern JavaScript (and the abandonment of `Object.observe`), Vue **cannot detect property addition or deletion**. Since Vue performs the getter/setter conversion process during instance initialization, a property must be present in the `data` object in order for Vue to convert it and make it reactive. For example:
受现代 JavaScript （以及 `Object.observe` 方法的废弃） 的限制，Vue **无法检测到对象属性的添加或删除**。Vue 是在实例初始化过程中执行 getter/setter 的转换，因此，只有在 data 对象中的属性，才能保证被 Vue 转换为响应式的（reactive）。例如：

``` js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` is now reactive
// `vm.a` 是响应式的

vm.b = 2
// `vm.b` is NOT reactive
// `vm.b` 不是响应式的
```

Vue does not allow dynamically adding new root-level reactive properties to an already created instance. However, it's possible to add reactive properties to a nested object using the `Vue.set(object, key, value)` method:
Vue 不允许动态添加新的根级（root-level）响应式属性（reactive properties）到已存在的实例中。但是，可以通过 `Vue.set(object, key, value)` 方法添加响应式的属性到一个嵌套对象中：

``` js
Vue.set(vm.someObject, 'b', 2)
```

You can also use the `vm.$set` instance method, which is just an alias to the global `Vue.set`:
你也可以使用 `vm.$set` 这个实例方法，它仅仅是全局方法 `Vue.set` 的别名：

``` js
this.$set(this.someObject, 'b', 2)
```

Sometimes you may want to assign a number of properties to an existing object, for example using `Object.assign()` or `_.extend()`. However, new properties added to the object will not trigger changes. In such cases, create a fresh object with properties from both the original object and the mixin object:
有些时候你可能希望添加多个属性到一个已存在的对象中，例如使用 `Object.assign()` 或者 `_.extend()`。然而，新增的属性并不能被触发更新。在这种情况下，应该使用原始对象和新增属性创建一个新的对象：

``` js
// instead of `Object.assign(this.someObject, { a: 1, b: 2 })`
// 替代 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

There are also a few array-related caveats, which were discussed earlier in the [list rendering section](list.html#Caveats).
还有一些数组相关的问题，之前已经在[列表渲染章节](list.html#Caveats)中讨论过了。

## 声明响应式属性 (Declaring Reactive Properties)

Since Vue doesn't allow dynamically adding root-level reactive properties, you have to initialize Vue instances by declaring all root-level reactive data properties upfront, even just with an empty value:
由于 Vue 不允许动态添加根级响应式属性，你必须在初始化 Vue 实例时，事先声明所有根级响应式属性，即使该属性的值为空：

```js
var vm = new Vue({
  data: {
    // declare message with an empty value
    // 用空字符串声明 message
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// set `message` later
// 后续设置 `message` 的值
vm.message = 'Hello!'

```

If you don't declare `message` in the data option, Vue will warn you that the render function is trying to access a property that doesn't exist.
如果你没有在 data 选项中声明 `message`，Vue 会发出警告：render 函数尝试访问一个不存在的属性。

There are technical reasons behind this restriction - it eliminates a class of edge cases in the dependency tracking system, and also makes Vue instances play nicer with type checking systems. But there is also an important consideration in terms of code maintainability: the `data` object is like the schema for your component's state. Declaring all reactive properties upfront makes the component code easier to understand when revisited later or read by another developer.
这个限制是出于技术角度的考量——这样设计消除了依赖追踪系统中一些边缘情况，同时也使 Vue 实例更好的与类型检查系统协作。除此之外还有一个出于代码维护方面的重要考虑：`data` 被视作组件状态的结构描述（schema）。事先声明所有响应式属性可以让组件代码在之后重读或者他人阅读时更易于理解。

## 异步更新队列 (Async Update Queue)

In case you haven't noticed yet, Vue performs DOM updates **asynchronously**. Whenever a data change is observed, it will open a queue and buffer all the data changes that happen in the same event loop. If the same watcher is triggered multiple times, it will be pushed into the queue only once. This buffered de-duplication is important in avoiding unnecessary calculations and DOM manipulations. Then, in the next event loop "tick", Vue flushes the queue and performs the actual (already de-duped) work. Internally Vue tries native `Promise.then` and `MutationObserver` for the asynchronous queuing and falls back to `setTimeout(fn, 0)`.
如果你还没有注意到，Vue 是**异步**执行 DOM 更新的。当数据更变被观察到时，它会开启一个队列并缓存相同事件循环中所有的数据更变到该队列中。如果相同的 watcher 被触发了多次，也只会被推到队列中一次。这个缓冲去重（buffered de-duplication）机制对于避免重复计算和多余的 DOM 操作来说是至关重要的。之后，在下个事件循环帧中，Vue 刷新队列并执行实际的（已被去重的）工作。在内部优先使用 `Promise.then` 和 `MutationObserver` 来实现异步队列，如果不支持则使用 setTimeout(fn, 0)。

For example, when you set `vm.someData = 'new value'`, the component will not re-render immediately. It will update in the next "tick", when the queue is flushed. Most of the time we don't need to care about this, but it can be tricky when you want to do something that depends on the post-update DOM state. Although Vue.js generally encourages developers to think in a "data-driven" fashion and avoid touching the DOM directly, sometimes it might be necessary to get your hands dirty. In order to wait until Vue.js has finished updating the DOM after a data change, you can use `Vue.nextTick(callback)` immediately after the data is changed. The callback will be called after the DOM has been updated. For example:
例如，当你设置 `vm.someData = 'new value'` 时，DOM 并不会立即更新。它将在下一次事件循环清空队列时更新。大多数情况下下我们不太需要关心这个，但是如果想在 DOM 状态更新后做点什么就会有些麻烦了。尽管 Vue.js 鼓励开发者秉承数据驱动的思想，不要直接接触 DOM，但是有些场景确是难以避免需要这样做。为了在数据变化之后等待 Vue.js 完成 DOM 更新，可以在数据变化之后立即使用 `Vue.nextTick(callback)`。callback 将在 DOM 更新完成后调用。例如：

``` html
<div id="example">{{ message }}</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // change data // 修改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

There is also the `vm.$nextTick()` instance method, which is especially handy inside components, because it doesn't need global `Vue` and its callback's `this` context will be automatically bound to the current Vue instance:
`vm.$nextTick()` 实例方法也可以使用，在组件内部使用尤其方便，因为它不需要全局的 `Vue`，同时回调函数中的 `this` 也会自动绑定到当前 Vue 实例：

``` js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: 'not updated'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = 'updated'
      console.log(this.$el.textContent) // => 'not updated'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => 'updated'
      })
    }
  }
})
```