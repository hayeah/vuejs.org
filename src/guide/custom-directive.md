---
title: Custom Directives
type: guide
order: 16
---

## Intro
## <font color="#FF6347">简介</font>

In addition to the default set of directives shipped in core (`v-model` and `v-show`), Vue also allows you to register your own custom directives. Note that in Vue 2.0, the primary form of code reuse and abstraction is components - however there may be cases where you just need some low-level DOM access on plain elements, and this is where custom directives would still be useful. An example would be focusing on an input element, like this one:
<font color="#FF6347">
除了vue默认提供的指令，vue也支持自定义指令。在Vue 2.0中，代码重用和抽象化的主要形式是组件化，但也有一些只需要对网页元素做一些低级的DOM操作的情况，此时自定义指令仍会非常有用。下面是一个置焦点到input上的例子：
</font>


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
<font color="#FF6347">
当页面加载时，元素获得焦点。实际上，如果没有点击过页面上的任何位置，输入框应该已获得焦点。现在，让我们来完成这个指令：
</font>

``` js
// Register a global custom directive called v-focus
// 注册一个全局的自定义指令： v-focus
Vue.directive('focus', {
  // When the bound element is inserted into the DOM...
  // 当绑定的元素被插入到DOM时...
  inserted: function (el) {
    // Focus the element
    // 置元素焦点
    el.focus()
  }
})
```

If you want to register a directive locally instead, components also accept a `directives` option:
<font color="#FF6347">
如果你要注册一个本地指令，组件也接受`directives`选项：
</font>

``` js
directives: {
  focus: {
    // directive definition
    // 指令定义
  }
}
```

Then in a template, you can use the new `v-focus` attribute on any element, like this:
<font color="#FF6347">
然后在模版中，在元素上使用新建的`v-foucs`属性：
</font>

``` html
<input v-focus>
```

## Hook Functions
## <font color="#FF6347">钩子函数</font>

A directive definition object can provide several hook functions (all optional):
<font color="#FF6347">
定义一个指令时，支持多种钩子函数（全部可选项）：
</font>

- `bind`: called only once, when the directive is first bound to the element. This is where you can do one-time setup work.
- <font color="#FF6347">`bind`: 只触发一次，当指令初次绑定到元素时触发。此处可以做一些一次性的配置。</font>

- `inserted`: called when the bound element has been inserted into its parent node (this only guarantees parent node presence, not necessarily in-document).
- <font color="#FF6347">`inserted`: 当元素被插入其父元素时触发（只需保证父元素存在，不要求父元素必须在document中）</font>

- `update`: called whenever the bound element's containing component is updated. The directive's value may or may not have changed. You can skip unnecessary updates by comparing the binding's current and old values (see below on hook arguments).
- <font color="#FF6347">`update`: 每当绑定元素包含的组件更新时触发。该指令的值可能改变也可能不改变。可以通过比较当前值是否变化来略过不必要的更新操作。（详见下文钩子参数）</font>

- `componentUpdated`: called after the containing component has completed an update cycle.
- <font color="#FF6347">`componentUpdated`: 包含的组件完成更新后触发。</font>

- `unbind`: called only once, when the directive is unbound from the element.
- <font color="#FF6347">只触发一次，当指令解除绑定时触发。</font>

We'll explore the arguments passed into these hooks (i.e. `el`, `binding`, `vnode`, and `oldVnode`) in the next section.
<font color="#FF6347">下一节我们将探讨以上这些钩子函数的参数</font>

## Directive Hook Arguments
## <font color="#FF6347">钩子函数的参数</font>

Directive hooks are passed these arguments:
<font color="#FF6347">指令中的参数：</font>

- **el**: The element the directive is bound to. This can be used to directly manipulate the DOM.
- <font color="#FF6347">**el**: 被绑定的元素，可用来直接操作DOM。</font>
- **binding**: An object containing the following properties.
- <font color="#FF6347">**binding**: 一个包含下列属性的对象。</font>
  - **name**: The name of the directive, without the `v-` prefix.
  - **name**: <font color=#FF6347>指令的名称，没有`v-`前缀</font>
  - **value**: The value passed to the directive. For example in `v-my-directive="1 + 1"`, the value would be `2`.
  - **value**L: <font color="#FF6347">传递给指令的值。比如：`v-my-directive="1 + 1"`，指令的value就是2</font>
  - **oldValue**: The previous value, only available in `update` and `componentUpdated`. It is available whether or not the value has changed.
  - **oldValue**: <font color="#FF6347">前一个值，仅于`update`和`componentUpdated`事件中提供。无论值是否改变，oldValue都会传递。</font>
  - **expression**: The expression of the binding as a string. For example in `v-my-directive="1 + 1"`, the expression would be `"1 + 1"`.
  - **expression**: <font color="#FF6347">字符串类型，指令中传入的表达式，比如`v-my-directive="1 + 1"`，expression为 `"1 + 1"`</font>
  - **arg**: The argument passed to the directive, if any. For example in `v-my-directive:foo`, the arg would be `"foo"`.
  - **arg**: <font color="#FF6347">传递给指令的参数，可空。比如：`v-my-directive:foo`，则arg为`"foo"`</font>
  - **modifiers**: An object containing modifiers, if any. For example in `v-my-directive.foo.bar`, the modifiers object would be `{ foo: true, bar: true }`.
  - **modifiers**: <font color="#FF6347"></font>
- **vnode**: The virtual node produced by Vue's compiler.<!--See the [VNode API]([!!TODO: Add link to the VNode API doc when it exists]) for full details.-->
- **vnode**: <font color="#FF6347">Vue编译产出的虚拟节点</font>
- **oldVnode**: The previous virtual node, only available in the `update` and `componentUpdated` hooks.
- **oldVnode**: <font color="#FF6347">上一个虚拟节点， 仅在`update`和`componentUpdated`事件中提供。</font>

<p class="tip">Apart from `el`, you should treat these arguments as read-only and never modify them. If you need to share information across hooks, it is recommended to do so through element's [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset).</p>
<font color="#FF6347">
<p class="tip">除了`el`，其它参数应被视作只读，不应被更改或编辑。若钩子函数之间需要通信，建议使用 [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)。</p>
</font>

An example of a custom directive using some of these properties:
<font color="#FF6347">下面是一个调试指令参数的示例：</font>
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

## Function Shorthand
## <font color="#FF6347">简略的写法</font>

In many cases, you may want the same behavior on `bind` and `update`, but don't care about the other hooks. For example:
<font color="#FF6347">如果指令只需要处理`bind`和`update`事件，可以简写为如下形式：</font>

``` js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

## Object Literals
## <font color="#FF6347">对象文本</font>
If your directive needs multiple values, you can also pass in a JavaScript object literal. Remember, directives can take any valid JavaScript expression.
<font color="#FF6347">如果指令需要多个value，可以传入一个JavaScript对象文本，亦可以是任何有效的JavaScript表达式。</font>

``` html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

``` js
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```
