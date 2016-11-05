---
title: 状态管理 (State Management)
type: guide
order: 22
---

## 官方的类 Flux 实现 (Official Flux-Like Implementation)

Large applications can often grow in complexity, due to multiple pieces of state scattered across many components and the interactions between them. To solve this problem, Vue offers [vuex](https://github.com/vuejs/vuex): our own Elm-inspired state management library. It even integrates into [vue-devtools](https://github.com/vuejs/vue-devtools), providing zero-setup access to time travel.
大型应用的复杂度一般比较高，因为状态的不同部分会散落在很多组件里，而且不同部分之间还存在交互。针对这个问题，Vue 提供了解决方案 [vuex](https://github.com/vuejs/vuex) ：一个受 Elm 启发的状态管理库。我们甚至把它集成到了 [vue-devtool](https://github.com/vuejs/vue-devtools) 中，不需要任何配置就可以实现应用状态的时空旅行。

### 如果你是 React 开发者 (Information for React Developers)

If you're coming from React, you may be wondering how vuex compares to [redux](https://github.com/reactjs/redux), the most popular Flux implementation in that ecosystem. Redux is actually view-layer agnostic, so it can easily be used with Vue via some [simple bindings](https://github.com/egoist/revue). Vuex is different in that it _knows_ it's in a Vue app. This allows it to better integrate with Vue, offering a more intuitive API and improved development experience.
如果你是 React 开发者，你可能想知道 vuex 和 [redux](https://github.com/reactjs/redux) 的区别。redux 是 React 生态系统里最受欢迎的 Flux 实现，它和视图层无关，所以通过一些[简单的绑定](https://github.com/reactjs/redux)就可以与 Vue 配合使用。Vuex 有点不一样，它必需在一个 Vue 应用里使用。这让它可以更好地集成到 Vue 里面，提供直观的 API 以及更好的开发体验。

## 从头实现简单的状态管理 (Simple State Management from Scratch)

It is often overlooked that the source of truth in Vue applications is the raw `data` object - a Vue instance simply proxies access to it. Therefore, if you have a piece of state that should be shared by multiple instances, you can simply share it by identity:
大家经常会忽略的一点就是，Vue 应用中的数据源就是原始的 `data` 对象 —— Vue 实例只是代理了对这个对象的访问。因此，如果你需要在多个实例之间共享状态，只要共享这个对象引用就可以了：

``` js
const sourceOfTruth = {}

const vmA = new Vue({
  data: sourceOfTruth
})

const vmB = new Vue({
  data: sourceOfTruth
})
```

Now whenever `sourceOfTruth` is mutated, both `vmA` and `vmB` will update their views automatically. Subcomponents within each of these instances would also have access via `this.$root.$data`. We have a single source of truth now, but debugging would be a nightmare. Any piece of data could be changed by any part of our app at any time, without leaving a trace.
现在，当 `sourceOfTruth` 改变，`vmA` 和 `vmB` 都会自动更新它们的视图。它们的子组件也可以通过 `this.$root.$data` 来访问这个数据。这样，我们就有了单一数据源。但是调试起来会很痛苦，因为应用的任意模块都可以随时修改数据的任何部分，而且不会留下记录。

To help solve this problem, we can adopt a simple **store pattern**:
要解决这个问题，我们采用一个简单的 **store 模式**：

``` js
var store = {
  debug: true,
  state: {
    message: '你好！(Hello!)'
  },
  setMessageAction (newValue) {
    this.debug && console.log('触发 setMessageAction，新值是 (triggered with)', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    this.debug && console.log('触发 clearMessageAction (clearMessageAction triggered)')
    this.state.message = '触发动作 B (action B triggered)'
  }
}
```

Notice all actions that mutate the store's state are put inside the store itself. This type of centralized state management makes it easier to understand what type of mutations could happen and how are they triggered. When something goes wrong, we'll also now have a log of what happened leading up to the bug.
要注意所有的修改 store 状态的动作，都被放在 store 里面了。这样的集中式的状态管理可以让我们更容易了解数据能够发生什么样的变化，以及怎样被触发。当某个地方产生错误，我们也可以从日志里看到是什么变化导致了 bug。

In addition, each instance/component can still own and manage its own private state:
而且，每一个实例/组件仍然可以拥有和管理它们自己的私有状态：

``` js
var vmA = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})

var vmB = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})
```

![State Management](/images/state.png)

<p class="tip">It's important to note that you should never replace the original state object in your actions - the components and the store need to share reference to the same object in order for mutations to be observed.
你永远都不应该在动作中替换掉原来的状态对象 —— 组件和 store 必需共享同一个对象的引用，这样 Vue 才能观察到数据的变异。这一点非常重要。</p>

As we continue developing the convention where components are never allowed to directly mutate state that belongs to a store, but should instead dispatch events that notify the store to perform actions, we eventually arrive at the [Flux](https://facebook.github.io/flux/) architecture. The benefit of this convention is we can record all state mutations happening to the store and implement advanced debugging helpers such as mutation logs, snapshots, and history re-rolls / time travel.
当我们继续完善这个约定，也就是组件永远不能直接改变属于 store 的状态，而应该通过分发事件来通知 store 执行动作，我们最终得出了和 [Flux](https://facebook.github.io/flux/) 一样的架构。这个约定的优点是，我们可以记录所有的状态变化，实现更加高级的调试辅助功能，比如变化日志，状态快照，历史回滚／时光旅行。

This brings us full circle back to [vuex](https://github.com/vuejs/vuex), so if you've read this far it's probably time to try it out!
绕了一圈我们还是回到了 [vuex](https://github.com/vuejs/vuex)，既然你已经读到了这里，为何不尝试用一下它呢？
