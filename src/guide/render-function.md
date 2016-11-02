---
title: 渲染函数 (Render Functions)
type: guide
order: 15
---

## 基础 (Basics)

Vue recommends using templates to build your HTML in the vast majority of cases. There are situations however, where you really need the full programmatic power of JavaScript. That's where you can use the **render  function**, a closer-to-the-compiler alternative to templates.
在绝大多数情况下，Vue 建议使用模板（template）来构建你的 HTML。然而在一些情况下，你真的需要 JavaScript 的编程能力。此时你就可以使用 **渲染函数**——一个更接近于编译器的选择(closer-to-the-compiler alternative)。

Let's dive into a simple example where a `render` function would be practical. Say you want to generate anchored headings:
我们来通过一个简单的例子感性了解一下 `render`。假设你想要生成一个带有链接的标题（anchored headings）：

``` html
<h1>
  <a name="hello-world" href="#hello-world">
    Hello world!
  </a>
</h1>
```

For the HTML above, you decide you want this component interface:
为了生成上面的 HTML，你决定这样定义组件接口：

``` html
<anchored-heading :level="1">Hello world!</anchored-heading>
```

When you get started with a component that just generates a heading based on the `level` prop, you quickly arrive at this:
当你开始写这样一个组件，它会通过 `level` 属性来生成标题，你可能很快就会想到这样实现：

``` html
<script type="text/x-template" id="anchored-heading-template">
  <div>
    <h1 v-if="level === 1">
      <slot></slot>
    </h1>
    <h2 v-if="level === 2">
      <slot></slot>
    </h2>
    <h3 v-if="level === 3">
      <slot></slot>
    </h3>
    <h4 v-if="level === 4">
      <slot></slot>
    </h4>
    <h5 v-if="level === 5">
      <slot></slot>
    </h5>
    <h6 v-if="level === 6">
      <slot></slot>
    </h6>
  </div>
</script>
```

``` js
Vue.component('anchored-heading', {
  template: '#anchored-heading-template',
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

That template doesn't feel great. It's not only verbose, but we're duplicating `<slot></slot>` for every heading level and will have to do the same when we add the anchor element. The whole thing is also wrapped in a useless `div` because components must contain exactly one root node.
这样的模板看起来就不那么舒服。不仅仅是冗余的问题，更重要的是，我们要为每个标题添加 `<slot></slot>` ，之后添加链接时也是如此。同时，所有的东西还需要被包裹在一个无用的 div 中，仅仅是因为组件必须要有一个根节点。

While templates work great for most components, it's clear that this isn't one of them. So let's try rewriting it with a `render` function:
虽然模板对于大多数组件来说非常适用，但在这个场景下就显得不够简洁了。那么，让我们来尝试一下用 `render` 函数来重新实现上面的例子：

``` js
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // tag 名称 // tag name
      this.$slots.default // children 数组 // array of children
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

Much simpler! Sort of. The code is shorter, but also requires greater familiarity with Vue instance properties. In this case, you have to know that when you pass children without a `slot` attribute into a component, like the `Hello world!` inside of `anchored-heading`, those children are stored on the component instance at `$slots.default`. If you haven't already, **it's recommended to read through the [instance properties API](/api/#vm-slots) before diving into render functions.**
简单多了吧！代码精简了许多，但是需要对 Vue 的实例属性有更为熟悉。在这个例子中，你需要知道当不使用 `slot` 属性向组件传递子元素时，比如 `anchored-heading` 中的 `Hello world!`，这些子元素会被存储在组件实例的 `$slots.default` 中。如果你对此不太了解， **在深入渲染函数之前建议阅读[实例属性 API](/api/#vm-slots)。**

## `createElement` 参数 (`createElement` Arguments)

The second thing you'll have to become familiar with is how to use template features in the `createElement` function. Here are the arguments that `createElement` accepts:
你需要熟悉的第二件事是如何在 `createElement` 函数中使用模板特性。以下是 `createElement` 接受的参数：

``` js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // An HTML tag name, component options, or function
  // HTML 标签名称, 组件选项, 或者是返回他们其中之一的函数。
  // returning one of these. Required.
  // 必要参数。
  'div',

  // {Object}
  // A data object corresponding to the attributes
  // you would use in a template. Optional.
  // 你希望在模板中使用的属性（attributes）
  // 对应的数据对象。可选参数。
  {
    // (see details in the next section below)
    // （在下个章节中查看详情）
  },

  // {String | Array}
  // Children VNodes. Optional.
  // 子节点（VNodes）。可选参数。
  [
    createElement('h1', 'hello world'),
    createElement(MyComponent, {
      props: {
        someProp: 'foo'
      }
    }),
    'bar'
  ]
)
```

### 深入了解数据对象 (The Data Object In-Depth)

One thing to note: similar to how `v-bind:class` and `v-bind:style` have special treatment in templates, they have their own top-level fields in VNode data objects.
这里有一件事需要注意：正如 `v-bind:class` 和 `v-bind:style` 在模板中会被特殊处理一样，VNode 数据对象中也拥有自己的顶层属性（top-level fields）。

``` js
{
  // Same API as `v-bind:class`
  // 与 `v-bind:class` 相同的 API
  'class': {
    foo: true,
    bar: false
  },
  // Same API as `v-bind:style`
  // 与 `v-bind:style` 相同的 API
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // Normal HTML attributes
  // 普通 HTML 属性（attributes）
  attrs: {
    id: 'foo'
  },
  // Component props
  // 组件 props
  props: {
    myProp: 'bar'
  },
  // DOM properties
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // Event handlers are nested under "on", though
  // modifiers such as in v-on:keyup.enter are not
  // supported. You'll have to manually check the
  // keyCode in the handler instead.
  // 事件监听器基于 "on"，
  // 因此不再支持如 v-on:keyup.enter 修饰器。
  // 你需要在 handler 中手动检查 keyCode 来实现。
  on: {
    click: this.clickHandler
  },
  // For components only. Allows you to listen to
  // native events, rather than events emitted from
  // the component using vm.$emit.
  // 组件特有。支持监听
  // 除了使用 vm.$emit 发起的事件
  // 以外的原生事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // Custom directives. Note that the binding's
  // oldValue cannot be set, as Vue keeps track
  // of it for you.
  directives: [
    {
      name: 'my-custom-directive',
      value: '2'
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // The name of a slot if the child of a component
  slot: 'name-of-slot'
  // Other special top-level properties
  // 其他特殊的顶层属性
  key: 'myKey',
  ref: 'myRef'
}
```

### 完整示例 (Complete Example)

With this knowledge, we can now finish the component we started:
有了以上知识，我们现在可以完成一开始想要组件了：

``` js
var getChildrenTextContent = function (children) {
  return children.map(function (node) {
    return node.children
      ? getChildrenTextContent(node.children)
      : node.text
  }).join('')
}

Vue.component('anchored-heading', {
  render: function (createElement) {
    // create kebabCase id
    // 创建短横分隔命名的 id
    var headingId = getChildrenTextContent(this.$slots.default)
      .toLowerCase()
      .replace(/\W+/g, '-')
      .replace(/(^\-|\-$)/g, '')

    return createElement(
      'h' + this.level,
      [
        createElement('a', {
          attrs: {
            name: headingId,
            href: '#' + headingId
          }
        }, this.$slots.default)
      ]
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

### 限制 (Constraints)

#### VNode 必须唯一 (VNodes Must Be Unique)

All VNodes in the component tree must be unique. That means the following render function is invalid:
组件树中的所有 VNode 必须唯一。这意味着下面的渲染函数是无效的：

``` js
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // Yikes - duplicate VNodes!
    myParagraphVNode, myParagraphVNode
  ])
}
```

If you really want to duplicate the same element/component many times, you can do so with a factory function. For example, the following render function is a perfectly valid way of rendering 20 identical paragraphs:
如果你确实需要多次使用相同元素/组件，你可以构造一个工厂函数。例如，下面的渲染函数就是一个完美的创建 20 个相同段落的方式：

``` js
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```

## 使用原生 JavaScript 替代模板特性 (Replacing Template Features with Plain JavaScript)

Wherever something can be easily accomplished in plain JavaScript, Vue render functions do not provide a proprietary alternative. For example, in a template using `v-if` and `v-for`:
所有原生 JavaScript 可以轻松完成的东西，Vue 渲染函数都不会提供专有的替换方式。例如，在模板中使用 `v-if` 和 `v-for`：

``` html
<ul v-if="items.length">
  <li v-for="item in items">{{ item.name }}</li>
</ul>
<p v-else>No items found.</p>
```

This could be rewritten with JavaScript's `if`/`else` and `map` in a render function:
上面的例子可以在渲染函数中使用 JavaScript 的 `if`/`else` 和 `map` 重写：

``` js
render: function (createElement) {
  if (this.items.length) {
    return createElement('ul', this.items.map(function (item) {
      return createElement('li', item.name)
    }))
  } else {
    return createElement('p', 'No items found.')
  }
}
```

## JSX

If you're writing a lot of `render` functions, it might feel painful to write something like this:
如果你写了很多 `render` 函数，类似这样的代码可能会让你非常蛋疼：


``` js
createElement(
  'anchored-heading', {
    props: {
      level: 1
    }
  }, [
    createElement('span', 'Hello'),
    ' world!'
  ]
)
```

Especially when the template version is so simple in comparison:
特别是，当对应的模版版本是如此简单：

``` html
<anchored-heading :level="1">
  <span>Hello</span> world!
</anchored-heading>
```

That's why there's a [Babel plugin](https://github.com/vuejs/babel-plugin-transform-vue-jsx) to use JSX with Vue, getting us back to a syntax that's closer to templates:
这是为什么会有一个让 Vue 可以使用 JSX 的[Babel 插件](https://github.com/vuejs/babel-plugin-transform-vue-jsx)，让我们的语法更为接近模板：

``` js
import AnchoredHeading from './AnchoredHeading.vue'

new Vue({
  el: '#demo',
  render (h) {
    return (
      <AnchoredHeading level={1}>
        <span>Hello</span> world!
      </AnchoredHeading>
    )
  }
})
```

<p class="tip">Aliasing `createElement` to `h` is a common convention you'll see in the Vue ecosystem and is actually required for JSX. If `h` is not available in the scope, your app will throw an error.
你会发现，在 Vue 生态中，将 `h` 作为 `createElement` 的别名是一种通用惯例，而且在 JSX 中这更被作为一种要求。如果在作用域中 `h` 不可用，你的应用将会抛出异常。</p>

For more on how JSX maps to JavaScript, see the [usage docs](https://github.com/vuejs/babel-plugin-transform-vue-jsx#usage).
关于更多 JSX 映射到 JavaScript 的使用方法，参考[使用文档](https://github.com/vuejs/babel-plugin-transform-vue-jsx#usage).

## 函数化组件 (Functional Components)

The anchored heading component we created earlier is relatively simple. It doesn't manage any state, watch any state passed to it, and it has no lifecycle methods. Really, it's just a function with some props.
之前创建的带链接的标题组件是个相对简单的例子。它既不管理任何状态，不监听任何传递给它的状态，也没有生命周期方法。事实上，它就是一个接受参数的函数。

In cases like this, we can mark components as `functional`, which means that they're stateless (no `data`) and instanceless (no `this` context). A **functional component** looks like this:
对于这样的例子，我们可以将组件标记为 `函数化（functional）`，这意味着它们是无状态（没有 `data`），无实例（没有 `this` 上下文）。一个 **函数化组件** 看起来应该像这样：

``` js
Vue.component('my-component', {
  functional: true,
  // To compensate for the lack of an instance,
  // we are now provided a 2nd context argument.
  // 为了弥补缺少的实例，
  // 我们提供了 context 参数
  render: function (createElement, context) {
    // ...
  },
  // Props are optional
  // Props 是可选的
  props: {
    // ...
  }
})
```

Everything the component needs is passed through `context`, which is an object containing:
组件需要的所有信息都是通过 `context` 来传递，它是一个包含了以下内容的对象：

- `props`: An object of the provided props
`props`: props 对象

- `children`: An array of the VNode children
`children`: VNode 子节点数组

- `slots`: A function returning a slots object
`slots`: 返回 slots 对象的函数

- `data`: The entire data object passed to the component
`data`: 传递给组件的完整 data 对象

- `parent`: A reference to the parent component
`parent`: 父组件的引用

After adding `functional: true`, updating the render function of our anchored heading component would simply require adding the `context` argument, updating `this.$slots.default` to `context.children`, then updating `this.level` to `context.props.level`.
添加完 `functional: true` 之后，更新我们的带链接的标题组件中的渲染函数就非常简单了：添加 `context` 参数，将 `this.$slots.default` 改为 `context.children`，再将 `this.level` 改为 `context.props.level`。

Since functional components are just functions, they're much cheaper to render. They're also very useful as wrapper components. For example, when you need to:
函数化组件仅仅是函数，它们的渲染成本更低。同时，函数化组件作为包装组件也非常有用。例如以下情形：

- Programmatically choose one of several other components to delegate to
用编程的形式选择使用的组件

- Manipulate children, props, or data before passing them on to a child component
在将 children、props 或者 data 传递给子组件之前操作它们

Here's an example of a `smart-list` component that delegates to more specific components, depending on the props passed to it:
这是一个 `smart-list` 组件的示例，它依赖接收到的 props 决定使用某个具体的组件：

``` js
var EmptyList = { /* ... */ }
var TableList = { /* ... */ }
var OrderedList = { /* ... */ }
var UnorderedList = { /* ... */ }

Vue.component('smart-list', {
  functional: true,
  render: function (createElement, context) {
    function appropriateListComponent () {
      var items = context.props.items

      if (items.length === 0)           return EmptyList
      if (typeof items[0] === 'object') return TableList
      if (context.props.isOrdered)      return OrderedList

      return UnorderedList
    }

    return createElement(
      appropriateListComponent(),
      context.data,
      context.children
    )
  },
  props: {
    items: {
      type: Array,
      required: true
    },
    isOrdered: Boolean
  }
})
```

### `slots()` vs `children`

You may wonder why we need both `slots()` and `children`. Wouldn't `slots().default` be the same as `children`? In some cases, yes - but what if you have a functional component with the following children?
你可能想知道为什么我们同时需要 `slots()` 和 `children`。难道 `slots().default` 和 `children` 不能是一样的吗？在一些例子中，情况的确是这样，但是如果你有这样一个函数化组件呢？

``` html
<my-functional-component>
  <p slot="foo">
    first
  </p>
  <p>second</p>
</my-functional-component>
```

For this component, `children` will give you both paragraphs, `slots().default` will give you only the second, and `slots().foo` will give you only the first. Having both `children` and `slots()` therefore allows you to choose whether this component knows about a slot system or perhaps delegates that responsibility to another component by simply passing along `children`.
对于这样一个组件，`children` 会给你两个段落标签， `slots().default` 只会给你第二个，而 `slots().foo` 则会给你第一个。同时拥有 `children` 和 `slots()`可以让你选择让组件完全了解 slot 系统，或者简单的通过 `children` 传递，让其他组件去处理。

## 模板编译 (Template Compilation)

You may be interested to know that Vue's templates actually compile to render functions. This is an implementation detail you usually don't need to know about, but if you'd like to see how specific template features are compiled, you may find it interesting. Below is a little demo using `Vue.compile` to live-compile a template string:
你可能有兴趣知道 Vue 模板实际上就是被编译成了渲染函数。这其实是个通常情况下无需了解的实现细节，但是如果你想看看模板是如何被编译的，可能会觉得非常有趣。下面是一个使用 `Vue.compile`  来实时编译模板字符串的小 demo：

{% raw %}
<div id="vue-compile-demo" class="demo">
  <textarea v-model="templateText" rows="10"></textarea>
  <div v-if="typeof result === 'object'">
    <label>render:</label>
    <pre><code>{{ result.render }}</code></pre>
    <label>staticRenderFns:</label>
    <pre v-for="(fn, index) in result.staticRenderFns"><code>_m({{ index }}): {{ fn }}</code></pre>
  </div>
  <div v-else>
    <label>Compilation Error:</label>
    <pre><code>{{ result }}</code></pre>
  </div>
</div>
<script>
new Vue({
  el: '#vue-compile-demo',
  data: {
    templateText: '\
<div>\n\
  <h1>I\'m a template!</h1>\n\
  <p v-if="message">\n\
    {{ message }}\n\
  </p>\n\
  <p v-else>\n\
    No message.\n\
  </p>\n\
</div>\
    ',
  },
  computed: {
    result: function () {
      if (!this.templateText) {
        return 'Enter a valid template above'
      }
      try {
        var result = Vue.compile(this.templateText.replace(/\s{2,}/g, ''))
        return {
          render: this.formatFunction(result.render),
          staticRenderFns: result.staticRenderFns.map(this.formatFunction)
        }
      } catch (error) {
        return error.message
      }
    }
  },
  methods: {
    formatFunction: function (fn) {
      return fn.toString().replace(/(\{\n)(\S)/, '$1  $2')
    }
  }
})
console.error = function (error) {
  throw new Error(error)
}
</script>
<style>
#vue-compile-demo pre {
  padding: 10px;
  overflow-x: auto;
}
#vue-compile-demo code {
  white-space: pre;
  padding: 0
}
#vue-compile-demo textarea {
  width: 100%;

}
</style>
{% endraw %}
