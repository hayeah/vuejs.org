---
title: 升级Vue 1.x（Migration from Vue 1.x）
type: guide
order: 24
---

## 常见问题解答（FAQ）

> Woah - this is a super long page! Does that mean 2.0 is completely different, I'll have to learn the basics all over again, and migrating will be practically impossible?
哇-这是超级长的一页！这是否意味着2.0已经是完全不同的版本，我不得不从头学习全部基础知识呢？

I'm glad you asked! The answer is no. About 90% of the API is the same and the core concepts haven't changed. It's long because we like to offer very detailed explanations and include a lot of examples. Rest assured, __this is not something you have to read from top to bottom!__
我很高兴的回答你！这个答案是，不用。大约90%的API是相同的，它的核心概念同时也没有被改变。如此长的篇幅，是因为我们更想为你提供非常详细的解析，包括大量的例子。请放心，__这里并没有什么是需要你从头到尾去研读的。__

> Where should I start in a migration?
应该从那里开始升级？

1. Start by running the [migration helper](https://github.com/vuejs/vue-migration-helper) on a current project. We've carefully minified and compressed a senior Vue dev into a simple command line interface. Whenever they recognize a deprecated pattern, they'll let you know, offer suggestions, and provide links to more info.
在当前的项目中开始运行[升级帮助者(migration helper)](https://github.com/vuejs/vue-migration-helper)。我们将会非常小心的缩小和压缩一个大的Vue dev 成为一个简单的命令行接口。无论何时，他识别到deprecated 样式，就会提示你，并提供建议，以及更多信息的链接。

2. After that, browse through the table of contents for this page in the sidebar. If you see a topic you may be affected by, but the migration helper didn't catch, check it out.
然后，浏览这个页面的目录栏。如果你看到一个标题可能会影响到你的，但是[升级帮助者(migration helper)](https://github.com/vuejs/vue-migration-helper)并没有捕获到，那就检查它。

3. If you have any tests, run them and see what still fails. If you don't have tests, just open the app in your browser and keep an eye out for warnings or errors as you navigate around.
如果你有很多的测试用例，运行他们，看看失败的地方。如过你没有测试用例，那就尝试在浏览器中打开你的应用程序,留意浏览器发出的警告和错误。

4. By now, your app should be fully migrated. If you're still hungry for more though, you can read the rest of this page - or just dive in to the new and improved guide from [the beginning](index.html). Many parts will be skimmable, since you're already familiar with the core concepts.
现在,你的程序应该完全升级。如果你仍然渴望更多，你可以阅读这页剩下的内容。 -  获或者只是想钻研，可以在[开始页](index.html)得到指导。

> How long will it take to migrate a Vue 1.x app to 2.0?
升级Vue 1到2.0 x应用程序需要多长时间?

It depends on a few factors:
<<<<<<< HEAD
这取决于以下几点：
=======
这取决于一下几点：
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

- The size of your app (small to medium-sized apps will probably be less than a day)
应用程序的大小（小到中型的应用程序将花费小于1天的时间）

- How many times you get distracted and start playing with a cool new feature. &nbsp;&nbsp;Not judging, it also happened to us while building 2.0!
多少次，你感到心不在焉的，开始玩一个很酷的新特性。 ðŸ˜‰ &nbsp;没有判断，他发生在我们构建2.0时。

- Which deprecated features you're using. Most can be upgraded with find-and-replace, but others might take a few minutes. If you're not currently following best practices, Vue 2.0 will also try harder to force you to. This is a good thing in the long run, but could also mean a significant (though possibly overdue) refactor.
<<<<<<< HEAD
那些你仍然在使用的废弃特性。大多数可以通过查找和替换升级，但是其他可能要花费几分钟。如果你目前不是遵循最佳实践，Vue 2.0 会也努力约束你。
=======
那些废弃特性你仍然在使用。大多数可以通过查找和替换升级，但是其他可能要花费几分钟。如果你目前不是遵循最佳实践，Vue 2.0 会也努力约束你。
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331


> If I upgrade to Vue 2, will I also have to upgrade Vuex and Vue-Router?
如果我升级到 Vue 2，我也要有升级 Vuex 和 Vue 路由器？

Only Vue-Router 2 is compatible with Vue 2, so yes, you'll have to follow the [migration path for Vue-Router](migration-vue-router.html) as well. Fortunately, most applications don't have a lot of router code, so this likely won't take more than an hour.
只有Vue-Router 2是兼容 Vue 2，所以答案是，是的，你最好遵循[Vue-Router升级方法](migration-vue-router.html)的建议。幸运的是，大多数应用程序并没有大量的路由代码，所以，这不会花费超过一个小时。

As for Vuex, even version 0.8 is compatible with Vue 2, so you're not forced to upgrade. The only reason you may want to upgrade immediately is to take advantage of the new features in Vuex 2, such as modules and reduced boilerplate.
就Vuex来说，版本 0.8 是兼容 Vue 2，所以，你不会强迫被要求更新。您可能希望立即升级的唯一原因是利用Vuex 2中的新功能,例如模块和减少样板。

## 模板（Templates）

### Fragment Instances <sup>deprecated</sup>
### 片段实例 <sup>弃用</sup>

Every component must have exactly one root element. Fragment instances are no longer allowed. If you have a template like this:
<<<<<<< HEAD
每个组件必须有一个根元素。不再允许片段实例。如果你有一个模版想如下这样：
=======
每个组件必须有一个根元素。不再允许片段实例。如果你有一个模版想如下这样：（Bad）
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

``` html
<p>foo</p>
<p>bar</p>
```

It's recommended to simply wrap the entire contents in a new element, like this:
<<<<<<< HEAD
建议简单包装的完整内容的新元素,像这样:
=======
建议简单包装的完整内容的新元素,像这样:(Good)
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about multiple root elements in a template.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在升级后，重头到尾运行你的测试用例 或 应用程序 ，查看<strong>控制台警告</strong> 警告关于模版存在多个根元素</p>的信息。
=======
  <p>在升级后，重头到尾运行你的测试用例 或 应用程序 ，查看<strong>console warnings</strong> about multiple root elements in a template.</p>的信息。
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

## 生命周期钩子（Lifecycle Hooks）

### `beforeCompile` <sup>deprecated</sup>
### `beforeCompile` <sup>弃用</sup>


Use the `created` hook instead.
<<<<<<< HEAD
使用 `created`钩子代替
=======
使用 `created` 代替
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级路径</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

### `compiled` <sup>deprecated</sup>
### `compiled` <sup>弃用</sup>

Use the new `mounted` hook instead.
用新的钩子 `mounted` 代替

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

### `attached` <sup>deprecated</sup>
### `attached` <sup>弃用</sup>

Use a custom in-dom check in other hooks. For example, to replace:
<<<<<<< HEAD
在其他钩子使用自定义dom检查。举一个例子,替换:
=======
在其他钩子使用自定义dom检查。例如,替换:
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

``` js
attached: function () {
  doSomething()
}
```

You could use:
你可以这样用：

``` js
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

### `detached` <sup>deprecated</sup>
### `detached` <sup>弃用</sup>

Use a custom in-dom check in other hooks. For example, to replace:
用自定义的 dom 中 检查其他钩子，如，若要替换︰

``` js
detached: function () {
  doSomething()
}
```

You could use:
你可以这样用：

``` js
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

### `init` <sup>deprecated</sup>
### `init` <sup>弃用</sup>

Use the new `beforeCreate` hook instead, which is essentially the same thing. It was renamed for consistency with other lifecycle methods.
使用新的钩子 `beforeCreate` 代替，这在本质上是相同的。它被命名为与其他生命周期命名方法的一致性。 

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

### `ready` <sup>deprecated</sup>
### `ready` <sup>弃用</sup>

Use the new `mounted` hook instead. It should be noted though that with `mounted`, there's no guarantee to be in-document. For that, also include `Vue.nextTick`/`vm.$nextTick`. For example:
<<<<<<< HEAD
使用新的钩子 `mounted` 代替。应该注意 `mounted`，这里并没有保证在文档内。因此，包含`Vue.nextTick`/`vm.$nextTick`。举个例子：
=======
使用新的钩子 `mounted` 代替。应该注意 `mounted`，这里并没有保证文档内。
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

``` js
mounted: function () {
  this.$nextTick(function () {
    // code that assumes this.$el is in-document
<<<<<<< HEAD
    // 代码假定 this.$el 在 DOM上
=======
    // 代码假定 this.$el 在 dom上
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find all examples of this hook.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>  找到与全部关于这个钩子的代码</p>
</div>
{% endraw %}

## `v-for`

### `v-for` Argument Order for Arrays
### `v-for` 参照Arrays的参数顺序

When including an `index`, the argument order for arrays used to be `(index, value)`. It is now `(value, index)` to be more consistent with JavaScript's native array methods such as `forEach` and `map`.
当包含一个 `index` ， 数组的参数顺序被使用为`(index, value)`， 现在为`(value, index)` 更符合JavaScript原生数组的方法,例如：`forEach` 和 `map`。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated argument order. Note that if you name your index arguments something unusual like <code>position</code> or <code>num</code>, the helper will not flag them.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如，被废弃的参数顺序。注意，是否你的name 和 index 参数有些不寻常的，像<code>position</code> or <code>num</code>,<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>不会识别到他们。</p>
=======
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如，被废弃的参数顺序。注意，是否你的name 和 index 参数有些不寻常的，像<code>position</code> or <code>num</code>,<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>不会识别到他们</p>
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

### `v-for` Argument Order for Objects
### `v-for` 参照Objects的参数顺序

When including a `key`, the argument order for objects used to be `(key, value)`. It is now `(value, key)` to be more consistent with common object iterators such as lodash's.
当包含一个 `key`， 对象的参数顺序被使用为`(key, value)`。现在为 `(value, key)` ，更符合公共对象迭代器，例如：lodash。                      

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated argument order. Note that if you name your key arguments something like <code>name</code> or <code>property</code>, the helper will not flag them.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如，被废弃的参数顺序。注意，是否你的 name你的 key 参数有些像<code>name</code> or <code>property</code>, <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>不会识别到他们。</p>
</div>
{% endraw %}

### `$index` and `$key` <sup>deprecated</sup>
### `$index` 和 `$key` <sup>弃用</sup>

The implicitly assigned `$index` and `$key` variables have been deprecated in favor of explicitly defining them in `v-for`. This makes the code easier to read for developers less experienced with Vue and also results in much clearer behavior when dealing with nested loops.
<<<<<<< HEAD
隐式分配，`$index` 和 `$key` 变量 已经被弃用了，在 `v-for` 中已经明确地定义了它们。这样对那些缺乏经验的Vue开发者来说，代码更加容易阅读，处理嵌套循环时，也会让处理行为产生更加清晰的结果。
=======
隐式分配，`$index` 和 `$key` 变量 已经被废弃了，在 `v-for` 中已经明确地定义了它们。这样对那些缺乏经验的Vue开发者来说，代码更加容易阅读，处理嵌套循环时，也会让处理行为产生更加清晰的结果。
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of these deprecated variables. If you miss any, you should also see <strong>console errors</strong> such as: <code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如，被废弃的变量. 如果你犯错了, 你应该看到 <strong>浏览器控制台输出异常</strong> 例如: <code>Uncaught ReferenceError: $index is not defined</code>这样的信息。</p>
=======
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如，被废弃的变量. 如果你犯错了, 你应该看到 <strong>浏览器控制台输出异常</strong> 例如: <code>Uncaught ReferenceError: $index is not defined</code></p>
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

### `track-by` <sup>deprecated</sup>
### `track-by` <sup>弃用</sup>

`track-by` has been replaced with `key`, which works like any other attribute: without the `v-bind:` or `:` prefix, it is treated as a literal string. In most cases, you'd want to use a dynamic binding which expects a full expression instead of a key. For example, in place of:
`track-by` 已经被 `key` 替代了，他会像其他属性一样工作，没有的`v-bind:` 或 `:` 前缀,它被当作一个文字字符串，在大多数情况下，要使用动态绑定的期望的是一个完整表达式，而不是键。例如,如下所示：

``` html
<div v-for="item in items" track-by="id">
```

You would now write:
你现在这样写：

``` html
<div v-for="item in items" v-bind:key="item.id">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>track-by</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>找到如 <code>track-by</code>.</p>
</div>
{% endraw %}

### `v-for` Range Values
<<<<<<< HEAD
### `v-for` 值的范围
=======
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

Previously, `v-for="number in 10"` would have `number` starting at 0 and ending at 9. Now it starts at 1 and ends at 10.
以前，`v-for="number in 10"`是让`number`从0开始，在9结束。现在他从1开始，结束为10。 

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Search your codebase for the regex <code>/\w+ in \d+/</code>. Wherever it appears in a <code>v-for</code>, check to see if you may be affected.</p>
</div>
<div class="upgrade-path">
  <h4>升级路径</h4>
  <p>用正则表达式查询你的代码库，如：<code>/\w+ in \d+/</code>. 无论在那里出现了一个<code>v-for</code>, 检查是否他会受到影响.</p>
</div>
{% endraw %}

## Props

### `coerce` Prop Option <sup>deprecated</sup>
### `coerce` Prop 操作 <sup>弃用</sup>

If you want to coerce a prop, setup a local computed value based on it instead. For example, instead of:
如果你想要一个prop，设置本地的计算的值，而是在此基础上操作。例如，取反:

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
你应该这样写：

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
有几个优势：

- You still have access to the original value of the prop.
- You are forced to be more explicit, by giving your coerced value a name that differentiates it from the value passed in the prop.
- 你仍然能访问原始值的prop。
- 你被迫更加明确，通过让你强迫一个value的name,不同与value传递过来的prop。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>coerce</code> option.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>查询代码如<code>coerce</code> option.</p>
</div>
{% endraw %}

### `twoWay` Prop Option <sup>deprecated</sup>
### `twoWay` Prop 操作 <sup>弃用</sup>


Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:
Props 现在总是 单向下来的，产生的副作用在父范围内。一个组件需要显式地发出一个事件,而不是依靠隐式绑定。更多的内容请查看：

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state managment](state-management.html)

- [常用组件事件](components.html#Custom-Events)
- [常用输入组件](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>twoWay</code> option.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>查询代码如<code>twoWay</code> option.</p>
</div>
{% endraw %}

### `v-bind` with `.once` and `.sync` Modifiers <sup>deprecated</sup>
### `v-bind` 与 `.once` 和 `.sync` 修饰符 <sup>弃用</sup>

Props are now always one-way down. To produce side effects in the parent scope, a component needs to explicitly emit an event instead of relying on implicit binding. For more information, see:
Props 现在总是 单向下来的，产生的副作用在父范围内。一个组件需要显式地发出一个事件,而不是依靠隐式绑定。更多的内容请查看：

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state managment](state-management.html)

- [常用组件事件](components.html#Custom-Events)
- [常用输入组件](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [全局状态管理](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>.once</code> and <code>.sync</code> modifiers.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>查询代码如<code>.once</code> and <code>.sync</code> 修改。</p>
</div>
{% endraw %}

### Prop Mutation <sup>deprecated</sup>
### Prop 突变 <sup>弃用</sup>

Mutating a prop locally is now considered an anti-pattern, e.g. declaring a prop and then setting `this.myProp = 'someOtherValue'` in the component. Due to the new rendering mechanism, whenever the parent component re-renders, the child component's local changes will be overwritten.
本地化的改变 Prop，现在被认为是反模式，e.g。声明一个 prop 接着在组件中设置  `this.myProp = 'someOtherValue'` ，由于新的渲染机制, 无论父组件在何时重新渲染，子组件本地改变将会被重写。

Most use cases of mutating a prop can be replaced by one of these options:
更多的突变prop案例能被以下的一些操作代替：

- a data property, with the prop used to set its default value
- a computed property
- 支持的数据属性,用于设置默认值
- 一个计算的属性

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about prop mutations.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在升级后，从头到尾运行测试用例 或 应用程序，查看 <strong>控制台输出警告</strong> 关于 prop 改变.</p>
</div>
{% endraw %}

### Props on a Root Instance <sup>deprecated</sup>
### Props 在根的实例 <sup>弃用</sup>

On root Vue instances (i.e. instances created with `new Vue({ ... })`), you must use `propsData` instead instead of `props`.
在Vue的根实例中（例如：实例会被 `new Vue({ ... })` 创建出来），你必须使用 `propsData` 代替`props`。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that props passed to root instances are no longer working.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>从头到尾运行你的测试用例, 如果你有测试用例. 这个<strong>失败测试</strong> 应该警告这个事实，props 被传递到根实例上已经不在被允许了。</p>
</div>
{% endraw %}

## 内置指令（Built-In Directives）

### Truthiness/Falsiness with `v-bind`
### `v-bind` 与 真/假 



When used with `v-bind`, the only falsy values are now: `null`, `undefined`, and `false`. This means `0` and empty strings will render as truthy. So for example, `v-bind:draggable="''"` will render as `draggable="true"`.
当你使用`v-bind`， 现在能唯一产生false的值为：`null`, `undefined`, and `false`。这将意味这'0' 和空字符串将会被渲染为true。所以例如：`v-bind:draggable="''"` 将会被渲染为`draggable="true"` 

For enumerated attributes, in addition to the falsy values above, the string `"false"` will also render as `attr="false"`.
对于枚举属性，除了上面的失败值中,字符串为`"false"`也将会被重新渲染为`attr="false"`

<p class="tip">Note that for other directives (e.g. `v-if` and `v-show`), JavaScript's normal truthiness still applies.</p>
<p class="tip">注意其他的指令 (e.g. `v-if` and `v-show`), JavaScript的常用真值仍然是可适用的。</p>

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>从头到尾运行你的测试用例, 如果你有测试用例. 这个<strong>失败测试</strong> 将会告诉你，你的应用在那一部分，可能会会应为这个改变而受到影响。</p>
</div>
{% endraw %}

### 在组件中使用 `v-on`监听本地事件（Listening for Native Events on Components with `v-on`）

When used on a component, `v-on` now only listens to custom events `$emit`ted by that component. To listen for a native DOM event on the root element, you can use the `.native` modifier. For example:
在使用一个组件时，`v-on` 现在仅仅只会监听组件的自定义的事件发出的信息。

``` html
<my-component v-on:click.native="doSomething"></my-component>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>从头到尾运行你的测试用例, 如果你有测试用例. 这个<strong>失败测试</strong>将会告诉你，你的应用在那一部分，可能会会应为这个改变而受到影响。</p>
</div>
{% endraw %}

### `v-model` with `debounce` <sup>deprecated</sup>
### `v-model` 与 `debounce` <sup>弃用</sup>


Debouncing is used to limit how often we execute Ajax requests and other expensive operations. Vue's `debounce` attribute parameter for `v-model` made this easy for very simple cases, but it actually debounced __state updates__ rather than the expensive operations themselves. It's a subtle difference, but it comes with limitations as an application grows.
消除抖动是用来限制我们执行Ajax请求的次数,和其他无谓的操作。Vue中`v-model` 的属性参数`debounce`会让这种实现变的简单，但是它实际上防反抖动状态更新,而不是无谓的操作。

These limitations become apparent when designing a search indicator, like this one for example:
这个限制抖动的非常明显的例子是，在设计一个搜索指标，像如下的例子：

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
使用`debounce`属性，就没有办法检测到"Typing"状态，因为我们失去访问输入的实时状态，然而，通过从Vue代码中解耦出防止抖动函数，我们就能实现防止抖动的那些操作是仅仅是我们所控制的。

``` html
<!--
By using the debounce function from lodash or another dedicated
utility library, we know the specific debounce implementation we
use will be best-in-class - and we can use it ANYWHERE. Not just
in our template.
-->
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
这种方法的另一个优点是有时会消除抖动不太正确的包装器函数。例如：当触发一个查询提示的API时，直到用户停止输入的时候才撤销查询提示，这不是一个好的用户体验。你可能想要的是一个__节流__功能来代替。既然你已经像lodash使用一个实用程序库。使用“节流”功能重构，这样就不用等待几秒了。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>debounce</code> attribute.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到例如<code>debounce</code>的属性.</p>
</div>
{% endraw %}

### `v-model` with `lazy` or `number` Param Attributes <sup>deprecated</sup>
### `v-model` 与 `lazy` 或 `number` 参数属性 <sup>弃用</sup>

The `lazy` and `number` param attributes are now modifiers, to make it more clear what That means instead of:
`lazy` 和 `number` 现在是一个修饰符的参数属性，为了让它更明白这意味着什么而不是什么︰

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

You would use:
你更会喜欢这样写：

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the these deprecated param attributes.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到例如这些被废弃的参数属性。</p>
</div>
{% endraw %}

### `v-model` with Inline `value` <sup>deprecated</sup>
### `v-model` 与内联 `value` <sup>弃用</sup>


`v-model` no longer cares about the initial value of an inline `value` attribute. For predictability, it will instead always treat the Vue instance data as the source of truth.
`v-model` 不在关心内联属性的初始化值，更可预测性，它不会总把 Vue 实例数据当作可靠数据的来源。

That means this element:
这个元素的意义：

``` html
<input v-model="text" value="foo">
```

backed by this data:
返回的数据：

``` js
data: {
  text: 'bar'
}
```

will render with a value of "bar" instead of "foo". The same goes for a `<textarea>` with existing content. Instead of:
它将会重新渲染 为“bar” 而不是"foo"。这同样适用`<textarea>` 便签中的现有内容。而不是这样： 

``` html
<textarea v-model="text">
  hello world
</textarea>
```

You should ensure your initial value for `text` is "hello world".
你应该明确的知道你初始化的`text`值是"hello world"

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about inline value attributes with <code>v-model</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在升级后，从头到尾运行你的测试用例或应用程序,以及查找<strong>控制台警告</strong> 关于内联属性<code>v-model</code>。</p>
</div>
{% endraw %}

### `v-model` with `v-for` Iterated Primitive Values <sup>deprecated</sup>
### `v-model` 与 `v-for` 迭代原始值 <sup>弃用</sup>


Cases like this no longer work:
这种情况下不再有效:

``` html
<input v-for="str in strings" v-model="str">
```

The reason is this is the equivalent JavaScript that the `<input>` would compile to:
原因是这句相当于JavaScript `<input>` 将被编译为:

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

As you can see, `v-model`'s two-way binding doesn't make sense here. Setting `str` to another value in the iterator function will do nothing because it's just a local variable in the function scope.
你可以看到，`v-model`的双向绑定在这里并不是合理的。设置`str`为另外一个值，在迭代器函数中并不会做任何事情，因为他只是一个函数的局部变量。

Instead, you should use an array of __objects__ so that `v-model` can update the field on the object. For example:
相反，你应该使用一个数组__对象__，因此，`v-model`就能更新这个成员变量到对象中。

``` html
<input v-for="obj in objects" v-model="obj.str">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your test suite, if you have one. The <strong>failed tests</strong> should alert to you to any parts of your app that may be affected by this change.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>如果你有测试用例，运行你的测试用例. 这个<strong>失败测试</strong> 那里会因为这个改变而受到影响。</p>
</div>
{% endraw %}

### `v-bind:style` with Object Syntax and `!important` <sup>deprecated</sup>
### `v-bind:style` 与Object 语法和 `!important` <sup>弃用</sup>


This will no longer work:
这个将不会再有效：

``` html
<p v-bind:style="{ color: myColor + ' !important' }">hello</p>
```

If you really need to override another `!important`, you must use the string syntax:
如果你真想要覆盖另一个`!important`，你必须使用字符串语法：

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">hello</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of style bindings with <code>!important</code> in objects.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 在到对象样式绑定为<code>!important</code>。</p>
</div>
{% endraw %}

### `v-el` and `v-ref` <sup>deprecated</sup>
### `v-el` 和 `v-ref` <sup>弃用</sup>


For simplicity, `v-el` and `v-ref` have been merged into the `ref` attribute, accessible on a component instance via `$refs`. That means `v-el:my-element` would become `ref="myElement"` and `v-ref:my-component` would become `ref="myComponent"`. When used on a normal element, the `ref` will be the DOM element, and when used on a component, the `ref` will be the component instance.
更加简单的，`v-el` 和 `v-ref` 已经被合并为`ref`属性，你可以访问组件实例引用`$refs`。这以为这`v-el:my-element`将会变成`ref="myElement"`，`v-ref:my-component` 会变成`ref="myComponent"`，当使用一般的元素，`ref` 代表为DOM元素，当使用为一个组件，`ref`就代表一个组件实例。

Since `v-ref` is no longer a directive, but a special attribute, it can also be dynamically defined. This is especially useful in combination with `v-for`. For example:
由于`v-ref` 不在是一个指令，但还是一个特殊的属性，他也能被动态的定义，他结合`v-for`使用变的尤其有用，例如：

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

Previously, `v-el`/`v-ref` combined with `v-for` would produce an array of elements/components, because there was no way to give each item a unique name. You can still achieve this behavior by given each item the same `ref`:
之前的`v-el`/`v-ref` 结合`v-for` 将会产生一个元素或组件的数组，因为这里没办法给每个item一个特殊的名称。你仍然只能通过给每个item相同的`ref`来触发这个行为：

``` html
<p v-for="item in items" ref="items"></p>
```

Unlike in 1.x, these `$refs` are not reactive, because they're registered/updated during the render process itself. Making them reactive would require duplicate renders for every change.
不像1.x，`$refs`是没有响应的，因为他们在渲染自身的过程中已经被注册/更新。他们的响应将对每一次改变进行重新渲染。

On the other hand, `$refs` are designed primarily for programmatic access in JavaScript - it is not recommended to rely on them in templates, because that would mean referring to state that does not belong to the instance itself. This would violate Vue's data-driven view model.
在另外一方面, `$refs` 主要被设计为在JavaScript可编程访问的，他并不会推荐使用他们依赖的模版，因为这以为着映射到的状态将不属于这个实例自己。这将违反Vue的数据驱动的视图模型。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>v-el</code> and <code>v-ref</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在你的代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到代码样例为<code>v-el</code>和<code>v-ref</code>的代码.</p>
=======
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>v-el</code> and <code>v-ref</code>.</p>
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

### `v-else` with `v-show` <sup>deprecated</sup>
### `v-else` 与 `v-show` <sup>弃用</sup>


`v-else` no longer works with `v-show`. Use `v-if` with a negation expression instead. For example, instead of:
`v-else` 将不会和`v-show` 一起使用。可以使用`v-if` 逻辑表达式代替。举一个反例：

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

You can use:
你可以这样：

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-else</code> with <code>v-show</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在你的代码库中运行 <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> 找到代码为 带 <code>v-show</code>的<code>v-else</code>代码。</p>
=======
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-else</code> with <code>v-show</code>.</p>
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

## 常用指令（Custom Directives）

Directives have a greatly reduced scope of responsibility: they are now only used for applying low-level direct DOM manipulations. In most cases, you should prefer using components as the main code-reuse abstraction.
指令已经大大减少责任的范围:他们现在只用于申请低级直接DOM操作。在更多的案例，你应该喜欢使用组件的主要是因为代码重用的抽象。

Some of the most notable differences include:
一些最显著的差异包括:

- Directives no longer have instances. This means there's no more `this` inside directive hooks. Instead, they receive everything they might need as arguments. If you really must persist state across hooks, you can do so on `el`.
- Options such as `acceptStatement`, `deep`, `priority`, etc are all deprecated.
- Some of the current hooks have different behavior and there are also a couple new hooks.

指令不再有实例。这意味着，不会有更多的`this`在指令钩子里面出现，相反，他们接受一切可能需要作为参数的值。如果你真的必须维持钩子之间的状态，你可以在`el`中这样做：
- 操作这些，`acceptStatement`, `deep`, `priority`等等全部被废弃了。
- 些当前有不同行为的钩子,他们也会被认为是新的钩子。

Fortunately, since the new directives are much simpler, you can master them more easily. Read the new [Custom Directives guide](custom-directive.html) to learn more.
幸运的是，由于新的指令是如此的简单，你可以更加简单的控制他们。阅读新的[Custom Directives guide](custom-directive.html)学习更多。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of defined directives. The helper will flag all of them, as it's likely in most cases that you'll want to refactor to a component.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找到定义指令的代码样例。帮助者会识别他们, 有可能在大多数情况下,你会想要重构一个组件</p>
</div>
{% endraw %}

## 转换（Transitions）

### `transition` Attribute <sup>deprecated</sup>
### `transition` 属性 <sup>弃用</sup>


Vue's transition system has changed quite drastically and now uses `<transition>` and `<transition-group>` wrapper elements, rather than the `transition` attribute. It's recommended to read the new [Transitions guide](transitions.html) to learn more.
Vue的转换系统已经被完全改变了，现在使用`<transition>` 和 `<transition-group>` 包裹元素，而不是`transition`属性。这里推荐阅读新的[Transitions guide](transitions.html)，这里能学到更多。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
   <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找到定义指令的代码样例 <code>transition</code> 属性。</p>
</div>
{% endraw %}

### `Vue.transition` for Reusable Transitions <sup>deprecated</sup>
<<<<<<< HEAD
### 可重用的`Vue.transition` <sup>弃用</sup>

=======
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

With the new transition system, you can now just [use components for reusable transitions](http://rc.vuejs.org/guide/transitions.html#Reusable-Transitions).
现在新的转换系统，你只能[适应组件重用转化](http://rc.vuejs.org/guide/transitions.html#Reusable-Transitions).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.transition</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找到定义指令的代码样例<code>Vue.transition</code>.</p>
</div>
{% endraw %}

### Transition `stagger` Attribute <sup>deprecated</sup>
### 转换`stagger` 属性 <sup>弃用</sup>


If you need to stagger list transitions, you can control timing by setting and accessing a `data-index` (or similar attribute) on an element. See [an example here](transitions.html#Staggering-List-Transitions).

如果你需要错开转换列表，你可以在一个元素上设置反问`data-index`（或相似的属性）。查看[一个具体的例子](transitions.html#Staggering-List-Transitions).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transition</code> attribute. During your update, you can transition (pun very much intended) to the new staggering strategy as well.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找到定义指令的代码样例<code>transition</code>属性. 在更新后, 你能转换为一个新的切换策略。</p>
</div>
{% endraw %}

## 事件（Events）

### `Vue.directive('on').keyCodes` <sup>deprecated</sup>
### `Vue.directive('on').keyCodes` <sup>弃用</sup>


The new, more concise way to configure `keyCodes` is through`Vue.config.keyCodes`. For example:
最新的，更多的关注通过`Vue.config.keyCodes`配置`keyCodes`，例如：

``` js
// enable v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the the old <code>keyCode</code> configuration syntax.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找旧的 <code>keyCode</code>配置项目。</p>
</div>
{% endraw %}

### `$dispatch` and `$broadcast` <sup>deprecated</sup>
### `$dispatch` 和 `$broadcast` <sup>弃用</sup>


`$dispatch` and `$broadcast` are being deprecated in favor of more explicitly cross-component communication and more maintainable state management solutions, such as [Vuex](https://github.com/vuejs/vuex).
`$dispatch` 和 `$broadcast` 已经被废弃，取代的是更多明确跨组件的沟通和维护状态管理解决方案，例如[Vuex](https://github.com/vuejs/vuex)。

The problem is event flows that depend on a component's tree structure can be hard to reason about and very brittle when the tree becomes large. It simply doesn't scale well and we don't want to set you up for pain later. `$dispatch` and `$broadcast` also do not solve communication between sibling components.
问题是取决于组件的树状结构的事件流，当树变得很大，他将变的很脆弱。


For the simplest possible upgrade from `$dispatch` and `$broadcast`, you can use a centralized event hub that allows components to communicate no matter where they are in the component tree. Because Vue instances implement an event emitter interface, you can actually use an empty Vue instance for this purpose.
最简单的升级`$dispatch` 和 `$broadcast`，您可以使用一个集中的活动中心,允许组件通信，不管他们在组件树的那个位置。因为Vue实例实现事件发射器接口，你可以使用一个空的Vue实例。

For example, let's say we have a todo app structured like this:
例如，假设我们有一个 _todo应用_ 结构是这样的:

```
Todos
|-- NewTodoInput
|-- Todo
    |-- DeleteTodoButton
```

We could manage communication between components with this single event hub:
我们能使用单一的事件中心来管理组件间的交流。

``` js
// This is the event hub we'll use in every
// component to communicate between them.
var eventHub = new Vue()
```

Then in our components, we can use `$emit`, `$on`, `$off` to emit events, listen for events, and clean up event listeners, respectively:
然后在我们的组件，我们能分别使用`$emit`，`$on`，`$off` 来发射事件，监听事件，以及清除事件监听者

``` js
// NewTodoInput
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
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// Todos
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// It's good to clean up event listeners before
// a component is destroyed.
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
这种模式可以在简单的场景中替代`$dispatch` 和 `$broadcast`

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>$dispatch</code> and <code>$broadcast</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在你的代码库运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on 找到定义指令的代码样例<code>$dispatch</code> 和 <code>$broadcast</code>。</p>
</div>
{% endraw %}

## 过滤器（Filters）

### Filters Outside Text Interpolations <sup>deprecated</sup>
### 过滤器外部文本嵌入 <sup>弃用</sup>


Filters can now only be used inside text interpolations (`{% raw %}{{ }}{% endraw %}` tags). In the past we've found using filters within directives such as `v-model`, `v-on`, etc led to more complexity than convenience. For list filtering on `v-for`, it's also better to move that logic into JavaScript as computed properties, so that it can be reused throughout your component.
过滤器只能使用文本拦截(`{% raw %}{{ }}{% endraw %}` 标签)。在过去,我们发现使用过滤器都是在指令中，例如：`v-model`, `v-on`, 等，导致一些比行为更加复杂的东西。对过滤列表使用`v-for`,如果一致逻辑到computed 属性中 他会表现更加好的。

In general, whenever something can be achieved in plain JavaScript, we want to avoid introducing a special syntax like filters to take care of the same concern. Here's how you can replace Vue's built-in directive filters:
一般来说，每当有困难的东西可以在纯JavaScript实现，我们会避免介绍一个特殊的语法就像过滤器一样而不是仅仅关心的相同的概念。

#### Replacing the `debounce` Filter
#### 代替 `debounce` 过滤器


Instead of:
而不是这样：

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
使用[lodash's `debounce`](https://lodash.com/docs/4.15.0#debounce) (or possibly [`throttle`](https://lodash.com/docs/4.15.0#throttle))直接限制调用昂贵的方法。

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

For more on the advantages of this strategy, see [the example here with `v-model`](#v-model-with-debounce-deprecated).
关于更多这个策略上的优势，参看[the example here with `v-model`](#v-model-with-debounce-deprecated)。

#### Replacing the `limitBy` Filter

Instead of:
<<<<<<< HEAD
而不是这样：
=======
而不是：
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

Use JavaScript's built-in [`.slice` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) in a computed property:
在 computed 属性用JavaScript的绑定[`.slice` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)

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

#### Replacing the `filterBy` Filter

Instead of:
<<<<<<< HEAD
不再是这样：
=======
不再是：
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

Use JavaScript's built-in [`.filter` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) in a computed property:
在 computed 属性用JavaScript的绑定[`.filter` 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples)


``` html
<p v-for="user in filteredUsers">{{ user.name }}</p>
```

``` js
computed: {
  filteredUsers: function () {
    return this.users.filter(function (user) {
      return user.name.indexOf(this.searchQuery)
    })
  }
}
```

JavaScript's native `.filter` can also manage much more complex filtering operations, because you have access to the full power of JavaScript within computed properties. For example, if you wanted to find all active users and case-insensitively match against both their name and email:
JavaScript的本地`.filter` 也能管理更多复杂的过滤操作。因为你有访问JavaScript内的全部有力的计算属性。例如：如果你想找到所有活跃用户和不区分大小写的匹配他们的名字和电子邮件:

``` js
this.users.filter(function (user) {
  var searchRegex = new RegExp(this.searchQuery, 'i')
  return user.isActive && (
    searchRegex.test(user.name) ||
    searchRegex.test(user.email)
  )
})
```

#### Replacing the `orderBy` Filter
#### 取代`orderBy` 过滤器

Instead of:
而不是这样：

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

Use [lodash's `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (or possibly [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)) in a computed property:
在一个计算属性中使用[lodash's `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (or 属性 [`sortBy`](https://lodash.com/docs/4.15.0#sortBy))

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
你甚至可以按多个列:

``` js
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters being used inside directives. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of filters being used inside directives. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### Filter Argument Syntax
### Filter 参数语法糖


Filters' syntax for arguments now better aligns with JavaScript function invocation. So instead of taking space-delimited arguments:
滤波器参数的语法现在更好的与JavaScript函数调用匹配。所以替换掉空格分隔参数:

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

We surround the arguments with parentheses and delimit the arguments with commas:
我们围绕参数用括号和分割参数用逗号:

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the old filter syntax. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### Built-In Text Filters <sup>deprecated</sup>
### 构建文本过滤器<sup>弃用</sup>


Although filters within text interpolations are still allowed, all of the filters have been removed. Instead, it's recommended to use more specialized libraries for solving problems in each domain (e.g. [`date-fns`](https://date-fns.org/) to format dates and [`accounting`](http://openexchangerates.github.io/accounting.js/) for currencies).
尽管过滤器内文本插入仍然是被允许的，但所有的过滤器已被移除了。相反，它的建议解决问题在每个域中的使用更多的专业的库。

For each of Vue's built-in text filters, we go through how you can replace them below. The example code could exist in custom helper functions, methods, or computed properties.
对于每一个Vue的绑定文本过滤器，下面我们浏览下如何能取代它们的实现。示例代码可能存在于自定义辅助函数,计算方法或属性。

#### Replacing the `json` Filter
#### 取代 `json` 过滤器


You actually don't need to for debugging anymore, as Vue will nicely format output for you automatically, whether it's a string, number, array, or plain object. If you want the exact same functionality as JavaScript's `JSON.stringify` though, then you can use that in a method or computed property.
你真的不需要进行调试了，Vue将会为你自动的输出漂亮的格式信息。不过你是字符串还是数组，亦或是简单的对象。如果你想要一个和JavaScript的 `JSON.stringify`完全一样的功能，你可以使用一个method或computed属性。

#### Replacing the `capitalize` Filter

``` js
text[0].toUpperCase() + text.slice(1)
```

#### Replacing the `uppercase` Filter
#### 取代大写 `uppercase`过滤器


``` js
text.toUpperCase()
```

#### Replacing the `lowercase` Filter
#### 取代小写 `lowercase`过滤器


``` js
text.toLowerCase()
```

#### Replacing the `pluralize` Filter
#### 取代 `pluralize` 过滤器


The [pluralize](https://www.npmjs.com/package/pluralize) package on NPM serves this purpose nicely, but if you only want to pluralize a specific word or want to have special output for cases like `0`, then you can also easily define your own pluralize functions. For example:
[pluralize](https://www.npmjs.com/package/pluralize)包在NPM服务中符合这个令人满意的目的，但如果你只是想一个特定的词复数化或想要特殊的输出情况， 比如‘0’，然后你还可以方便地定义自己的复数函数。举个例子：

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

#### Replacing the `currency` Filter
#### 取代`currency` 过滤器


For a very naive implementation, you could just do something like this:
对于一个简单的实现，你只需要像下面那样做：

``` js
'$' + price.toFixed(2)
```

In many cases though, you'll still run into strange behavior (e.g. `0.035.toFixed(2)` rounds up to `0.4`, but `0.045` rounds down to `0.4`). To work around these issues, you can use the [`accounting`](http://openexchangerates.github.io/accounting.js/) library to more reliably format currencies.
虽然在1许多情况下，你仍然会遇到奇怪的行为(e.g. `0.035.toFixed(2)`向上舍入为`0.4`，但实际`0.045`向下舍入为 `0.4`）。为了解决这些问题，你可以使用[`accounting`](http://openexchangerates.github.io/accounting.js/) 库实现更加可靠的货币格式。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated text filters. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## 插槽（Slots）

### Duplicate Slots <sup>deprecated</sup>
### 重复的插槽 <sup>弃用</sup>


It is no longer supported to have `<slot>`s with the same name in the same template. When a slot is rendered it is "used up" and cannot be rendered elsewhere in the same render tree. If you must render the same content in multiple places, pass that content as a prop.
现在不再支持在template使用使用相同名称的`<slot>`标签。当一个slot被渲染，他就是正在使用，而不能同一个渲染树中渲染其他。如果你必须在多个位置呈现相同的内容,使用prop传递这些内容。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about duplicate slots <code>v-model</code>.</p>
</div>
{% endraw %}

### `slot` Attribute Styling <sup>deprecated</sup>
### `slot`属性样式<sup>弃用</sup>


Content inserted via named `<slot>` no longer preserves the `slot` attribute. Use a wrapper element to style them, or for advanced use cases, modify the inserted content programmatically using [render functions](render-function.html).
通过一名字嵌入内容到`<slot>`将不在保留`<slot>`属性。使用包装元素来改变它们的样式，或用高级用例，使用[render functions](render-function.html)通过编程方式修改插入的内容。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find CSS selectors targeting named slots (e.g. <code>[slot="my-slot-name"]</code>).</p>
</div>
{% endraw %}

## 特殊属性（Special Attributes）

### `keep-alive` Attribute <sup>deprecated</sup>
### `keep-alive` 属性<sup>弃用</sup>


`keep-alive` is no longer a special attribute, but rather a wrapper component, similar to `<transition>`. For example:
`keep-alive`已经不在是一个特殊的属性，而是一个包裹组件，与`<transition>`类似。举个例子：

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

This makes it possible to use `<keep-alive>` on multiple conditional children:
在多个子条件中，更加可能使用`<keep-alive>`

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">When `<keep-alive>` has multiple children, they should eventually evaluate to a single child. Any child other than the first one will simply be ignored.</p>

When used together with `<transition>`, make sure to nest it inside:
在和`<transition>`一起使用的时候，确保被包裹在里面。

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find <code>keep-alive</code> attributes.</p>
</div>
{% endraw %}

## 插值（Interpolation）

### 嵌入属性<sup>弃用</sup>

Interpolation within attributes is no longer valid. For example:
嵌入属性已经不在有效，例如：

``` html
<button v-bind:class="btn btn-{{ size }}"></button>
```

Should either be updated to use an inline expression:
应该被更新为使用内联表达式:

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

Or a data/computed property:
或者 data/computed 属性

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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of interpolation used within attributes.</p>
</div>
{% endraw %}

### HTML Interpolation <sup>deprecated</sup>
### HTML 嵌入 <sup>弃用</sup>


HTML interpolations (`{% raw %}{{{ foo }}}{% endraw %}`) have been deprecated in favor of the [`v-html` directive](/api/#v-html).
HTML嵌入(`{% raw %}{{{ foo }}}{% endraw %}`)已经被弃用[`v-html` directive](/api/#v-html)。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find HTML interpolations.</p>
</div>
{% endraw %}

### One-Time Bindings <sup>deprecated</sup>
### 一次性绑定<sup>弃用</sup>

One time bindings (`{% raw %}{{* foo }}{% endraw %}`) have been deprecated in favor of the new [`v-once` directive](/api/#v-once).
一次绑定(`{% raw %}{{* foo }}{% endraw %}`) 在新的[`v-once` directive](/api/#v-once)中已经被废用了。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find one-time bindings.</p>
</div>
{% endraw %}

## 反应性（Reactivity）

### `vm.$watch`

Watchers created via `vm.$watch` are now fired before the associated component rerenders. This gives you the chance to further update state before the component rerender, thus avoiding unnecessary updates. For example, you can watch a component prop and update the component's own data when the prop changes.
Watchers 是由`vm.$watch` 创建的,现在解除了之前关联组件的重新渲染问题。这能让你在组件被重新渲染前能更早的更新状态，这样能避免不必要的更新。例如，当prop变更时，你能监控到组件的prop,以及更新组件所拥有的data。

If you were previously relying on `vm.$watch` to do something with the DOM after a component updates, you can instead do so in the `updated` lifecycle hook.
如果你以前依赖 `vm.$watch`，在组件更新后，来做一些Dom操作，你可以使用生命周期的钩子来代替实现。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite, if you have one. The <strong>failed tests</strong> should alert to you to the fact that a watcher was relying on the old behavior.</p>
</div>
{% endraw %}

### `vm.$set`

The former `vm.$set` behavior has been deprecated and it is now just an alias for [`Vue.set`](/api/#Vue-set).
之前的`vm.$set`已经被废用，现在被改名为[`Vue.set`](/api/#Vue-set).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated usage.</p>
</div>
{% endraw %}

### `vm.$delete`

The former `vm.$delete` behavior has been deprecated and it is now just an alias for [`Vue.delete`](/api/#Vue-delete)
以前的`vm.$delete` 行为已经被废弃，现在他被改名为[`Vue.delete`](/api/#Vue-delete)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the deprecated usage.</p>
</div>
{% endraw %}

### `Array.prototype.$set`  <sup>deprecated</sup>
### `Array.prototype.$set`  <sup>弃用</sup>

use Vue.set instead
使用Vue.set替代

(console error, migration helper)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$set</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>deprecated</sup>
### `Array.prototype.$remove` <sup>弃用</sup>


Use `Array.prototype.splice` instead. For example:
使用`Array.prototype.splice` 代替。例如：

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

Or better yet, just pass removal methods an index:
或者更好的是,通过删除索引方法:

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>.$remove</code> on an array. If you miss any, you should see <strong>console errors</strong> from the missing method.</p>
</div>
{% endraw %}

### `Vue.set` and `Vue.delete` on Vue instances <sup>deprecated</sup>
### `Vue.set` 和 `Vue.delete`在 Vue 实例化中<sup>弃用</sup>

Vue.set and Vue.delete can no longer work on Vue instances. It is now mandatory to properly declare all top-level reactive properties in the data option. If you'd like to delete properties on a Vue instance or its `$data`, just set it to null.
在Vue实力华中，Vue.set 和 Vue.delete 已经不在被使用。现在强制性的正确声明在数据操作中所有顶级活性属性，如果你想在Vue实例中删除属性或他的`$data`，你只需要把他设置为null。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.set</code> or <code>Vue.delete</code> on a Vue instance. If you miss any, they'll trigger <strong>console warnings</strong>.</p>
</div>
{% endraw %}

### Replacing `vm.$data` <sup>deprecated</sup>
### 替换 `vm.$data` <sup>弃用</sup>

It is now prohibited to replace a component instance's root $data. This prevents some edge cases in the reactivity system and makes the component state more predictable (especially with type-checking systems).
现在禁止替换组件实例的根 $data,这样可以防止一些边界情况的反应系统,使组件状态更容易被预测。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of overwriting <code>vm.$data</code>. If you miss any, <strong>console warnings</strong> will be emitted.</p>
</div>
{% endraw %}

### `vm.$get`<sup>deprecated</sup>
### `vm.$get`<sup>弃用</sup>

Just retrieve reactive data directly.
直接检索活性数据。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$get</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## DOM聚焦实例化方法（DOM-Focused Instance Methods）
<<<<<<< HEAD

### `vm.$appendTo`<sup>deprecated</sup>
### `vm.$appendTo`<sup>弃用</sup>
=======
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331


Use the native DOM API:
使用简单的DOM API:

``` js
myElement.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$appendTo</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$before` <sup>deprecated</sup>
### `vm.$before` <sup>弃用</sup>

Use the native DOM API:
使用简单的 DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$before</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$after` <sup>deprecated</sup>
### `vm.$after` <sup>弃用</sup>

Use the native DOM API:
使用简单的 DOM API：

``` js
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
```

Or if `myElement` is the last child:
或者 `myElement` 是最后一个孩子：

``` js
myElement.parentNode.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$after</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$remove` <sup>deprecated</sup>
### `vm.$remove` <sup>弃用</sup>

Use the native DOM API:
使用简单的 DOM API：

``` js
vm.$el.remove()
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$remove</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## 元实例方法（Meta Instance Methods）

### `vm.$eval` <sup>deprecated</sup>
### `vm.$eval` <sup>弃用</sup>


No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.
没有真正的使用。如果你碰巧依靠这个特性在某种程度上和不确定如何解决它。参考[the forum](http://forum.vuejs.org/)是个不错的主义。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$eval</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$interpolate` <sup>deprecated</sup>
### `vm.$interpolate` <sup>弃用</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](http://forum.vuejs.org/) for ideas.
没有真正的使用。如果你碰巧依靠这个特性在某种程度上和不确定如何解决它。参考[the forum](http://forum.vuejs.org/)是个不错的主义。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$interpolate</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$log` <sup>deprecated</sup>
### `vm.$log` <sup>弃用</sup>

Use the [Vue Devtools](https://github.com/vuejs/vue-devtools) for the optimal debugging experience.
使用[Vue Devtools](https://github.com/vuejs/vue-devtools) 来选择最优的调试体验。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>vm.$log</code>. If you miss any, you'll see <strong>console errors</strong>.</p>
</div>
{% endraw %}

## 实例化DOM操作（Instance DOM Options）

### `replace: false` <sup>deprecated</sup>
### `replace: false` <sup>弃用</sup>


Components now always replace the element they're bound to. To simulate the behavior of `replace: false`, you can wrap your root component with an element similar to the one you're replacing. For example:
组件现在总是取代它们绑定到元素，模拟的行为`replace: false`，你可以把你根组件与一个相似元素更换。举个例子：

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

Or with a render function:
或者附带一个渲染方法：
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
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>replace: false</code>.</p>
</div>
{% endraw %}

## 全局配置（Global Config）

### `Vue.config.debug` <sup>deprecated</sup>
### `Vue.config.debug` <sup>弃用</sup>

No longer necessary, since warnings come with stack traces by default now.
不再需要,因为现在警告有默认堆栈跟踪。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.debug</code>.</p>
</div>
{% endraw %}

### `Vue.config.async` <sup>deprecated</sup>
### `Vue.config.async` <sup>弃用</sup>

Async is now required for rendering performance.
异步现在需要渲染性能。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.async</code>.</p>
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>deprecated</sup>
### `Vue.config.delimiters` <sup>弃用</sup>

This has been reworked as a [component-level option](/api/#delimiters). This allows you to use alternative delimiters within your app without breaking 3rd-party components.
这已经被修改了为 [component-level option](/api/#delimiters)。在你的应用程序没有违反第三方组件的情况下，这允许您使用替代分隔符。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.delimiters</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.delimiters</code>.</p>
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>deprecated</sup>
### `Vue.config.unsafeDelimiters` <sup>弃用</sup>


HTML interpolation has been [deprecated in favor of `v-html`](#HTML-Interpolation-deprecated).
<<<<<<< HEAD
HTML嵌入已经被[弃用 `v-html`](#HTML-Interpolation-deprecated).
=======
HTML 拦截器已经被[弃用 `v-html`](#HTML-Interpolation-deprecated).
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.config.unsafeDelimiters</code>. After this, the helper will also find instances of HTML interpolation so that you can replace them with `v-html`.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行<a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>查找到例子如：<code>Vue.config.unsafeDelimiters</code>. 然后, 帮助者会找到嵌入的HTML实例因此， 你可以使用`v-html`取代他们。</p>
</div>
{% endraw %}

## 全局API(Global API)

### `Vue.extend` with `el` <sup>deprecated</sup>
### `Vue.extend` 中的`el` <sup>弃用</sup>

The el option can no longer be used in `Vue.extend`. It's only valid as an instance creation option.
el 元素操作不在被使用在`Vue.extend`中。他仅仅在创建实例时可使用。 

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about the <code>el</code> option with <code>Vue.extend</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
<<<<<<< HEAD
  <p>在升级后从头到尾运行你的测试用例或应用程序，查看 <strong>控制台警告</strong> 关于<code>el</code> 在<code>Vue.extend</code>下的操作警告。</p>
=======
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about the <code>el</code> option with <code>Vue.extend</code>.</p>
>>>>>>> 56526b49071e9d37ed02848ec0e33bb4a0797331
</div>
{% endraw %}

### `Vue.elementDirective` <sup>deprecated</sup>
### `Vue.elementDirective` <sup>弃用</sup>

Use components instead.
使用组件代替。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.elementDirective</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行 <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>并找到例如 <code>Vue.elementDirective</code>的代码.</p>
</div>
{% endraw %}

### `Vue.partial` <sup>deprecated</sup>
### `Vue.partial` <sup>弃用</sup>

Use [functional components](render-function.html#Functional-Components) instead.
使用[功能组件](render-function.html#Functional-Components)代替。

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>Vue.partial</code>.</p>
</div>
<div class="upgrade-path">
  <h4>升级方法</h4>
  <p>在代码库中运行 <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a>并找到例如<code>Vue.partial</code>的代码</p>
</div>
{% endraw %}
