---
title: Vue 实例 (The Vue Instance)
type: guide
order: 3
---

## 构造器 (Constructor)

Every Vue vm is bootstrapped by creating a **root Vue instance** with the `Vue` constructor function:
我们总是通过 `Vue` 构造函数来创建一个 **Vue 的根实例**：

``` js
var vm = new Vue({
  // options
  // 选项
})
```

Although not strictly associated with the [MVVM pattern](https://en.wikipedia.org/wiki/Model_View_ViewModel), Vue's design was no doubt inspired by it. As a convention, we often use the variable `vm` (short for ViewModel) to refer to our Vue instances.
Vue 虽然和 [MVVM 模式](https://en.wikipedia.org/wiki/Model_View_ViewModel)没有什么紧密的联系，但的确受到了它的启发。作为约定，我们通常用 `vm` (ViewModel 的缩写) 来表示 Vue 实例。

When you instantiate a Vue instance, you need to pass in an **options object** which can contain options for data, template, element to mount on, methods, lifecycle callbacks and more. The full list of options can be found in the [API reference](../api).
在实例化 Vue 时，需要传入一个**选项对象**，它可以包含数据、模板、挂载元素、方法、生命周期钩子等选项。全部的选项可以在 [API 文档](../api)中查看。

The `Vue` constructor can be extended to create reusable **component constructors** with pre-defined options:
通过扩展 `Vue` 构造函数，我们可以创建自定义的 **组件构造函数**：

``` js
var MyComponent = Vue.extend({
  // extension options
  // 扩展选项
})

// all instances of `MyComponent` are created with
// the pre-defined extension options
// 所有的 `MyComponent` 实例都将用预定义的扩展选项被创建
var myComponentInstance = new MyComponent()
```

Although it is possible to create extended instances imperatively, most of the time it is recommended to compose them declaratively in templates as custom elements. We will talk about [the component system](components.html) in detail later. For now, you just need to know that all Vue components are essentially extended Vue instances.
在创建扩展实例时，大部分情况下你应该使用声明式而不是命令式的方法，也就是在模版中将它们声明为自定义元素。我们将在后面详细说明[组件系统](components.html)。现在你只需知道所有的 Vue 组件其实都是被扩展的 Vue 实例。

## 属性与方法 (Properties and Methods)

Each Vue instance **proxies** all the properties found in its `data` object:
每个 Vue 实例都会**代理**其 `data` 对象中的所有属性：

``` js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> true

// setting the property also affects original data
// 设置属性也会影响原始数据
vm.a = 2
data.a // -> 2

// ... and vice-versa
// ... 反之亦然
data.a = 3
vm.a // -> 3
```

It should be noted that only these proxied properties are **reactive**. If you attach a new property to the instance after it has been created, it will not trigger any view updates. We will discuss the reactivity system in detail later.
注意只有这些被代理的属性是**响应式的**。如果在实例创建之后再给它添加新的属性，那么这个属性不会触发视图更新。我们将在后面详细讨论响应系统。

In addition to data properties, Vue instances expose a number of useful instance properties and methods. These properties and methods are prefixed with `$` to differentiate them from proxied data properties. For example:
除了这些数据属性，Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 `$`，以便与代理的数据属性区分。例如：

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true

// $watch is an instance method
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
  // this callback will be called when `vm.a` changes
  // 这个回调将在 `vm.a` 改变后调用
})
```

<p class="tip">Don't use [arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) on an instance property or callback (e.g. `vm.$watch('a', newVal => this.myMethod())`). As arrow functions are bound to the parent context, `this` will not be the Vue instance as you'd expect and `this.myMethod` will be undefined.</p>

Consult the [API reference](../api) for the full list of instance properties and methods.
参考 [API 文档](../api)查看全部的实例属性与方法。

## 实例生命周期 (Instance Lifecycle)

Each Vue instance goes through a series of initialization steps when it is created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it will also invoke some **lifecycle hooks**, which give us the opportunity to execute custom logic. For example, the `created` hook is called after the instance is created:
Vue 实例在创建时有一系列初始化步骤——例如，建立数据观察，编译模板，挂载 DOM 实例，数据变化时更新 DOM。在此过程中，它也将调用一些**生命周期钩子**，这样我们就可以执行一些自定义逻辑。例如 `created` 钩子会在实例创建后被调用：

``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```

There are also other hooks which will be called at different stages of the instance's lifecycle, for example `mounted`, `updated`, and `destroyed`. All lifecycle hooks are called with their `this` context pointing to the Vue instance invoking it. You may have been wondering where the concept of "controllers" lives in the Vue world and the answer is: there are no controllers. Your custom logic for a component would be split among these lifecycle hooks.
在实例生命周期的不同阶段，还会调用其它的钩子，如 `mounted`、 `updated`、`destroyed`。钩子的 `this` 指向调用它的 Vue 实例。你可能会问，Vue 里面没有“控制器”吗？答案是，没有。组件的自定义逻辑会分布在这些生命周期的钩子中。

## 生命周期图示 (Lifecycle Diagram)

Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but this diagram will be helpful in the future.
下图解释了 Vue 实例的生命周期。如果你现在还不能完全理解它，没有关系，总有一天它会派上用场。

{% img /images/lifecycle.png "Lifecycle" %}
