---
title: 自定义指令 (Custom Directives)
type: guide
order: 16
---

## 简介 (Intro)

In addition to the default set of directives shipped in core (`v-model` and `v-show`), Vue also allows you to register your own custom directives. Note that in Vue 2.0, the primary form of code reuse and abstraction is components - however there may be cases where you just need some low-level DOM access on plain elements, and this is where custom directives would still be useful. An example would be focusing on an input element, like this one:
除了 Vue 默认提供的指令（如：`v-model`，`v-show`），Vue 也支持自定义指令。Vue 2.0 主要通过组件来进行代码重用和抽象，但有时需要对网页元素做一些底层的 DOM 操作，此时自定义指令仍会非常有用。比如说，把焦点定位到 input 元素，代码如下：

{% raw %}
<div id="simplest-directive-example" class="demo">
  <input v-focus>
</div>
<script>
Vue.directive('focus', {
  inserted: function (el) {
    el.focus()
  }
})
new Vue({
  el: '#simplest-directive-example'
})
</script>
{% endraw %}

When the page loads, that element gains focus. In fact, if you haven't clicked on anything else since visiting this page, the input above should be focused now. Now let's build the directive that accomplishes this:
当页面加载时，焦点会定位到该元素。实际上，如果你进入本页面之后没有点击任何东西，焦点应该已经定位到上面的输入控件了。现在，让我们来完成这个指令：


``` js
// Register a global custom directive called v-focus
// 注册一个全局的自定义指令： v-focus
Vue.directive('focus', {
  // When the bound element is inserted into the DOM...
  // 当绑定的元素被插入到 DOM 时...
  inserted: function (el) {
    // Focus the element
    // 将焦点定位到元素
    el.focus()
  }
})
```

If you want to register a directive locally instead, components also accept a `directives` option:
如果你要注册一个本地指令，组件也接受 `directives` 选项：

``` js
directives: {
  focus: {
    // directive definition
    // 指令定义
  }
}
```

Then in a template, you can use the new `v-focus` attribute on any element, like this:
然后在模版中，在元素上使用新建的 `v-foucs` 属性：

``` html
<input v-focus>
```

## 钩子函数 (Hook Functions)

A directive definition object can provide several hook functions (all optional):
定义一个指令时，支持多种钩子函数（全部可选项）：


- `bind`: called only once, when the directive is first bound to the element. This is where you can do one-time setup work.
`bind`：只触发一次，当指令初次绑定到元素时触发。此处可以做些一次性的配置。

- `inserted`: called when the bound element has been inserted into its parent node (this only guarantees parent node presence, not necessarily in-document).
`inserted`: 当元素被插入其父元素时触发（只需保证父元素存在，不要求父元素必须在网页文档中）

- `update`: called after the containing component has updated, __but possibly before its children have updated__. The directive's value may or may not have changed, but you can skip unnecessary updates by comparing the binding's current and old values (see below on hook arguments).
`update`：当所包含的组件更新时触发，__但有可能是在它的子组件更新之前__。该指令的值可能改变也可能不改变。可以通过比较当前值是否变化来略过不必要的更新操作。（详见下文钩子参数）

- `componentUpdated`: called after the containing component __and its children__ have updated.
`componentUpdated`：包含的组件__以及它的子组件__完成更新后触发。

- `unbind`: called only once, when the directive is unbound from the element.
`unbind`：只触发一次，当指令解除绑定时触发。

We'll explore the arguments passed into these hooks (i.e. `el`, `binding`, `vnode`, and `oldVnode`) in the next section.
下一节我们将探讨以上这些钩子函数（即 `el`、`binding`、`vnode` 和 `oldVnode`）的参数。

## 钩子函数的参数 (Directive Hook Arguments)

Directive hooks are passed these arguments:
指令中的参数：

- **el**: The element the directive is bound to. This can be used to directly manipulate the DOM.
**el**: 被绑定的元素，可用来直接操作 DOM。

- **binding**: An object containing the following properties.
**binding**: 一个包含下列属性的对象。

  - **name**: The name of the directive, without the `v-` prefix.
  **name**: 指令的名称，没有 `v-` 前缀。

  - **value**: The value passed to the directive. For example in `v-my-directive="1 + 1"`, the value would be `2`.
  **value**L: 传递给指令的值。比如：`v-my-directive="1 + 1"`，指令的值就是 2。

  - **oldValue**: The previous value, only available in `update` and `componentUpdated`. It is available whether or not the value has changed.
  **oldValue**: 前一个值，仅于 `update` 和 `componentUpdated` 事件中提供。无论值是否改变，oldValue 都会传递。

  - **expression**: The expression of the binding as a string. For example in `v-my-directive="1 + 1"`, the expression would be `"1 + 1"`.
  **expression**: 字符串类型，指令中传入的表达式，比如 `v-my-directive="1 + 1"`，表达式是 `"1 + 1"`

  - **arg**: The argument passed to the directive, if any. For example in `v-my-directive:foo`, the arg would be `"foo"`.
  **arg**: 传递给指令的参数，可空。比如：`v-my-directive:foo`，则 arg 为 `"foo"`。

  - **modifiers**: An object containing modifiers, if any. For example in `v-my-directive.foo.bar`, the modifiers object would be `{ foo: true, bar: true }`.
  **modifiers**: 一个包含修饰符（如果有）的对象。比如在 `v-my-directive.foo.bar` 中，修饰符对象就是 `{ foo: true, bar: true }`。

- **vnode**: The virtual node produced by Vue's compiler. See the [VNode API](../api/#VNode-Interface) for full details.
**vnode**: Vue 的编译器生成的虚拟节点。详情参考 [VNode API](../api/#VNode-Interface)。
- **oldVnode**: The previous virtual node, only available in the `update` and `componentUpdated` hooks.
**oldVnode**: 上一个虚拟节点， 仅在 `update` 和 `componentUpdated` 事件中提供。

<p class="tip">Apart from `el`, you should treat these arguments as read-only and never modify them. If you need to share information across hooks, it is recommended to do so through element's [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset).
除了`el`，其它参数应被视作只读，不应被更改或编辑。若钩子函数之间需要通信，建议使用 [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)。</p>

An example of a custom directive using some of these properties:
下面是一个调试指令参数的示例：

``` html
<div id="hook-arguments-example" v-demo:hello.a.b="message"></div>
```

``` js
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
```

{% raw %}
<div id="hook-arguments-example" v-demo:hello.a.b="message" class="demo"></div>
<script>
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})
new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
</script>
{% endraw %}

## 简略的写法 (Function Shorthand)

In many cases, you may want the same behavior on `bind` and `update`, but don't care about the other hooks. For example:
如果指令只需要处理`bind`和`update`事件，可以简写为如下形式：

``` js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

## 对象字面量 (Object Literals)

If your directive needs multiple values, you can also pass in a JavaScript object literal. Remember, directives can take any valid JavaScript expression.
如果指令需要多个值，可以传入一个 JavaScript 对象字面量，亦可以是任何有效的 JavaScript 表达式。

``` html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

``` js
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```
