---
title: class 与 style 绑定 (Class and Style Bindings)
type: guide
order: 6
---

A common need for data binding is manipulating an element's class list and its inline styles. Since they are both attributes, we can use `v-bind` to handle them: we just need to calculate a final string with our expressions. However, meddling with string concatenation is annoying and error-prone. For this reason, Vue provides special enhancements when `v-bind` is used with `class` and `style`. In addition to strings, the expressions can also evaluate to objects or arrays.
数据绑定的一个常见需求是操作元素的 class 列表和其内联样式。由于他们都是属性 ，我们可以使用 `v-bind` 来处理它们：我们只需要计算出表达式最终的字符串结果就要好了。然而，拼接字符串是一件繁琐而又容易出错的事情。因此，Vue 特地强化了 `v-bind` 在 `class` 和 `style` 上的应用。除了字符串之外，表达式的结果类型还可以是对象或者数组。

## 绑定 HTML class (Binding HTML Classes)

### 对象语法 (Object Syntax)

We can pass an object to `v-bind:class` to dynamically toggle classes:
我们可以传递给 `v-bind:class` 一个对象，以动态地切换 class：

``` html
<div v-bind:class="{ active: isActive }"></div>
```

The above syntax means the presence of the `active` class will be determined by the [truthiness](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) of the data property `isActive`.
以上的语法的意味着，class `active` 的存在与否，取决于数据属性 `isActive` 是否[为真](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)。

You can have multiple classes toggled by having more fields in the object. In addition, the `v-bind:class` directive can also co-exist with the plain `class` attribute. So given the following template:
在对象中，你可以有多个属性以切换多个 class 。除此之外，`v-bind:class` 指令也可以和原生的 `class` 属性共存。所以，下面的模版：

``` html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

And the following data:
以及以下数据:

``` js
data: {
  isActive: true,
  hasError: false
}
```

It will render:
会渲染为:

``` html
<div class="static active"></div>
```

When `isActive` or `hasError` changes, the class list will be updated accordingly. For example, if `hasError` becomes `true`, the class list will become `"static active text-danger"`.
每当 `isActive` 或者 `hasError` 的值改变了，class 列表就会跟着更新。举个例子，如果 `hasError` 的值变为 `true` ，class 列表就会变成 `"static active text-danger"`。

The bound object doesn't have to be inline:
绑定的对象不一定要是内联对象，也可以是表达式:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

This will render the same result. We can also bind to a [computed property](computed.html) that returns an object. This is a common and powerful pattern:
这会被渲染为同样的结果。我们也可以绑定返回一个对象的[计算属性](computed.html)。 这是一个常用且强大的模式:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### 数组语法 (Array Syntax)

We can pass an array to `v-bind:class` to apply a list of classes:
我们可以把一个数组传给 `v-bind:class`，以添加一个 class 列表：

``` html
<div v-bind:class="[activeClass, errorClass]">
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Which will render:
会渲染为：

``` html
<div class="active text-danger"></div>
```

If you would like to also toggle a class in the list conditionally, you can do it with a ternary expression:
如果你也想要在这个数组列表中根据条件切换 class，你可以使用三元表达式：

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```

This will always apply `errorClass`, but will only apply `activeClass` when `isActive` is `true`.
这个例子中将总是添加 `errorClass` ，但是只有当 `isAcitve` 为真时才会添加 `activeClass`。

However, this can be a bit verbose if you have multiple conditional classes. That's why it's also possible to use the object syntax inside array syntax:
然而，当你有多个带条件的 class 这个写法可能会稍微有些啰嗦了，这就是为什么你也可以在数组语法中使用对象语法：

``` html
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### 使用组件 (With Components)

> This section assumes knowledge of [Vue Components](components.html). Feel free to skip it and come back later.
> 要理解本小节，你需要先了解 [Vue 组件](components.html)的知识。如果你还不了解组件，可以先跳过这个段落，之后再回头看。

When you use the `class` attribute on a custom component, those classes will be added to the component's root element. Existing classes on this element will not be overwritten.
如果一个自定义组件使用 `class` 属性，那么这些 class 会被添加到组件的根元素。这个元素已有的类不会被覆盖。

For example, if you declare this component:
例如，如果你声明了这样一个组件：

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Then add some classes when using it:
然后，在使用时添加了几个 class：

``` html
<my-component class="baz boo"></my-component>
```

The rendered HTML will be:
渲染出来的 HTML 会是：

``` html
<p class="foo bar baz boo">Hi</p>
```

The same is true for class bindings:
class 绑定也是一样的：

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

When `isActive` is truthy, the rendered HTML will be:
当 `isActive` 为真时，渲染出来的 HTML 会是：

``` html
<p class="foo bar active"></p>
```

## 绑定行内样式 (Binding Inline Styles)

### 对象语法 (Object Syntax)

The object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case for the CSS property names:
`v-bind:style` 的对象语法非常直观——它看起来就像是 CSS ，除了它是一个 JavaScript 对象以外。你可以以用驼峰命名或短横分隔命名来命名 CSS 属性:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:
直接绑定一个样式对象通常来说是个好主意，因为它能让模板变得清晰：

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Again, the object syntax is often used in conjunction with computed properties that return objects.
同样的，对象语法通常会结合计算属性返回样式对象来使用。

### 数组语法 (Array Syntax)

The array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:
`v-bind:style` 的数组语法允许你添加多个样式对象到同一个元素上：

``` html
<div v-bind:style="[baseStyles, overridingStyles]">
```

### 自动补齐前缀 (Auto-prefixing)

When you use a CSS property that requires vendor prefixes in `v-bind:style`, for example `transform`, Vue will automatically detect and add appropriate prefixes to the applied styles.
当你在 `v-bind:style` 中使用的 CSS 属性需要浏览器前缀时，例如 `transform` 属性，Vue 将会自动的检测并添加合适的前缀到实际的样式上。
