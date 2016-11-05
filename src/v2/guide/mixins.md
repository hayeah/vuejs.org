---
title: 混合 (Mixins)
type: guide
order: 17
---

## 基础 (Basics)

Mixins are a flexible way to distribute reusable functionalities for Vue components. A mixin object can contain any component options. When a component uses a mixin, all options in the mixin will be "mixed" into the component's own options.
混合以一种灵活的方式为 Vue 组件提供可复用功能。混合对象可以包含任意的组件选项。当组件使用了混合对象时，混合对象的所有选项将被“混入”组件自己的选项中。

Example:
示例：

``` js
// define a mixin object
// 定义一个混合对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// define a component that uses this mixin
// 定义一个组件，使用这个混合对象
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // -> "hello from mixin!"
```

## 选项合并 (Option Merging)

When a mixin and the component itself contain overlapping options, they will be "merged" using appropriate strategies. For example, hook functions with the same name are merged into an array so that all of them will be called. In addition, mixin hooks will be called **before** the component's own hooks:
当混合对象与组件包含同名选项时，这些选项将以适当的策略合并。例如，同名钩子函数被并入一个数组，因而都会被调用。另外，混合的钩子将在组件自己的钩子之前调用：

``` js
var mixin = {
  created: function () {
    console.log('mixin hook called')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('component hook called')
  }
})

// -> "mixin hook called"
// -> "component hook called"
```

Options that expect object values, for example `methods`, `components` and `directives`, will be merged into the same object. The component's options will take priority when there are conflicting keys in these objects:
值为对象的选项，如 `methods`, `components` 和 `directives` 将合并到同一个对象内。如果键冲突则组件的选项优先:

``` js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // -> "foo"
vm.bar() // -> "bar"
vm.conflicting() // -> "from self"
```

Note that the same merge strategies are used in `Vue.extend()`.
注意 `Vue.extend()` 使用同样的合并策略。

## 全局混合 (Global Mixin)

You can also apply a mixin globally. Use caution! Once you apply a mixin globally, it will affect **every** Vue instance created afterwards. When used properly, this can be used to inject processing logic for custom options:
也可以全局注册混合。小心使用！一旦全局注册混合，它会影响所有之后创建的 Vue 实例。如果使用恰当，可以为自定义选项注入处理逻辑：

``` js
// inject a handler for `myOption` custom option
// 为 `myOption` 自定义选项注入一个处理器
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// -> "hello!"
```

<p class="tip">Use global mixins sparsely and carefully, because it affects every single Vue instance created, including third party components. In most cases, you should only use it for custom option handling like demonstrated in the example above. It's also a good idea to ship them as [Plugins](plugins.html) to avoid duplicate application.
慎用全局混合，因为它影响到每个创建的 Vue 实例，包括第三方组件。在大多数情况下，它应当只用于自定义选项，就像上面示例一样。把他收入[Plugins](plugins.html) 去避免重复的应用也是不错的选择。</p>

## 自定义选项合并策略 (Custom Option Merge Strategies)

When custom options are merged, they use the default strategy, which simply overwrites the existing value. If you want a custom option to be merged using custom logic, you need to attach a function to `Vue.config.optionMergeStrategies`:
在合并自定义选项时，默认的合并策略是简单的覆盖已有值。如果你想用自定义逻辑合并自定义选项，可以向 `Vue.config.optionMergeStrategies` 添加一个函数：

``` js
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // return mergedVal
  // 返回 mergedVal
}
```

For most object-based options, you can simply use the same strategy used by `methods`:
对于大多数值为对象的选项，你可以简单地使用 `methods` 所用的合并策略：

``` js
var strategies = Vue.config.optionMergeStrategies
strategies.myOption = strategies.methods
```

A more advanced example can be found on [Vuex](https://github.com/vuejs/vuex)'s 1.x merging strategy:
在 [Vuex 1.x](https://github.com/vuejs/vuex) 和合并策略中，可以找到更多的进阶示例：

``` js
const merge = Vue.config.optionMergeStrategies.computed
Vue.config.optionMergeStrategies.vuex = function (toVal, fromVal) {
  if (!toVal) return fromVal
  if (!fromVal) return toVal
  return {
    getters: merge(toVal.getters, fromVal.getters),
    state: merge(toVal.state, fromVal.state),
    actions: merge(toVal.actions, fromVal.actions)
  }
}
```
