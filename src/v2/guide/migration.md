---
title: 从 Vue 1.x 迁移 (Migration from Vue 1.x)
type: guide
order: 25
---

## 常见问题（FAQ）

> Woah - this is a super long page! Does that mean 2.0 is completely different, I'll have to learn the basics all over again, and migrating will be practically impossible?
> 卧槽 —— 这个教程怎么这么长？Vue 2.0 和 Vue 1.x 相差这么远？我是不是要从头学习 Vue 2.0？Vue 1.x 的项目是不是无法迁移了？

I'm glad you asked! The answer is no. About 90% of the API is the same and the core concepts haven't changed. It's long because we like to offer very detailed explanations and include a lot of examples. Rest assured, __this is not something you have to read from top to bottom!__
很高兴地告诉你，并不！Vue 2.0 和 Vue 1.x 中，大约 90% 的API是相同的，而且它们的核心概念也没有改变。这篇教程之所以这么长，是因为我们想非常详细地解释如何迁移，还包含了大量的示例代码。请放心，__你不需要从头到尾地阅读这篇教程__。

> Where should I start in a migration?
> 那我应该从哪里开始着手迁移？

1. Start by running the [migration helper](https://github.com/vuejs/vue-migration-helper) on a current project. We've carefully minified and compressed a senior Vue dev into a simple command line interface. Whenever they recognize a deprecated pattern, they'll let you know, offer suggestions, and provide links to more info.
首先，在当前的项目中运行[迁移工具](https://github.com/vuejs/vue-migration-helper)。我们非常细心将一个 Vue 开发版本最小化和压缩，然后打包到一个命令行界面。每当它识别出一个被弃用的模式，它会告诉你，提供建议，并且给出包含更多信息的链接。

2. After that, browse through the table of contents for this page in the sidebar. If you see a topic you may be affected by, but the migration helper didn't catch, check it out.
然后，你可以浏览一下本页面在侧栏中的目录。如果你看到一个[迁移工具](https://github.com/vuejs/vue-migration-helper)没有捕获到的主题，那就点击它进行阅读。

3. If you have any tests, run them and see what still fails. If you don't have tests, just open the app in your browser and keep an eye out for warnings or errors as you navigate around.
如果你有测试用例，运行它们，看看有哪些无法通过。如果你没有测试用例，那就尝试在浏览器中打开你的应用，留意浏览器发出的警告和错误。

4. By now, your app should be fully migrated. If you're still hungry for more though, you can read the rest of this page - or just dive in to the new and improved guide from [the beginning](index.html). Many parts will be skimmable, since you're already familiar with the core concepts.
现在，你的程序应该已经完全迁移到 Vue 2.0 了。如果你还想知道得更多，你可以阅读本页剩下的内容，或者看一下我们新的[上手教程](index.html)。你可以略过其中的很多内容，因为你对核心概念已经很熟悉了。

> How long will it take to migrate a Vue 1.x app to 2.0?
> 一个 Vue 1.x 的应用要迁移到 Vue 2.0 需要多长时间?

It depends on a few factors:
这取决于以下几点：

- The size of your app (small to medium-sized apps will probably be less than a day)
应用程序的大小（中小型应用应该只需要不到一天）

- How many times you get distracted and start playing with a cool new feature. <U+1F609> &nbsp;Not judging, it also happened to us while building 2.0!
你分心去玩酷炫新特性的次数。<U+1F609> &nbsp;我们并不是在吐槽你，因为我们自己在写 Vue 2.0 的时候也是这样的！

- Which obsolete features you're using. Most can be upgraded with find-and-replace, but others might take a few minutes. If you're not currently following best practices, Vue 2.0 will also try harder to force you to. This is a good thing in the long run, but could also mean a significant (though possibly overdue) refactor.
你使用了哪些过时的特性。大多数过时特性可以通过找到包含和替换升级，但有一些可能要花费好几分钟。如果你之前并没有遵循最佳实践，Vue 2.0 还会更加强迫你去遵循。这从长期来说是件好事，但也可能意味着不小的重构。

> If I upgrade to Vue 2, will I also have to upgrade Vuex and Vue-Router?
> 如果我升级到 Vue 2，我还要升级 Vuex 和 Vue-Router 吗？

Only Vue-Router 2 is compatible with Vue 2, so yes, you'll have to follow the [migration path for Vue-Router](migration-vue-router.html) as well. Fortunately, most applications don't have a lot of router code, so this likely won't take more than an hour.
只有 Vue-Router 2 兼容 Vue 2，所以你还要按照 [Vue-Router 升级方法](migration-vue-router.html)中的指引进行迁移。幸运的是，大多数应用的路由代码都不多，应该可以在一个小时以内搞定。


As for Vuex, even version 0.8 is compatible with Vue 2, so you're not forced to upgrade. The only reason you may want to upgrade immediately is to take advantage of the new features in Vuex 2, such as modules and reduced boilerplate.
你可以不升级 Vuex，因为它从版本 0.8 就开始兼容 Vue 2 了。你想要进行升级的唯一原因，可能是你希望使用 Vuex 2 的新特性，例如模块和更少的样板文件。

## 模板（Templates）

### 片段实例 (Fragment Instances) <sup>移除 (removed)</sup>

Every component must have exactly one root element. Fragment instances are no longer allowed. If you have a template like this:
每个组件必须有且只能有一个根元素。不再支持片段实例。如果你有一个像这样的模版：

``` html
<p>foo</p>
<p>bar</p>
```

It's recommended to simply wrap the entire contents in a new element, like this:
应该用一个新元素把它们包裹起来：

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about multiple root elements in a template.<br>在升级后，运行你的端对端测试或者直接运行应用，查看<strong>控制台警告</strong>中关于模版存在多个根元素的信息。</p>
</div>
{% endraw %}

## 生命周期钩子（Lifecycle Hooks）

### `beforeCompile` <sup>移除 (removed)</sup>

Use the `created` hook instead.
用钩子 `created` 。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

### `compiled` <sup>取代 (replaced)</sup>

Use the new `mounted` hook instead.
用新的钩子 `mounted` 取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

### `attached` <sup>移除 (removed)</sup>

Use a custom in-dom check in other hooks. For example, to replace:
在其他钩子中使用自定义的 dom 内检查。例如，下面的代码：

``` js
attached: function () {
  doSomething()
}
```

You could use:
可以用这段代码取代：

``` js
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

### `detached` <sup>移除 (removed)</sup>

Use a custom in-dom check in other hooks. For example, to replace:
在其他钩子使用自定义的 dom 内检查。例如，下面的代码：

``` js
detached: function () {
  doSomething()
}
```

You could use:
可以用这段代码取代：

``` js
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

### `init` <sup>重命名 (renamed)</sup>

Use the new `beforeCreate` hook instead, which is essentially the same thing. It was renamed for consistency with other lifecycle methods.
用新的钩子 `beforeCreate` 取代，它们本质上是一个东西。为了与其它生命周期方法一致，才将它重新命名。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

### `ready` <sup>取代 (replaced)</sup>

Use the new `mounted` hook instead. It should be noted though that with `mounted`, there's no guarantee to be in-document. For that, also include `Vue.nextTick`/`vm.$nextTick`. For example:
用新的钩子 `mounted` 取代。要注意，`mounted` 不保证实例已经插入到文档中。因此，你应该使用 `Vue.nextTick`/`vm.$nextTick` 来确保这一点。例如：


``` js
mounted: function () {
  this.$nextTick(function () {
    // code that assumes this.$el is in-document
    // 这里就可以保证 this.$el 已经插入到文档了
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.<br>在项目中运行<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这个钩子的所有代码。</p>
</div>
{% endraw %}

## `v-for` 指令 (`v-for`)

### `v-for` 对于数组的参数顺序 (`v-for` Argument Order for Arrays) <sup>改动 (changed)</sup>

When including an `index`, the argument order for arrays used to be `(index, value)`. It is now `(value, index)` to be more consistent with JavaScript's native array methods such as `forEach` and `map`.
当包含 `index` 时，之前数组的参数顺序是 `(index, value)`。而现在变成 `(value, index)` ，这是为了和 JavaScript 的原生数组方法，例如 `forEach` 和 `map` ，保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete argument order. Note that if you name your index arguments something unusual like <code>position</code> or <code>num</code>, the helper will not flag them.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到过时参数顺序的代码。注意，如果你用像 <code>position</code> 或 <code>num</code> 这样的名字给索引参数命名，那么迁移工具不会将它们标记出来。</p>
</div>
{% endraw %}

### `v-for` 对于对象的参数顺序 (`v-for` Argument Order for Objects) <sup>改动 (changed)</sup>

When including a `key`, the argument order for objects used to be `(key, value)`. It is now `(value, key)` to be more consistent with common object iterators such as lodash's.
当包含 `key` 时，之前对象的参数顺序时 `(key, value)`。而现在变成 `(value, key)`，这是为了和常用的对象迭代器，例如 lodash 的迭代器，保持一致。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete argument order. Note that if you name your key arguments something like <code>name</code> or <code>property</code>, the helper will not flag them.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到过时参数顺序的代码。注意，如果你用像 <code>position</code> 或 <code>num</code> 这样的名字给键参数命名，那么迁移工具不会将它们标记出来。</p>
</div>
{% endraw %}

### `$index` 和 `$key` (`$index` and `$key`) <sup>移除 (removed)</sup>

 The implicitly assigned `$index` and `$key` variables have been removed in favor of explicitly defining them in `v-for`. This makes the code easier to read for developers less experienced with Vue and also results in much clearer behavior when dealing with nested loops.
 隐式分配的 `$index` 和 `$key` 变量已经被弃用，现在你需要在 `v-for` 中明确地定义它们。这样的代码对那些缺乏经验的 Vue 开发者来说更容易阅读，而且在处理嵌套循环时也会更加清晰。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of these removed variables. If you miss any, you should also see <strong>console errors</strong> such as: <code>Uncaught ReferenceError: $index is not defined</code><br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这些移除变量的代码。如果没有找到，你还应该留意<strong>控制台异常</strong>，例如：<code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
{% endraw %}

### `track-by` <sup>取代 (replaced)</sup>

`track-by` has been replaced with `key`, which works like any other attribute: without the `v-bind:` or `:` prefix, it is treated as a literal string. In most cases, you'd want to use a dynamic binding which expects a full expression instead of a key. For example, in place of:
`key` 取代了 `track-by`，而且 `key` 像其他属性一样，在没有 `v-bind:` 或 `:` 前缀的情况下，会被当作一个字符串。在大多数情况下，你应该将 `key` 动态绑定到一个完整的表达式，而不是一个单独的键。例如，下面的代码：

``` html
<div v-for="item in items" track-by="id">
```

You would now write:
应该改成这样：

``` html
<div v-for="item in items" v-bind:key="item.id">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>track-by</code>.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>track-by</code> 的代码。</p>
</div>
{% endraw %}

### `v-for` 中的数值范围 (`v-for` Range Values) <sup>改动 (changed)</sup>

Previously, `v-for="number in 10"` would have `number` starting at 0 and ending at 9. Now it starts at 1 and ends at 10.
之前，`v-for="number in 10"` 是让 `number` 从 0 到 9 迭代。现在是 从 1 到 10 迭代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Search your codebase for the regex <code>/\w+ in \d+/</code>. Wherever it appears in a <code>v-for</code>, check to see if you may be affected.<br>在项目中用正则表达式 <code>/\w+ in \d+/</code> 进行找到包含。检查那些在 <code>v-for</code> 中的匹配，看新的数值范围是否影响了你的代码逻辑。</p>
</div>
{% endraw %}

## 属性 (Props)

### `coerce` 属性选项 (`coerce` Prop Option) <sup>移除 (removed)</sup>

If you want to coerce a prop, setup a local computed value based on it instead. For example, instead of:
如果你想强制转换一个属性，你应该设置基于它的本地计算值。例如，下面的代码：

``` js
props: {
  username: {
    type: String,
    coerce: function (value) {
      return value
        .toLowerCase()
        .replace(/\s+/, '-')
    }
  }
}
```

You could write:
可以改成这样：

``` js
props: {
  username: String,
},
computed: {
  normalizedUsername: function () {
    return this.username
      .toLowerCase()
      .replace(/\s+/, '-')
  }
}
```

There are a few advantages:
这样做有几个优点：

- You still have access to the original value of the prop.
你仍然能够访问属性的原始值；
- You are forced to be more explicit, by giving your coerced value a name that differentiates it from the value passed in the prop.
通过给强制转换的值命名，将它和原始值区分开来，提高代码的可读性。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>coerce</code> option.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>coerce</code> 选项的代码。</p>
</div>
{% endraw %}

### `twoWay` 属性选项 (`twoWay` Prop Option) <sup>移除 (removed)</sup>

Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:
现在，属性只能单向传递。如果要在父作用域中产生副作用，组件需要显式地触发事件，而不是依赖隐式绑定。更多的内容请参考：

- [Custom component events](components.html#Custom-Events)
[自定义组件事件](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
[自定义输入组件](components.html#Form-Input-Components-using-Custom-Events)（使用组件事件）
- [Global state management](state-management.html)
[全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>twoWay</code> option.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>twoWay</code> 选项的代码。</p>
</div>
{% endraw %}

### `v-bind` 的 `.once` 和 `.sync` 修饰符 (`v-bind` with `.once` and `.sync` Modifiers) <sup>移除 (removed)</sup>

Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:
现在，属性只能单向传递。如果要在父作用域中产生副作用，组件需要显式地触发事件，而不是依赖隐式绑定。更多的内容请参考：

- [Custom component events](components.html#Custom-Events)
[自定义组件事件](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
[自定义输入组件](components.html#Form-Input-Components-using-Custom-Events)（使用组件事件）
- [Global state management](state-management.html)
[全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>.once</code> and <code>.sync</code> modifiers.<br>在项目中使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>.once</code> 和 <code>.sync</code> 修饰符的代码。</p>
</div>
{% endraw %}

### 属性修改 (Prop Mutation) <sup>弃用 (deprecated)</sup>

Mutating a prop locally is now considered an anti-pattern, e.g. declaring a prop and then setting `this.myProp = 'someOtherValue'` in the component. Due to the new rendering mechanism, whenever the parent component re-renders, the child component's local changes will be overwritten.
现在，在组件内修改属性被认为是反模式，例如声明一个属性，然后在组件中通过 `this.myProp = 'someOtherValue'` 设置它的值。在新的渲染机制下，父组件在重新渲染时，会覆盖子组件的本地修改。

Most use cases of mutating a prop can be replaced by one of these options:
大多数对属性的修改，都可以用以下选项取代：

- a data property, with the prop used to set its default value
数据属性，用组件属性来设置它的默认值
- a computed property
计算属性

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about prop mutations.<br>在升级后运行端对端测试或者直接运行应用，留意有关属性修改的<strong>控制台警告</strong>。</p>
</div>
{% endraw %}

### 根实例的属性 (Props on a Root Instance) <sup>取代 (replaced)</sup>

On root Vue instances (i.e. instances created with `new Vue({ ... })`), you must use `propsData` instead of `props`.
在 Vue 的根实例中（例如用 `new Vue({ ... })` 创建的实例），你必须使用 `propsData` 而不是 `props`。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that props passed to root instances are no longer working.<br>如果有的话，运行你的端对端测试。其中的<strong>失败测试</strong>应该会提示你，传递给根实例的属性已经失效了。</p>
</div>
{% endraw %}

## 内置指令（Built-In Directives）

### `v-bind` 中的真假值 (Truthiness/Falsiness with `v-bind`) <sup>改动 (changed)</sup>

When used with `v-bind`, the only falsy values are now: `null`, `undefined`, and `false`. This means `0` and empty strings will render as truthy. So for example, `v-bind:draggable="''"` will render as `draggable="true"`.
当你使用 `v-bind` 时， 只有这些值会被当作假值：`null`，`undefined`，和 `false`。这意味着 `0` 和空字符串会被认为是真值。所以 `v-bind:draggable="''"` 会被渲染成 `draggable="true"`。

For enumerated attributes, in addition to the falsy values above, the string `"false"` will also render as `attr="false"`.
对于枚举属性，除了上面的假值以外，字符串 `"false"` 也会被渲染为 `attr="false"`。

<p class="tip">Note that for other directives (e.g. `v-if` and `v-show`), JavaScript's normal truthiness still applies.
注意，其它指令（例如 `v-if` 和 `v-show`）中的真假值判断，还是跟 JavaScript 的一致。</p>

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.<br>如果有的话，运行你的端对端测试。其中的<strong>失败测试</strong>应该会提示你，应用中的哪些地方可能受到这个改动影响。</p>
</div>
{% endraw %}

### 在组件中使用 `v-on`监听本地事件（Listening for Native Events on Components with `v-on`）<sup>改动 (changed)</sup>

When used on a component, `v-on` now only listens to custom events `$emit`ted by that component. To listen for a native DOM event on the root element, you can use the `.native` modifier. For example:
在使用一个组件时， Vue 2.0 的 `v-on` 只会监听组件触发的自定义事件。要监听根元素的原生 DOM 事件，你可以使用 `.native` 修饰符。例如这样：

``` html
<my-component v-on:click.native="doSomething"></my-component>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.<br>如果有的话，运行你的端对端测试。其中的<strong>失败测试</strong>应该会提示你，应用中的哪些地方可能受到这个改动影响。</p>
</div>
{% endraw %}

### `v-model` 的 `debounce` 参数属性 (`debounce` Param Attribute for `v-model`) <sup>移除 (removed)</sup>

Debouncing is used to limit how often we execute Ajax requests and other expensive operations. Vue's `debounce` attribute parameter for `v-model` made this easy for very simple cases, but it actually debounced __state updates__ rather than the expensive operations themselves. It's a subtle difference, but it comes with limitations as an application grows.
Vue 通过去除抖动来限制执行 Ajax 请求以及其它高耗操作的频率。在 Vue 中，`v-model` 的属性参数 `debounce` 让你可以轻松地处理简单情况，但是实际上，它限制了状态更新，而不是那些高耗操作。这是一个很小的差别，但随着应用规模的增长，会带来很多的限制。

These limitations become apparent when designing a search indicator, like this one for example:
例如，在设计一个搜索应用时，这些限制变得非常明显，如下例：

{% raw %}
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo" class="demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
<script>
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return 'âŸ³ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return 'âœ“ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
</script>
{% endraw %}

Using the `debounce` attribute, there'd be no way to detect the "Typing" state, because we lose access to the input's real-time state. By decoupling the debounce function from Vue however, we're able to debounce only the operation we want to limit, removing the limits on features we can develop:
使用 `debounce` 属性，就没有办法检测到 "Typing" 状态，因为我们无法得到输入控件的实时状态。不过，如果将去除抖动的函数从 Vue 代码中解耦出来，我们就可以只对特定操作进行去抖，而不会限制其它操作：

``` html
<!--
By using the debounce function from lodash or another dedicated
utility library, we know the specific debounce implementation we
use will be best-in-class - and we can use it ANYWHERE. Not just
in our template.
-->
<!-- 使用 lodash 或者其它专门工具库的去抖函数，我们不但可以获得最好的实现，而且可以在代码中的任何地方使用，而不局限在模版中。 -->
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
```

``` js
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return 'âŸ³ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return 'âœ“ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    // This is where the debounce actually belongs.
    // 这是去抖真正发生的地方。
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
```

Another advantage of this approach is there will be times when debouncing isn't quite the right wrapper function. For example, when hitting an API for search suggestions, waiting to offer suggestions until after the user has stopped typing for a period of time isn't an ideal experience. What you probably want instead is a __throttling__ function. Now since you're already using a utility library like lodash, refactoring to use its `throttle` function instead takes only a few seconds.
这个方法还有另一个好处。有时，去除抖动不作为包裹函数不太合适。例如，当我们查询 API 来获得搜索提示时，直到用户停止输入的时候显示提示，并不是一个好的用户体验。这时你可能想要用一个 __节流__ 函数来取代去抖函数。现在，因为你已经使用了 lodash 这样的工具库，所以你可以用它的 `throttle` 函数在十秒内完成重构。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>debounce</code> attribute.<br>在项目里使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>debounce</code> 属性的代码。</p>
</div>
{% endraw %}

### 使用 `lazy` 或 `number` 参数属性的 `v-model` (`v-model` with `lazy` or `number` Param Attributes)<sup>取代 (replaced)</sup>

The `lazy` and `number` param attributes are now modifiers, to make it more clear what That means instead of:
`lazy` 和 `number` 参数属性现在变成了修饰符。也就是说，下面的代码：

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

You would use:
应该改成这样：

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the these param attributes.<br>在项目里使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含这些参数属性的代码。</p>
</div>
{% endraw %}

### 使用内联 `value` 的 `v-model` (`v-model` with Inline `value`) <sup>移除 (removed)</sup>

`v-model` no longer cares about the initial value of an inline `value` attribute. For predictability, it will instead always treat the Vue instance data as the source of truth.
`v-model` 不再关心内联 `value` 属性的初始值。为了数据的可预测性，现在 `v-model` 会将 Vue 实例当作唯一的数据来源。

That means this element:
这意味着这个元素：

``` html
<input v-model="text" value="foo">
```

backed by this data:
以及数据：

``` js
data: {
  text: 'bar'
}
```

will render with a value of "bar" instead of "foo". The same goes for a `<textarea>` with existing content. Instead of:
会将 `text` 渲染成 "bar" 而不是 "foo"。这对于包含已有内容的 `<textarea>` 也是一样。你需要确保 `text` 的初始值是 "hello world"，而不是只在模版中这样定义：

``` html
<textarea v-model="text">
  hello world
</textarea>
```

You should ensure your initial value for `text` is "hello world".

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about inline value attributes with <code>v-model</code>.<br>在升级后，运行你的端对端测试或者直接运行应用，查看<strong>控制台警告</strong>中关于 <code>v-model</code> 的内联 <code>value</code> 属性的信息。</p>
</div>
{% endraw %}

### 使用 `v-for` 迭代原始值的 `v-model` (`v-model` with `v-for` Iterated Primitive Values )<sup>移除 (removed)</sup>

Cases like this no longer work:
这种写法将不再有效：

``` html
<input v-for="str in strings" v-model="str">
```

The reason is this is the equivalent JavaScript that the `<input>` would compile to:
原因是，上面这段代码会编译成类似下面的 JavaScript 代码：

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

As you can see, `v-model`'s two-way binding doesn't make sense here. Setting `str` to another value in the iterator function will do nothing because it's just a local variable in the function scope.
可以看到，`v-model` 的双向绑定在这里并不合理。在迭代器函数中设置 `str`毫无意义，因为它只是一个函数的局部变量。

Instead, you should use an array of __objects__ so that `v-model` can update the field on the object. For example:
这里你应该使用一个 __对象__ 的数组，这样 `v-model` 可以更新对象中的字段。例如：

``` html
<input v-for="obj in objects" v-model="obj.str">
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.<br>如果有测试套件，运行它。那些 <strong>失败的测试用例</strong> 应该可以提醒你，应用的哪些部分受到了这个变动的影响。</p>
</div>
{% endraw %}

### 用对象语法和 `!important` 的 `v-bind:style` (`v-bind:style` with Object Syntax and `!important`)<sup>移除 (removed)</sup>

This will no longer work:
这种写法不再有效：

``` html
<p v-bind:style="{ color: myColor + ' !important' }">hello</p>
```

If you really need to override another `!important`, you must use the string syntax:
如果你确实需要覆盖另一个`!important`，你必须使用字符串语法：

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">hello</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of style bindings with <code>!important</code> in objects.<br>在项目里使用<a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>!important</code> 样式绑定的对象。</p>
</div>
{% endraw %}

### `v-el` 和 `v-ref` (`v-el` and `v-ref`) <sup>取代 (replaced)</sup>

For simplicity, `v-el` and `v-ref` have been merged into the `ref` attribute, accessible on a component instance via `$refs`. That means `v-el:my-element` would become `ref="myElement"` and `v-ref:my-component` would become `ref="myComponent"`. When used on a normal element, the `ref` will be the DOM element, and when used on a component, the `ref` will be the component instance.
为了简洁，`v-el` 和 `v-ref` 已经被合并为 `ref` 属性，可以在组件实例上通过 `$refs` 访问。这意味着 `v-el:my-element` 将会变成 `ref="myElement"`，`v-ref:my-component` 会变成`ref="myComponent"`。对于普通的 HTML 元素，`ref` 就是 DOM 元素；对于组件，`ref` 就是组件实例。

Since `v-ref` is no longer a directive, but a special attribute, it can also be dynamically defined. This is especially useful in combination with `v-for`. For example:
由于 `v-ref` 不再是指令而是特殊属性，所以它也能被动态地定义。结合 `v-for` 使用时尤其有用。例如：

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

Previously, `v-el`/`v-ref` combined with `v-for` would produce an array of elements/components, because there was no way to give each item a unique name. You can still achieve this behavior by given each item the same `ref`:
之前的 `v-el`/`v-ref` 结合 `v-for` 会生成一个元素或组件的数组，因为没办法给每个数组项唯一的名称。你仍然可以通过给每个数组项相同的 `ref` 来达到这个效果：

``` html
<p v-for="item in items" ref="items"></p>
```

Unlike in 1.x, these `$refs` are not reactive, because they're registered/updated during the render process itself. Making them reactive would require duplicate renders for every change.
不像 Vue 1.x，Vue 2.0 中的 `$refs` 不是响应式的，因为它们在渲染过程中已经被注册/更新。如果将它们变成响应式的，会在每一次改动时造成重复渲染。

On the other hand, `$refs` are designed primarily for programmatic access in JavaScript - it is not recommended to rely on them in templates, because that would mean referring to state that does not belong to the instance itself. This would violate Vue's data-driven view model.
另外一方面, `$refs` 的设计主要是为了在 JavaScript 中访问，所以你不应该在模版中依赖它们，因为这样做会引用不属于实例本身的状态，从而违反了 Vue 的数据驱动视图模型。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>v-el</code> and <code>v-ref</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>v-el</code> 和 <code>v-ref</code> 的代码。</p>
</div>
{% endraw %}

### `v-else` 与 `v-show` (`v-else` with `v-show` ) <sup>移除 (removed)</sup>

`v-else` no longer works with `v-show`. Use `v-if` with a negation expression instead. For example, instead of:
`v-else` 将不再能够和 `v-show` 一起使用。你可以用 `v-if` 以及一个反表达式来取代。例如，下面的代码：

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

You can use:
可以改成：

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-else</code> with <code>v-show</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>v-else</code> 的代码。</p>
</div>
{% endraw %}

## 自定义指令（Custom Directives）<sup>简化 (simplified)</sup>

Directives have a greatly reduced scope of responsibility: they are now only used for applying low-level direct DOM manipulations. In most cases, you should prefer using components as the main code-reuse abstraction.
指令的职责范围已经大大缩小：它们现在只用来进行底层的直接 DOM 操作。在大多数情况下，你应该使用组件来作为代码重用的主要抽象手段。

Some of the most notable differences include:
一些最为显著的差异：

- Directives no longer have instances. This means there's no more `this` inside directive hooks. Instead, they receive everything they might need as arguments. If you really must persist state across hooks, you can do so on `el`.
指令不再有实例。这意味着，指令钩子里面不会再有 `this` 出现。现在，钩子需要的一切都只能通过参数接收。如果你确实需要在不同钩子之间维护状态，你可以使用 `el`。
- Options such as `acceptStatement`, `deep`, `priority`, etc have all been removed. To replace `twoWay` directives, see [this example](#Two-Way-Filters-replaced).
像 `acceptStatement`, `deep`, `priority` 等等的这些选项全部被移除。要取代 `twoWay` 指令，请参考[这个示例](#Two-Way-Filters-replaced)。
- Some of the current hooks have different behavior and there are also a couple new hooks.
一些现有钩子的行为已经改变了，而且我们还添加了几个新的钩子。

Fortunately, since the new directives are much simpler, you can master them more easily. Read the new [Custom Directives guide](custom-directive.html) to learn more.
幸运的是，新的指令非常简单，你可以轻松地掌握它们。你可以阅读教程的[自定义指令](custom-directive.html)一节来深入学习。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of defined directives. The helper will flag all of them, as it's likely in most cases that you'll want to refactor to a component.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含预设指令的代码。迁移工具会将它们全部标记出来，因为在大多数情况下你可能需要将它们重构成一个组件</p>
</div>
{% endraw %}

### 指令的 `.literal` 修饰符 (Directive `.literal` Modifier) <sup>移除 (removed)</sup>

The `.literal` modifier has been removed, as the same can be easily achieved by just providing a string literal as the value.
`.literal` 修饰符已经被移除，同样的效果只需要用一个字符串字面量作为值就可以实现。

For example, you can update:
例如你可以把下面的代码：

``` js
<p v-my-directive.literal="foo bar baz"></p>
```

to just:
改成这样：

``` html
<p v-my-directive="'foo bar baz'"></p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the `.literal` modifier on a directive.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在指令中使用 `.literal` 修饰符的代码。</p>
</div>
{% endraw %}

## 过渡效果（Transitions）

### `transition` 属性 (`transition` Attribute) <sup>取代 (replaced)</sup>

Vue's transition system has changed quite drastically and now uses `<transition>` and `<transition-group>` wrapper elements, rather than the `transition` attribute. It's recommended to read the new [Transitions guide](transitions.html) to learn more.
Vue 的过渡效果系统已经发生了大幅度的改变，它现在使用 `<transition>` 和 `<transition-group>` 包裹元素，取代 `transition` 属性。你应该阅读教程中的[过渡效果](transitions.html)一节来进一步学习。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>transition</code> 属性的代码。</p>
</div>
{% endraw %}

### 可重用的 `Vue.transition` (`Vue.transition` for Reusable Transitions)<sup>取代 (replaced)</sup>

With the new transition system, you can now just [use components for reusable transitions](transitions.html#Reusable-Transitions).
在新的过渡效果系统中，你只能[使用组件来重用过渡效果](transitions.html#Reusable-Transitions).

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.transition</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.transition</code> 的代码。</p>
</div>
{% endraw %}

### 过渡效果的 `stagger` 属性 (Transition `stagger` Attribute)<sup>移除 (removed)</sup>

If you need to stagger list transitions, you can control timing by setting and accessing a `data-index` (or similar attribute) on an element. See [an example here](transitions.html#Staggering-List-Transitions).
如果你需要错开列表的过渡效果，你可以在元素上设置和访问一个 `data-index`（或类似的）属性。参考[这个示例](transitions.html#Staggering-List-Transitions)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute. During your update, you can transition (pun very much intended) to the new staggering strategy as well.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>transition</code> 属性的代码。在更新过程中，你也可以选择使用新的错开策略。</p>
</div>
{% endraw %}

## 事件（Events）

### `events` 选项 (`events` option) <sup>移除 (removed)</sup>

The `events` option has been removed. Event handlers should now be registered in the `created` hook instead. Check out the [`$dispatch` and `$broadcast` migration guide](#dispatch-and-broadcast-replaced) for a detailed example.
`events` 选项已经被移除。现在你应该在 `created` 钩子中注册事件处理器。更详细的示例请参考 [`$dispatch` 和 `$broadcast` 迁移教程](#dispatch-and-broadcast-replaced)。

### `Vue.directive('on').keyCodes` <sup>取代 (replaced)</sup>

The new, more concise way to configure `keyCodes` is through `Vue.config.keyCodes`. For example:
更加简洁的配置 `keyCodes` 的方式是通过 `Vue.config.keyCodes`。例如：

``` js
// enable v-on:keyup.f1
// 启用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the the old <code>keyCode</code> configuration syntax.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含旧的 <code>keyCode</code> 配置语法的代码。</p>
</div>
{% endraw %}

### `$dispatch` 和 `$broadcast` (`$dispatch` and `$broadcast`)<sup>取代 (replaced)</sup>

`$dispatch` and `$broadcast` have been removed in favor of more explicitly cross-component communication and more maintainable state management solutions, such as [Vuex](https://github.com/vuejs/vuex).
`$dispatch` 和 `$broadcast` 已经被移除，取代它们的是更为明确的跨组件沟通，以及更易于维护的状态管理解决方案，例如 [Vuex](https://github.com/vuejs/vuex)。

The problem is event flows that depend on a component's tree structure can be hard to reason about and very brittle when the tree becomes large. It simply doesn't scale well and we don't want to set you up for pain later. `$dispatch` and `$broadcast` also do not solve communication between sibling components.
它们的问题在于，当组件树变得巨大时，事件流变得很难推理。原来的做法很难规模化，而我们不想给你挖坑。还有，`$dispatch` 和 `$broadcast` 无法解决兄弟组件间的通信问题。

One of the most common uses for these methods is to communicate between a parent and its direct children. In these cases, you can actually [listen to an `$emit` from a child with `v-on`](components.html#Form-Input-Components-using-Custom-Events). This allows you to keep the convenience of events with added explicitness.
这些方法最常用的情景就是父子组件间的通信。而要处理这些问题，你实际上可以[在有 `v-on` 的子组件中监听 `$emit`](components.html#Form-Input-Components-using-Custom-Events)。这既可以保留事件的便利性，同时代码也更加明确。

However, when communicating between distant descendants/ancestors, `$emit` won't help you. Instead, the simplest possible upgrade would be to use a centralized event hub. This has the added benefit of allowing you to communicate between components no matter where they are in the component tree - even between siblings! Because Vue instances implement an event emitter interface, you can actually use an empty Vue instance for this purpose.
不过，如果进行通信的组件隔了很多代，那么 `$emit` 也帮不了你。最简单的做法是，使用一个集中的事件中心。它给你带来的好处就是让不同的组件可以进行通信，无论它们在组件树中处于什么位置 —— 即使是在兄弟组件之间！因为 Vue 实例实现了一个事件触发器的接口，你可以用一个空的 Vue 实例来达到这个目的。

For example, let's say we have a todo app structured like this:
例如，假设我们有一个 todo 应用，它的结构像这样：

```
Todos
|-- NewTodoInput
|-- Todo
    |-- DeleteTodoButton
```

We could manage communication between components with this single event hub:
我们可以用单一的事件中心来管理组件间的通信：

``` js
// This is the event hub we'll use in every
// component to communicate between them.
// 在每一个部件中都使用这个事件中心来进行通信。
var eventHub = new Vue()
```

Then in our components, we can use `$emit`, `$on`, `$off` to emit events, listen for events, and clean up event listeners, respectively:
然后在组件中，我们可以分别使用 `$emit`，`$on`，`$off` 来触发事件，监听事件，以及清除事件监听器：

``` js
// NewTodoInput
// 新 todo 的输入控件
// ...
methods: {
  addTodo: function () {
    eventHub.$emit('add-todo', { text: this.newTodoText })
    this.newTodoText = ''
  }
}
```

``` js
// DeleteTodoButton
// 删除 todo 的按钮
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// Todos
// Todos 组件
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// It's good to clean up event listeners before
// a component is destroyed.
// 最好在组件被销毁前清楚事件监听器。
beforeDestroy: function () {
  eventHub.$off('add-todo', this.addTodo)
  eventHub.$off('delete-todo', this.deleteTodo)
},
methods: {
  addTodo: function (newTodo) {
    this.todos.push(newTodo)
  },
  deleteTodo: function (todoId) {
    this.todos = this.todos.filter(function (todo) {
      return todo.id !== todoId
    })
  }
}
```

This pattern can serve as a replacement for `$dispatch` and `$broadcast` in simple scenarios, but for more complex cases, it's recommended to use a dedicated state management layer such as [Vuex](https://github.com/vuejs/vuex).
在简单的使用场景中，这种模式可以替代 `$dispatch` 和 `$broadcast`。不过在更复杂的场景里，你应该使用一个专门的状态管理层，例如 [Vuex](https://github.com/vuejs/vuex)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>$dispatch</code> and <code>$broadcast</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>$dispatch</code> 和 <code>$broadcast</code> 的代码。</p>
</div>
{% endraw %}

## 过滤器（Filters）

### 文本插值外的过滤器 (Filters Outside Text Interpolations)<sup>移除 (removed)</sup>

Filters can now only be used inside text interpolations (`{% raw %}{{ }}{% endraw %}` tags). In the past we've found using filters within directives such as `v-model`, `v-on`, etc led to more complexity than convenience. For list filtering on `v-for`, it's also better to move that logic into JavaScript as computed properties, so that it can be reused throughout your component.
过滤器现在只能在文本插值中使用（`{% raw %}{{ }}{% endraw %}` 标签）。之前我们发现在指令（例如 `v-model`）中使用过滤器，会让事情变得更加复杂。如果在 `v-on` 里过滤列表，最好将逻辑移到 JavaScript 中，让它成为计算属性，这样可以在整个组件中重用。

In general, whenever something can be achieved in plain JavaScript, we want to avoid introducing a special syntax like filters to take care of the same concern. Here's how you can replace Vue's built-in directive filters:
一般来说，如果一个功能可以在 JavaScript 中实现，我们都会避免引入一个特殊的语法（例如过滤器）。你可以用以下方式取代 Vue 内置的指令过滤器。

#### 取代 `debounce` 过滤器

Instead of:
之前的写法：

``` html
<input v-on:keyup="doStuff | debounce 500">
```

``` js
methods: {
  doStuff: function () {
    // ...
  }
}
```

Use [lodash's `debounce`](https://lodash.com/docs/4.15.0#debounce) (or possibly [`throttle`](https://lodash.com/docs/4.15.0#throttle)) to directly limit calling the expensive method. You can achieve the same as above like this:
现在，你可以使用 [lodash 的 `debounce`](https://lodash.com/docs/4.15.0#debounce)（或者 [`throttle`](https://lodash.com/docs/4.15.0#throttle)），来限制对高耗方法的调用。比如将上面的代码改成这样：

``` html
<input v-on:keyup="doStuff">
```

``` js
methods: {
  doStuff: _.debounce(function () {
    // ...
  }, 500)
}
```

For more on the advantages of this strategy, see [the example here with `v-model`](#v-model-with-debounce-removed).
这个策略的更多优点，可以参考[这个使用 `v-model` 的示例](#v-model-with-debounce-removed)。

#### 取代 `limitBy` 过滤器 (Replacing the `limitBy` Filter)

Instead of:
之前的写法：

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

Use JavaScript's built-in [`.slice` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) in a computed property:
现在你可以在计算属性中使用 JavaScript 内置的 [`.slice` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)：

``` html
<p v-for="item in filteredItems">{{ item }}</p>
```

``` js
computed: {
  filteredItems: function () {
    return this.items.slice(0, 10)
  }
}
```

#### 取代 `filterBy` 过滤器 (Replacing the `filterBy` Filter)

Instead of:
之前的写法：

``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

Use JavaScript's built-in [`.filter` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) in a computed property:
现在，你可以在计算属性中使用 JavaScript 内置的 [`.filter` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)：


``` html
<p v-for="user in filteredUsers">{{ user.name }}</p>
```

``` js
computed: {
  filteredUsers: function () {
    var self = this
    return self.users.filter(function (user) {
      return user.name.indexOf(self.searchQuery) !== -1
    })
  }
}
```

JavaScript's native `.filter` can also manage much more complex filtering operations, because you have access to the full power of JavaScript within computed properties. For example, if you wanted to find all active users and case-insensitively match against both their name and email:
JavaScript 原生的 `.filter` 还可以进行更复杂的过滤操作，因为你可以在计算属性中发挥 JavaScript 的全部能力。例如，如果你想用不区分大小写的名字和电子邮件，找到所有活跃用户：

``` js
var self = this
self.users.filter(function (user) {
  var searchRegex = new RegExp(self.searchQuery, 'i')
  return user.isActive && (
    searchRegex.test(user.name) ||
    searchRegex.test(user.email)
  )
})
```

#### 取代 `orderBy` 过滤器 (Replacing the `orderBy` Filter)

Instead of:
之前的写法：

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

Use [lodash's `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (or possibly [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)) in a computed property:
现在，你可以在计算属性中使用 [lodash 的 `orderBy`](https://lodash.com/docs/4.15.0#orderBy)（或者 [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)）：

``` html
<p v-for="user in orderedUsers">{{ user.name }}</p>
```

``` js
computed: {
  orderedUsers: function () {
    return _.orderBy(this.users, 'name')
  }
}
```

You can even order by multiple columns:
你甚至可以按多列排序：

``` js
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters being used inside directives. If you miss any, you should also see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在指令中使用过滤器的代码。如果有遗漏，你应该会看到 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### 过滤器参数语法 (Filter Argument Syntax) <sup>改动 (changed)</sup>

Filters' syntax for arguments now better aligns with JavaScript function invocation. So instead of taking space-delimited arguments:
现在，过滤器的参数语法与 JavaScript 的函数调用更加一致。之前的写法：

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

We surround the arguments with parentheses and delimit the arguments with commas:
现在，我们用括号来包裹参数，用逗号来分隔参数：

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the old filter syntax. If you miss any, you should also see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含旧的过滤器语法的代码。如果有遗漏，你应该会看到 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### 内置的文本过滤器 (Built-In Text Filters)<sup>移除 (removed)</sup>

Although filters within text interpolations are still allowed, all of the filters have been removed. Instead, it's recommended to use more specialized libraries for solving problems in each domain (e.g. [`date-fns`](https://date-fns.org/) to format dates and [`accounting`](http://openexchangerates.github.io/accounting.js/) for currencies).
虽然在文本插值内的过滤器还是合法的，但所有的过滤器都被删除了。取而代之地，你应该使用专门的库来解决不同领域的问题（例如日期格式化的 [`date-fns`](https://date-fns.org/)，还有货币格式化的 [`accounting`](http://openexchangerates.github.io/accounting.js/)）。

For each of Vue's built-in text filters, we go through how you can replace them below. The example code could exist in custom helper functions, methods, or computed properties.
我们会在下面告诉你，如何取代 Vue 的每一个内置文本过滤器。下面的示例代码可以在自定义的工具函数，方法或者计算属性中使用。

#### 取代 `json` 过滤器 (Replacing the `json` Filter)

You actually don't need to for debugging anymore, as Vue will nicely format output for you automatically, whether it's a string, number, array, or plain object. If you want the exact same functionality as JavaScript's `JSON.stringify` though, then you can use that in a method or computed property.
你不再需要它来进行调试了，Vue 会自动帮你格式化输出，无论是字符串，数组，还是普通对象。不过，如果你还是想要和 JavaScript 的 `JSON.stringify` 一样的功能，你可以在方法或者计算属性中直接使用它。

#### 取代 `capitalize` 过滤器 (Replacing the `capitalize` Filter)

``` js
text[0].toUpperCase() + text.slice(1)
```

#### 取代 `uppercase` 过滤器 (Replacing the `uppercase` Filter)

``` js
text.toUpperCase()
```

#### 取代 `lowercase` 过滤器 (Replacing the `lowercase` Filter)

``` js
text.toLowerCase()
```

#### 取代 `pluralize` 过滤器 (Replacing the `pluralize` Filter)

The [pluralize](https://www.npmjs.com/package/pluralize) package on NPM serves this purpose nicely, but if you only want to pluralize a specific word or want to have special output for cases like `0`, then you can also easily define your own pluralize functions. For example:
NPM 中的 [pluralize](https://www.npmjs.com/package/pluralize) 包可以很好地解决这个问题。不过如果你只是想将特定的词复数化，或者给像 `0` 这样的例子一个特殊的输出，你可以自定义复数化函数。例如：

``` js
function pluralizeKnife (count) {
  if (count === 0) {
    return 'no knives'
  } else if (count === 1) {
    return '1 knife'
  } else {
    return count + 'knives'
  }
}
```

#### 取代 `currency` 过滤器 (Replacing the `currency` Filter)

For a very naive implementation, you could just do something like this:
简单的实现如下：

``` js
'$' + price.toFixed(2)
```

In many cases though, you'll still run into strange behavior (e.g. `0.035.toFixed(2)` rounds up to `0.04`, but `0.045` rounds down to `0.04`). To work around these issues, you can use the [`accounting`](http://openexchangerates.github.io/accounting.js/) library to more reliably format currencies.
但是，在许多情况下，你会遇到奇怪的行为（例如 `0.035.toFixed(2)` 会向上舍入到 `0.04`，但是 `0.045` 会向下舍入到 `0.04` ）。要绕开这些问题，你可以使用 [`accounting`](http://openexchangerates.github.io/accounting.js/) 库来对货币进行更可靠的格式化。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete text filters. If you miss any, you should also see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含过时文本过滤器的代码。如果有遗漏，你应该会看到 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### 双向过滤器 (Two-Way Filters) <sup>取代 (replaced)</sup>

Some users have enjoyed using two-way filters with `v-model` to create interesting inputs with very little code. While _seemingly_ simple however, two-way filters can also hide a great deal of complexity - and even encourage poor UX by delaying state updates. Instead, components wrapping an input are recommended as a more explicit and feature-rich way of creating custom inputs.
一些用户很喜欢 `v-model` 和双向过滤器，他们可以用非常少的代码来构建一些有趣的输入控件。虽然这_看起来_很简单，但实际上双向过滤器隐藏了大量的复杂度 —— 甚至会因为拖延了状态更新而造成不好的用户体验。所以，你应该使用组件去包裹输入控件，这样构建自定义控件会更加清晰，并且能够实现更丰富的功能。

As an example, we'll now walk the migration of a two-way currency filter:
作为示例，我们来完成一个双向货币过滤器的迁移：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/6744xnjk/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

It mostly works well, but the delayed state updates can cause strange behavior. For example, click on the `Result` tab and try entering `9.999` into one of those inputs. When the input loses focus, its value will update to `$10.00`. When looking at the calculated total however, you'll see that `9.999` is what's stored in our data. The version of reality that the user sees is out of sync!
大多数情况下这段代码都没问题，不过被推迟的状态更新有时会产生奇怪的行为。例如，点击 `Result` 标签，尝试在其中一个输入框里输入 `9.99` 。当焦点不在输入框后，值会更新成 `10.00`。不过，如果看一下合计的总数，你会看到我们数据里储存的是 `9.999`。用户所看到的数据和实际数据是不同步的！

To start transitioning towards a more robust solution using Vue 2.0, let's first wrap this filter in a new `<currency-input>` component:
要迁移到一个使用 Vue 2.0 的更为健壮的解决方案，我们先要将过滤器包裹在一个新的 `<currency-input>` 组件中：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/943zfbsh/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This allows us add behavior that a filter alone couldn't encapsulate, such as selecting the content of an input on focus. Now the next step will be to extract the business logic from the filter. Below, we pull everything out into an external [`currencyValidator` object](https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e):
这让我们可以增加一个单独的过滤器无法封装的功能，例如当焦点定位到一个输入控件时全选其中的内容。下一步，我们将过滤器中的业务逻辑提取出来。我们会将代码放在外部的 [`currencyValidator` 对象](https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e)里：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/9c32kev2/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

This increased modularity not only makes it easier to migrate to Vue 2, but also allows currency parsing and formatting to be:
这样做提高了代码的模块化，不仅迁移到 Vue 2 变得更加轻松，而且让货币解析和格式化的功能：

- unit tested in isolation from your Vue code
可以从 Vue 代码中分离出来，单独地进行单元测试；
- used by other parts of your application, such as to validate the payload to an API endpoint
在应用的其它部分使用，例如校验 API 接口返回的信息。

Having this validator extracted out, we've also more comfortably built it up into a more robust solution. The state quirks have been eliminated and it's actually impossible for users to enter anything wrong, similar to what the browser's native number input tries to do.
因为将校验器提取出来，我们构建了一个更为健壮的解决方案。那个关于状态的奇怪问题被解决了，实际上用户不再能输入奇奇怪怪的东西，这点和浏览器的原生数值输入控件类似。

We're still limited however, by filters and by Vue 1.0 in general, so let's complete the upgrade to Vue 2.0:
不过，因为过滤器以及 Vue 1.0 的原因，我们仍然受到限制。所以接下来让我们完成到 Vue 2.0 的升级：

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/1oqjojjx/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

You may notice that:
你会注意到：

- Every aspect of our input is more explicit, using lifecycle hooks and DOM events in place of the hidden behavior of two-way filters.
输入控件的每一个部分都更为明晰，因为使用了生命周期钩子以及 DOM 事件，取代双向过滤器的隐藏行为；
- We can now use `v-model` directly on our custom inputs, which is not only more consistent with normal inputs, but also means our component is Vuex-friendly.
我们现在可以在自定义输入控件上直接使用 `v-model`，自定义控件和普通输入控件不仅更加一致，而且我们的部件对 Vuex 更加友好；
- Since we're no longer using filter options that require a value to be returned, our currency work could actually be done asynchronously. That means if we had a lot of apps that had to work with currencies, we could easily refactor this logic into a shared microservice.
因为我们不再使用过滤器选项，不需要返回一个值，所以实际上可以异步完成对货币的处理。这意味着如果我们有大量的要处理货币问题的应用，我们可以轻松地将逻辑重构成一个微服务。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters used in directives like <code>v-model</code>. If you miss any, you should also see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在指令中使用过滤器的代码，例如 <code>v-model</code>。如果有遗漏，你应该会看到 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

## 插槽（Slots）

### 重复插槽 (Duplicate Slots )<sup>移除 (removed)</sup>

It is no longer supported to have `<slot>`s with the same name in the same template. When a slot is rendered it is "used up" and cannot be rendered elsewhere in the same render tree. If you must render the same content in multiple places, pass that content as a prop.
现在不再支持在同一个模版中使用同名的 `<slot>`。当一个插槽被渲染时，它的内容就已经“用完”了，没有办法在同一颗渲染树中再次渲染。如果你一定要在多个地方渲染同样的内容，你应该将内容作为属性传递。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about duplicate slots <code>v-model</code>.<br>在升级后，运行你的端对端测试或者直接运行应用，查看<strong>控制台警告</strong>中关于重复插槽的信息。</p>
</div>
{% endraw %}

### `slot` 属性样式 (`slot` Attribute Styling)<sup>移除 (removed)</sup>

Content inserted via named `<slot>` no longer preserves the `slot` attribute. Use a wrapper element to style them, or for advanced use cases, modify the inserted content programmatically using [render functions](render-function.html).
通过具名 `<slot>` 插入的内容，不再保留 `slot` 属性。你应该使用包裹元素给它们添加样式。更高级的做法是，使用[渲染函数](render-function.html)，通过代码来修改插入的内容。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find CSS selectors targeting named slots (e.g. <code>[slot="my-slot-name"]</code>).<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在具名插槽上的 CSS 选择器（例如 <code>[slot="my-slot-name"]</code>）。</p>
</div>
{% endraw %}

## 特殊属性（Special Attributes）

### `keep-alive` 属性 (`keep-alive` Attribute)<sup>取代 (replaced)</sup>

`keep-alive` is no longer a special attribute, but rather a wrapper component, similar to `<transition>`. For example:
`keep-alive` 已经不再是一个特殊属性，而是一个包裹组件，与 `<transition>` 类似。例如：

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

This makes it possible to use `<keep-alive>` on multiple conditional children:
这样可以将 `<keep-alive>` 应用在多个条件渲染的子组件上：

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">When `<keep-alive>` has multiple children, they should eventually evaluate to a single child. Any child other than the first one will simply be ignored.
如果 `<keep-alive>` 有多个子组件时，在求值后应该只剩一个子组件。否则，除了第一个子组件以外的子组件都会被忽略。</p>

When used together with `<transition>`, make sure to nest it inside:
在和 `<transition>` 一起使用时，确保 `<keep-alive>` 被包在里面：

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find <code>keep-alive</code> attributes.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>keep-alive</code> 属性的代码。</p>
</div>
{% endraw %}

## 插值（Interpolation）

### 属性内的插值 (Interpolation within Attributes)<sup>移除 (removed)</sup>

Interpolation within attributes is no longer valid. For example:
属性内的插值不再有效。例如：

``` html
<button class="btn btn-{{ size }}"></button>
```

Should either be updated to use an inline expression:
应该改为使用内联表达式：

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

Or a data/computed property:
或者数据/计算属性：

``` html
<button v-bind:class="buttonClasses"></button>
```

``` js
computed: {
  buttonClasses: function () {
    return 'btn btn-' + size
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of interpolation used within attributes.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在属性内使用插值的代码。</p>
</div>
{% endraw %}

### HTML 插值 (HTML Interpolation)<sup>移除 (removed)</sup>

HTML interpolations (`{% raw %}{{{ foo }}}{% endraw %}`) have been removed in favor of the [`v-html` directive](../api/#v-html).
HTML 插值（`{% raw %}{{{ foo }}}{% endraw %}`）已经被移除，你应该使用 [`v-html` 指令](../api/#v-html)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find HTML interpolations.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到 HTML 插值的代码。</p>
</div>
{% endraw %}

### 一次性绑定 (One-Time Bindings)<sup>取代 (replaced)</sup>

One time bindings (`{% raw %}{{* foo }}{% endraw %}`) have been replaced by the new [`v-once` directive](../api/#v-once).
一次性绑定（`{% raw %}{{* foo }}{% endraw %}`）已经被用新的 [`v-once` 指令](../api/#v-once)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find one-time bindings.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到一次性绑定的代码。</p>
</div>
{% endraw %}

## 响应式系统（Reactivity）

### `vm.$watch` <sup>改动 (changed)</sup>

Watchers created via `vm.$watch` are now fired before the associated component rerenders. This gives you the chance to further update state before the component rerender, thus avoiding unnecessary updates. For example, you can watch a component prop and update the component's own data when the prop changes.
由 `vm.$watch` 创建的监视器，会在关联的组件渲染前触发。这让你能够在组件被重新渲染前进一步更新状态，避免不必要的更新。例如，你可以监视组件属性，当它变化时更新组件自己的数据。

If you were previously relying on `vm.$watch` to do something with the DOM after a component updates, you can instead do so in the `updated` lifecycle hook.
如果你之前依赖 `vm.$watch`，在组件更新后做一些 DOM 操作，那么现在你可以把代码逻辑放在 `updated` 生命周期钩子中。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that a watcher was relying on the old behavior.<br>如果有的话，运行你的端对端测试。其中的<strong>失败测试</strong>应该会提示你，哪一个监听器在依赖旧的行为。</p>
</div>
{% endraw %}

### `vm.$set` <sup>改动 (changed)</sup>

`vm.$set` is now just an alias for [`Vue.set`](../api/#Vue-set).
`vm.$set` 现在仅仅是 [`Vue.set`](../api/#Vue-set) 的别名。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete usage.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 `vm.$set` 的代码。</p>
</div>
{% endraw %}

### `vm.$delete` <sup>改动 (changed)</sup>

`vm.$delete` is now just an alias for [`Vue.delete`](../api/#Vue-delete).
`vm.$delete` 现在仅仅是 [`Vue.delete`](../api/#Vue-delete) 的别名。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete usage.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 `vm.$delete` 的代码。</p>
</div>
{% endraw %}

### `Array.prototype.$set` <sup>移除 (removed)</sup>

Use `Vue.set` instead.
用 `Vue.set` 取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$set</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>.$set</code> 的代码。如果有遗漏，你应该能看到遗漏方法的 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>移除 (removed)</sup>

Use `Array.prototype.splice` instead. For example:
用 `Array.prototype.splice` 取代。例如：

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

Or better yet, just pass removal methods an index:
更好的做法是，只传递索引给删除方法：

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$remove</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在数组上使用 <code>.$remove</code> 的代码。如果有遗漏，你应该能看到缺失方法的 <strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### 在 Vue 实例上使用 `Vue.set` 和 `Vue.delete` (`Vue.set` and `Vue.delete` on Vue instances )<sup>移除 (removed)</sup>

`Vue.set` and `Vue.delete` can no longer work on Vue instances. It is now mandatory to properly declare all top-level reactive properties in the data option. If you'd like to delete properties on a Vue instance or its `$data`, just set it to null.
在 Vue 实例上，`Vue.set` 和 `Vue.delete` 已经失效。Vue 现在强制要求，必须在 `data` 选项里声明所有的顶层响应式属性。如果你想要删除 Vue 实例或者它的 `$data` 上的属性，只要设为 `null` 就好了。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.set</code> or <code>Vue.delete</code> on a Vue instance. If you miss any, they'll trigger <strong>console warnings</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到在 Vue 实例上的 <code>Vue.set</code> 或者 <code>Vue.delete</code>。如果有遗漏，它们会触发<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### 取代 `vm.$data` (Replacing `vm.$data`)<sup>移除 (removed)</sup>

It is now prohibited to replace a component instance's root $data. This prevents some edge cases in the reactivity system and makes the component state more predictable (especially with type-checking systems).
现在禁止替换组件实例的根数据对象。这样可以避免响应式系统的一些边界情况，让组件的状态更容易预测（特别是类型检查系统）。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of overwriting <code>vm.$data</code>. If you miss any, <strong>console warnings</strong> will be emitted.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到重写 <code>vm.$data</code> 的代码。如果有遗漏，它们会触发<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$get`<sup>移除 (removed)</sup>

Just retrieve reactive data directly.
直接读取响应式数据就好了。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$get</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$get</code> 的代码。如果有遗漏，你会看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

## DOM 相关的实例方法（DOM-Focused Instance Methods）

### `vm.$appendTo`<sup>移除 (removed)</sup>

Use the native DOM API:
直接使用原生的 DOM API：

``` js
myElement.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$appendTo</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$appendTo</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$before` <sup>移除 (removed)</sup>

Use the native DOM API:
直接使用原生的 DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$before</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$before</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$after` <sup>移除 (removed)</sup>

Use the native DOM API:
直接使用原生的 DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
```

Or if `myElement` is the last child:
或者，如果 `myElement` 是最后一个子元素：

``` js
myElement.parentNode.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$after</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$after</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$remove` <sup>移除 (removed)</sup>

Use the native DOM API:
直接使用原生的 DOM API：

``` js
vm.$el.remove()
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$remove</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$remove</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

## 元实例方法（Meta Instance Methods）

### `vm.$eval` <sup>移除 (removed)</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.
没什么卵用。如果你碰巧依赖了这个特性，而且不知道怎么绕过它，可以在[论坛上](http://forum.vuejs.org/)发帖求助。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$eval</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$eval</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$interpolate` <sup>移除 (removed)</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.
没什么卵用。如果你碰巧依赖了这个特性，而且不知道怎么绕过它，可以在[论坛上](http://forum.vuejs.org/)发帖求助。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$interpolate</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$interpolate</code> 的代码。如果有遗漏，你应该能看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

### `vm.$log` <sup>移除 (removed)</sup>

Use the [Vue Devtools](https://github.com/vuejs/vue-devtools) for the optimal debugging experience.
使用 [Vue Devtools](https://github.com/vuejs/vue-devtools) 来获得最佳的调试体验。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$log</code>. If you miss any, you'll see <strong>console errors</strong>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>vm.$log</code> 的代码。如果有遗漏，你应该会看到<strong>控制台报错</strong>。</p>
</div>
{% endraw %}

## 实例的 DOM 选项（Instance DOM Options）

### `replace: false` <sup>移除 (removed)</sup>

Components now always replace the element they're bound to. To simulate the behavior of `replace: false`, you can wrap your root component with an element similar to the one you're replacing. For example:
现在，组件会总是取代绑定的元素。要模拟 `replace: false` 的行为，你可以用一个类似要取代元素的元素，包裹你的根组件。例如：

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

Or with a render function:
或者用渲染函数：

``` js
new Vue({
  el: '#app',
  render: function (h) {
    h('div', {
      attrs: {
        id: 'app',
      }
    }, /* ... */)
  }
})
```

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>replace: false</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>replace: false</code> 的代码。</p>
</div>
{% endraw %}

## 全局配置（Global Config）

### `Vue.config.debug` <sup>移除 (removed)</sup>

No longer necessary, since warnings come with stack traces by default now.
不再需要，现在的警告默认带有堆栈追踪信息。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.debug</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.config.debug</code> 的代码。</p>
</div>
{% endraw %}

### `Vue.config.async` <sup>移除 (removed)</sup>

Async is now required for rendering performance.
如果要追求渲染性能，异步是必需的。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.async</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.config.async</code> 的代码。</p>
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>取代 (replaced)</sup>

This has been reworked as a [component-level option](../api/#delimiters). This allows you to use alternative delimiters within your app without breaking 3rd-party components.
这已经被修改为[组件级的选项](../api/#delimiters)。这可以让你在应用内使用替代的分隔符，而不会影响第三方组件。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.delimiters</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.config.delimiters</code> 的代码。</p>
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>移除 (removed)</sup>

 HTML interpolation has been [removed in favor of `v-html`](#HTML-Interpolation-removed).
 HTML 插值已经被[移除，你应该使用 `v-html`](#HTML-Interpolation-deprecated)。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.unsafeDelimiters</code>. After this, the helper will also find instances of HTML interpolation so that you can replace them with `v-html`.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.config.unsafeDelimiters</code> 的代码。然后，迁移工具还会找到包含 HTML 插值的地方，这样就可以用 `v-html` 来替换。</p>
</div>
{% endraw %}

## 全局API(Global API)

### `Vue.extend` 与`el` (`Vue.extend` with `el`)<sup>移除 (removed)</sup>

The el option can no longer be used in `Vue.extend`. It's only valid as an instance creation option.
`Vue.extend` 不再能使用 `el` 选项。只有在实例创建时 `el` 才是一个合法的选项。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about the <code>el</code> option with <code>Vue.extend</code>.<br>在升级后，运行你的端对端测试或者直接运行应用，查看<strong>控制台警告</strong>中关于在 <code>Vue.extend</code> 中使用 <code>el</code> 选项的信息。</p>
</div>
{% endraw %}

### `Vue.elementDirective` <sup>移除 (removed)</sup>

Use components instead.
用组件取代。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.elementDirective</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 <code>Vue.elementDirective</code> 的代码。</p>
</div>
{% endraw %}

### `Vue.partial` <sup>移除 (removed)</sup>

Partials have been removed in favor of more explicit data flow between components, using props. Unless you're using a partial in a performance-critical area, the recommendation is to simply use a [normal component](components.html) instead. If you were dynamically binding the `name` of a partial, you can use a [dynamic component](components.html#Dynamic-Components).
partial 已经被移除，你应该使用属性来让组件间的数据流更加明晰。除非你在追求性能的场合使用 partial，否则你应该就用[普通组件](components.html)。如果你之前动态绑定了 partial 的 `name`，你可以使用[动态组件](components.html#Dynamic-Components)。

If you happen to be using partials in a performance-critical part of your app, then you should upgrade to [functional components](render-function.html#Functional-Components). They must be in a plain JS/JSX file (rather than in a `.vue` file) and are stateless and instanceless, just like partials. This makes rendering extremely fast.
如果你刚好是在应用中追求性能的部分使用 partial，那么你应该升级到[函数式组件](render-function.html#Functional-Components)。它们应该被放在一个 JS/JSX 文件里（而不是一个 `.vue` 文件），而且应该是像 partial 一样无状态，不需要实例化的。这会让渲染变的超级快。

A benefit of functional components over partials is that they can be much more dynamic, because they grant you access to the full power of JavaScript. There is a cost to this power however. If you've never used a component framework with render functions before, they may take a bit longer to learn.
函数式组件相对 partial 的一个优点是，函数式组件更加动态，因为它们让你可以发挥 JavaScript 的全部能力。不过，获得这种能力也是有代价的。如果你从来没有用过一个使用渲染函数的组件框架，你需要花一点时间来学习。

{% raw %}
<div class="upgrade-path">
  <h4>升级方法 (Upgrade Path)</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.partial</code>.<br>在项目中运行 <a href="https://github.com/vuejs/vue-migration-helper">迁移工具</a>，找到包含 `Vue.partial` 的代码。</p>
</div>
{% endraw %}
