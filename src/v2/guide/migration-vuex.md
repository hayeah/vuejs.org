---
title: 从 Vuex 0.6.x 迁移到 1.0 (Migration from Vuex 0.6.x to 1.0)
type: guide
order: 27
---

> Vuex 2.0 is released, but this guide only covers the migration to 1.0? Is that a typo? Also, it looks like Vuex 1.0 and 2.0 were released simultaneously. What's going on? Which one should I use and what's compatible with Vue 2.0?
> Vuex 2.0 已经发布了，而这个教程只告诉我如何迁移到 1.0 ？你在逗我吧？还有，Vuex 的 1.0 和 2.0 版本好像是同时发布的。什么鬼？我应该用哪一个版本？哪一个版本跟 Vue 2.0 兼容？

Both Vuex 1.0 and 2.0:
Vuex 1.0 和 2.0 都：

- fully support both Vue 1.0 and 2.0
完全支持 Vue 1.0 和 2.0
- will be maintained for the forseeable future
在相当一段时间内会一直维护

They have slightly different target users however.
不过它们的目标用户稍有不同。

__Vuex 2.0__ is a radical redesign and simplification of the API, for those who are starting new projects or want to be on the cutting edge of client-side state management. __It is not covered by this migration guide__, so you should check out [the Vuex 2.0 docs](https://vuex.vuejs.org/en/index.html) if you'd like to learn more about it.
__Vuex 2.0__ 是一个激进的重新设计，极大简化了 API，面向的是那些刚开始新项目，或者想使用客户端状态管理的最新技术的开发者。__本教程没有涉及 Vuex 2.0 的内容__，所以请参考 [Vuex 2.0 文档](https://vuex.vuejs.org/en/index.html)来进一步了解。

__Vuex 1.0__ is mostly backwards-compatible, so requires very few changes to upgrade. It is recommended for those with large existing codebases or who just want the smoothest possible upgrade path to Vue 2.0. This guide is dedicated to facilitating that process, but only includes migration notes. For the complete usage guide, see [the Vuex 1.0 docs](https://github.com/vuejs/vuex/tree/1.0/docs/en).
__Vuex 1.0__ 几乎是完全向后兼容的，只需要很小的改动就可以将旧项目升级到 Vuex 1.0。对于那些已经有大量代码的项目，或者想尽可能顺畅地升级到 Vue 2.0 的开发者，Vuex 1.0 会是更好的选择。本教程会帮助你完成这个过程，不过只会包括迁移的注意事项。如果你想查看完整的使用教程，可以看 [Vuex 1.0 文档](https://github.com/vuejs/vuex/tree/1.0/docs/en)。

## 接受字符串属性路径的 `store.watch` (`store.watch` with String Property Path) <sup>取代 (replaced)</sup>

`store.watch` now only accept functions. So for example, you would have to replace:
`store.watch` 现在只接受函数。比如，你需要将下面的代码：

``` js
store.watch('user.notifications', callback)
```

替换成：

``` js
store.watch(
  // When the returned result changes...
  // 当返回的结果发生变化时...
  function (state) {
    return state.user.notifications
  },
  // Run this callback
  // 调用这个回调函数
  callback
)
```

This gives you more complete control over the reactive properties you'd like to watch.
这让你可以更好地掌控要监听的响应式属性。

{% raw %}
<div class="upgrade-path">
  <h4>升级路径 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>store.watch</code> with a string as the first argument.<br>在你的项目里使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，查找以字符串作为第一参数的 <code>store.watch</code> 调用。</p>
</div>
{% endraw %}

## store 的事件分发器 (Store's Event Emitter) <sup>移除 (removed)</sup>

The store instance no longer exposes the event emitter interface (`on`, `off`, `emit`). If you were previously using the store as a global event bus, [see this section](migration.html#dispatch-and-broadcast-removed) for migration instructions.
store 实例不会再提供事件分发器的接口（`on`，`off`，`emit`）。如果你之前使用 store 来作为全局的事件中心，可以参考[教程的这一节]((migration.html#dispatch-and-broadcast-removed)，其中有相关的迁移指引。

Instead of using this interface to watch events emitted by the store itself (e.g. `store.on('mutation', callback)`), a new method `store.subscribe` is introduced. Typical usage inside a plugin would be:
为了替代原有的接口（比如 `store.on('mutation', callback)`)，我们引入了一个新的方法 `store.subscribe`。这个方法的典型使用场景，就是在插件中使用：

``` js
var myPlugin = store => {
  store.subscribe(function (mutation, state) {
    // Do something...
    // 代码逻辑
  })
}

```

See example [the plugins docs](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md) for more info.
可以参考 [插件文档](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md)中的示例代码。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>store.on</code>, <code>store.off</code>, and <code>store.emit</code>.<br>在你的项目里使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，查找用到 <code>store.on</code>，<code>store.off</code> 和 <code>store.emit</code> 的代码。</p>
</div>
{% endraw %}

## 中间件 (Middlewares) <sup>取代 (replaced)</sup>

Middlewares are replaced by plugins. A plugin is simply a function that receives the store as the only argument, and can listen to the mutation event on the store:
插件替代了中间件。插件其实就是接受 store 作为唯一参数的函数，它可以监听 store 的 mutation 事件：

``` js
const myPlugins = store => {
  store.subscribe('mutation', (mutation, state) => {
    // Do something...
    // 代码逻辑
  })
}
```

For more details, see [the plugins docs](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md).
更多细节可以参考[插件文档](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>middlewares</code> option on a store.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，查找带有 <code>middlewares</code> 选项的 store 对象。</p>
</div>
{% endraw %}
