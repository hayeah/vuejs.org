---
title: Template Syntax
type: guide
order: 4
---

Vue.js uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying Vue instance's data. All Vue.js templates are valid HTML that can be parsed by spec-compliant browsers and HTML parsers.
Vue.js 使用基于 HTML 的模板语法，它可以声明式地绑定渲染的 DOM 到底层的 Vue 实例数据上。所有的 Vue.js 模板都是可以被符合规范的浏览器和 HTML 解析器解析的有效的 HTML。

Under the hood, Vue compiles the templates into Virtual DOM render functions. Combined with the reactivity system, Vue is able to intelligently figure out the minimal amount of components to re-render and apply the minimal amount of DOM manipulations when the app state changes.
引擎之下，Vue 把模板编译成虚拟的 DOM render 方法。结合响应式系统，当应用状态改变时，Vue 可以智能地计算出需要重新渲染的最少组件，并进行最少量的 DOM 修改。

If you are familiar with Virtual DOM concepts and prefer the raw power of JavaScript, you can also [directly write render functions](/guide/render-function.html) instead of templates, with optional JSX support.
如果你熟悉虚拟 DOM 概念，而且更喜欢原生 JavsScript，你也可以使用可选的 JSX 替代模板来[直接写 render 方法](/guide/render-function.html)。

## Interpolations
## 插值

### Text
### 文本

The most basic form of data binding is text interpolation using the "Mustache" syntax (double curly braces):
数据绑定最基础的形式是文本插值，使用「Mustache」语法（双大括号）：

``` html
<span>Message: {{ msg }}</span>
```

The mustache tag will be replaced with the value of the `msg` property on the corresponding data object. It will also be updated whenever the data object's `msg` property changes.
mustache 标签会被相应数据对象的 `msg` 属性的值替换。每当数据对象的 `msg` 属性变化时它也会更新。

You can also perform one-time interpolations that do not update on data change by using the [v-once directive](/api/#v-once), but keep in mind this will also affect any binding on the same node:
你也可以用 [v-once 指令](/api/#v-once) 实现单次插值，之后的数据变化不会再更新插值，但注意这会影响这个节点上的所有绑定。

``` html
<span v-once>This will never change: {{ msg }}</span>
```

### Raw HTML
### 原始的 HTML

The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use the `v-html` directive:
双 mustache 标签将数据解析为纯文本而不是 HTML。为了输出真的 HTML 字符串，需要用 `v-html` 指令：

``` html
<div v-html="rawHtml"></div>
```

The contents are inserted as plain HTML - data bindings are ignored. Note that you cannot use `v-html` to compose template partials, because Vue is not a string-based templating engine. Instead, components are preferred as the fundamental unit for UI reuse and composition.
内容以 HTML 字符串插入——数据绑定将被忽略。注意你不能使用 `v-html` 生成模板片断，因为 Vue 不是基于字符串的模板引擎。相反，组件才是 UI 复用和构成的最基本单元。

<p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use HTML interpolation on trusted content and **never** on user-provided content.</p>
<p class="tip">在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。记住，只对可信内容使用 HTML 插值，**永不**用于用户提交的内容。</p>


### Attributes
### 特性

Mustaches cannot be used inside HTML attributes, instead use a [v-bind directive](/api/#v-bind):
Mustaches 标签不能用在 HTML 特性内，使用 [v-bind 指令](/api/#v-bind) 替代：

``` html
<div v-bind:id="dynamicId"></div>
```

It also works for boolean attributes - the attribute will be removed if the condition evaluates to a falsy value:
同样可以用于布尔特性——如果条件为假，这个特性将会被移除。

``` html
<button v-bind:disabled="someDynamicCondition">Button</button>
```

### Using JavaScript Expressions
### 使用 JavaScript 表达式

So far we've only been binding to simple property keys in our templates. But Vue.js actually supports the full power of JavaScript expressions inside all data bindings:
到目前为止，我们的模板只绑定到简单的属性键。不过实际上 Vue.js 在所有的数据绑定内支持全功能的 JavaScript 表达式：

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

These expressions will be evaluated as JavaScript in the data scope of the owner Vue instance. One restriction is that each binding can only contain **one single expression**, so the following will **NOT** work:
这些表达式将作为 JavaScript 代码在所属的 Vue 实例的作用域内计算。一个限制是每个绑定只能包含**单个表达式**，因此下面的语句是**无效**的：

``` html
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```
``` html
<!-- 这是一个语句，不是表达式： -->
{{ var a = 1 }}

<!-- 流程控制也不可以，可改用三元表达式 -->
{{ if (ok) { return message } }}
```

<p class="tip">Template expressions are sandboxed and only have access to a whitelist of globals such as `Math` and `Date`. You should not attempt to access user defined globals in template expressions.</p>
<p class="tip">模板表达式运行在沙盒中，只能访问白名单中的全局变量，如 `Math` 和 `Date`，不要试图访问用户在模板表达式中定义的全局变量。</p>

### Filters
### 过滤器

Vue.js allows you to define filters that can be used to apply common text formatting. Filters should be appended to the end of a **mustache interpolation**, denoted by the "pipe" symbol:
Vue.js 允许定义过滤器用于格式化文本，添加在 **mustache 插值**的尾部，以「管道符」指示：

``` html
{{ message | capitalize }}
```

<p class="tip">Vue 2.x filters can only be used inside mustache bindings. To achieve the same behavior inside directive bindings, you should use [Computed properties](/guide/computed.html) instead.</p>
<p class="tip">Vue 2.x 过滤器只能用在 mustache 绑定中，如果想在指令绑定中实现相同的行为，你应该用[计算属性](/guide/computed.html)替代。</p>

The filter function always receives the expression's value as the first argument.
过滤方法始终接收表达式的值作为第一个参数。

``` js
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

Filters can be chained:
过滤器可以链式使用：

``` html
{{ message | filterA | filterB }}
```

Filters are JavaScript functions, therefore they can take arguments:
过滤器是 JavaScript 方法，所以可以接收参数：

``` html
{{ message | filterA('arg1', arg2) }}
```

Here, the plain string `'arg1'` will be passed into the filter as the second argument, and the value of expression `arg2` will be evaluated and passed in as the third argument.
这里，字符串 `arg1` 将作为第二个参数传递给过滤器，表达式 `arg2` 计算得到的值将作为第三个参数。

## Directives
## 指令

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **a single JavaScript expression** (with the exception for `v-for`, which will be discussed later). A directive's job is to reactively apply side effects to the DOM when the value of its expression changes. Let's review the example we saw in the introduction:
指令是带有 `v-` 前缀的特殊属性。指令值被限定为**一个单一 JavaScript 表达式**(`v-for` 是特例，之后会讨论)。指令的职责就是当其表达式的值改变时响应式地把副作用应用到 DOM 上。我们来回头看下「概述」里的例子：

``` html
<p v-if="seen">Now you see me</p>
```

Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `seen`.
这里 `v-if` 指令将根据表达式 `seen` 值的真假删除/插入 `<p>` 元素。

### Arguments
### 参数

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:
有些指令可以在其名称后面带一个「参数」，中间放一个冒号隔开。例如，`v-bind` 指令用于响应地更新 HTML 特性：

``` html
<a v-bind:href="url"></a>
```

Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`.
这里 `href` 是参数，它告诉 `v-bind` 指令将元素的 `href` 特性跟表达式 `url` 的值绑定。

Another example is the `v-on` directive, which listens to DOM events:
另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

``` html
<a v-on:click="doSomething">
```

Here the argument is the event name to listen to. We will talk about event handling in more detail too.
这里参数是被监听的事件的名字。我们也会详细说明事件绑定。

### Modifiers
### 修饰符

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:
修饰符是以半角句号 `.` 开始的特殊后缀，用于表示指令应当以特殊方式绑定。例如 `.prevent` 修饰符告诉 `v-on` 指令事件触发时调用 `event.preventDefault()`：

``` html
<form v-on:submit.prevent="onSubmit"></form>
```

We will see more use of modifiers later when we take a more thorough look at `v-on` and `v-model`.
之后我们深入学习 `v-on` 和 `v-model` 时，我们将看到修饰符更多的用法。

## Shorthands
## 缩写

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building an [SPA](https://en.wikipedia.org/wiki/Single-page_application) where Vue.js manages every template. Therefore, Vue.js provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:
`v-` 前缀是一种标识模板中特定的 Vue 特性的视觉暗示。当你需要在一些现有的 HTML 代码中添加动态行为时，这些前缀可以起到很好的区分效果。但你在使用一些常用指令的时候，你会感觉一直这么写实在是啰嗦。而且在构建[单页应用](https://en.wikipedia.org/wiki/Single-page_application)时，Vue.js 会管理所有的模板，此时 `v-` 前缀也没那么重要了。因此 Vue.js 为两个最常用的指令 `v-bind` 和 `v-on` 提供特别的缩写：

### `v-bind` Shorthand
### `v-bind` 缩写

``` html
<!-- full syntax -->
<a v-bind:href="url"></a>

<!-- shorthand -->
<a :href="url"></a>
```
``` html
<!-- 完整语法 -->
<a v-bind:href="url"></a>

<!-- 缩写 -->
<a :href="url"></a>
```

### `v-on` Shorthand
### `v-on` 缩写

``` html
<!-- full syntax -->
<a v-on:click="doSomething"></a>

<!-- shorthand -->
<a @click="doSomething"></a>
```
``` html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>

<!-- 缩写 -->
<a @click="doSomething"></a>
```

They may look a bit different from normal HTML, but `:` and `@` are valid chars for attribute names and all Vue.js supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
它们看起来跟「合法」的 HTML 有点不同，但是 `:` 和 `@` 是用于特性命名的有效字符，而且在所有 Vue.js 支持的浏览器中都能被正确解析，并且不会出现在最终渲染的标记中。缩写语法完全是可选的，不过随着一步步学习的深入，你会庆幸拥有它们。
