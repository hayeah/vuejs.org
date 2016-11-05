---
title: 列表渲染 (List Rendering)
type: guide
order: 8
---

## `v-for` 指令 (`v-for`)

We can use the `v-for` directive to render a list of items based on an array. The `v-for` directive requires a special syntax in the form of `item in items`, where `items` is the source data array and `item` is an **alias** for the array element being iterated on:
我们可以使用 `v-for` 指令来将数组渲染成一个列表。`v-for` 指令需要用到一个 `item in items` 的特殊语法，其中 `items` 是源数据的数组，而 `item` 是当前迭代的数组元素的别名：

### 基本用法 (Basic Usage)

``` html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

``` js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: '苹果' },
      { message: '香蕉' }
    ]
  }
})
```

Result:

{% raw %}
<ul id="example-1" class="demo">
  <li v-for="item in items">
    {{item.message}}
  </li>
</ul>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: '苹果' },
      { message: '香蕉' }
    ]
  },
  watch: {
    items: function () {
      smoothScroll.animateScroll(null, '#example-1')
    }
  }
})
</script>
{% endraw %}

Inside `v-for` blocks we have full access to parent scope properties. `v-for` also supports an optional second argument for the index of the current item.
在 `v-for` 块里，我们可以访问父作用域的属性。`v-for` 也支持可选的第二参数，它的值是当前数组项的索引。

``` html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

``` js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: '父作用域',
    items: [
      { message: '苹果' },
      { message: '香蕉' }
    ]
  }
})
```

Result:

{% raw%}
<ul id="example-2" class="demo">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
<script>
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: '父作用域',
    items: [
      { message: '苹果' },
      { message: '香蕉' }
    ]
  },
  watch: {
    items: function () {
      smoothScroll.animateScroll(null, '#example-2')
    }
  }
})
</script>
{% endraw %}

You can also use `of` as the delimiter instead of `in`, so that it is closer to JavaScript's syntax for iterators:
你也可以用 `of` 来代替 `in`，这样会跟 JavaScript 的迭代器语法更加接近：

``` html
<div v-for="item of items"></div>
```

### template 标签中使用 v-for (Template v-for)

Similar to template `v-if`, you can also use a `<template>` tag with `v-for` to render a block of multiple elements. For example:
和 `v-if` 类似，你可以在 `<template>` 标签中用 `v-for` 来渲染多个元素。比如：

``` html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider"></li>
  </template>
</ul>
```

### 用 v-for 迭代对象

You can also use `v-for` to iterate through the properties of an object.
你也可以使用 `v-for` 来迭代对象中的属性：

``` html
<ul id="repeat-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```

``` js
new Vue({
  el: '#repeat-object',
  data: {
    object: {
      FirstName: '狗蛋',
      LastName: '李',
      Age: 30
    }
  }
})
```

结果：

{% raw %}
<ul id="repeat-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
<script>
new Vue({
  el: '#repeat-object',
  data: {
    object: {
      FirstName: '狗蛋',
      LastName: '李',
      Age: 30
    }
  }
})
</script>
{% endraw %}

You can also provide a second argument for the key:
你也可以用第二个参数来得到键：

``` html
<div v-for="(value, key) in object">
  {{ key }} : {{ value }}
</div>
```

And another for the index:
还可以用第三个参数来获得索引：

``` html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }} : {{ value }}
</div>
```

<p class="tip">When iterating over an object, the order is based on the key enumeration order of `Object.keys()`, which is **not** guaranteed to be consistent across JavaScript engine implementations.
当对一个对象进行迭代时，迭代的顺序基于 `Object.keys()` 的枚举顺序，而在不同的 JavaScript 引擎的实现中，这个顺序有可能会不尽相同。</p>

### 用 v-for 迭代值域 (Range v-for)

`v-for` can also take an integer. In this case it will repeat the template that many times.
`v-for` 也可以接受一个整数，它会按照这个次数重复渲染模版：

``` html
<div>
  <span v-for="n in 10">{{ n }}</span>
</div>
```

结果：

{% raw %}
<div id="range" class="demo">
  <span v-for="n in 10">{{ n }} </span>
</div>
<script>
new Vue({ el: '#range' })
</script>
{% endraw %}

### 组件中使用 v-for (Components and v-for)

> This section assumes knowledge of [Components](components.html). Feel free to skip it and come back later.
> 阅读这个段落你需要了解[组件](components.html)的知识。如果你还不了解组件，可以先跳过这个段落，之后再回头看。

You can directly use `v-for` on a custom component, like any normal element:
你可以直接在自定义组件上使用 `v-for`，就像在普通元素上一样：

``` html
<my-component v-for="item in items"></my-component>
```

However, this won't automatically pass any data to the component, because components have isolated scopes of their own. In order to pass the iterated data into the component, we should also use props:
不过，这段代码不会自动将数据绑定到组件，因为组件有自己独立的作用域。要将迭代的数据传递给组件，我们应该使用属性：

``` html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index">
</my-component>
```

The reason for not automatically injecting `item` into the component is because that makes the component tightly coupled to how `v-for` works. Being explicit about where its data comes from makes the component reusable in other situations.
 Vue 没有把 `item` 自动注入组件，是因为这样可以让组件和 `v-for` 更好地解耦。因为我们明确地说明数据的来源，所以我们可以在其它场景中复用组件。

Here's a complete example of a simple todo list:
这是一个简单的 todo 列表的完整示例代码：

``` html
<div id="todo-list-example">
  <input
    v-model="newTodoText"
    v-on:keyup.enter="addNewTodo"
    placeholder="添加一个 todo (Add a todo)"
  >
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:title="todo"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```

``` js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">X</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      '洗碗 (Do the dishes)',
      '倒垃圾 (Take out the trash)',
      '除草 (Mow the lawn)'
    ]
  },
  methods: {
    addNewTodo: function () {
      this.todos.push(this.newTodoText)
      this.newTodoText = ''
    }
  }
})
```

{% raw %}
<div id="todo-list-example" class="demo">
  <input
    v-model="newTodoText" v
    v-on:keyup.enter="addNewTodo"
    placeholder="添加一个 todo (Add a todo)"
  >
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:title="todo"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
<script>
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">X</button>\
    </li>\
  ',
  props: ['title']
})
new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      '洗碗 (Do the dishes)',
      '倒垃圾 (Take out the trash)',
      '除草 (Mow the lawn)'
    ]
  },
  methods: {
    addNewTodo: function () {
      this.todos.push(this.newTodoText)
      this.newTodoText = ''
    }
  }
})
</script>
{% endraw %}

## key 属性 (key)

When Vue.js is updating a list of elements rendered with `v-for`, it by default uses an "in-place patch" strategy. If the order of the data items has changed, instead of moving the DOM elements to match the order of the items, Vue will simply patch each element in-place and make sure it reflects what should be rendered at that particular index. This is similar to the behavior of `track-by="$index"` in Vue 1.x.
当 Vue.js 在更新用 `v-for` 渲染的列表时，它默认使用了“原地更新”策略。如果数据项的顺序改变了，Vue 会对每一个元素原地更新，而不会去修改 DOM 的结构。这跟 Vue 1.x 里 `track-by="$index"` 的行为类似。

This default mode is efficient, but only suitable **when your list render output does not rely on child component state or temporary DOM state (e.g. form input values)**.
这种默认的模式是高效的，不过这只适用于**你的列表渲染结构不依赖于子组件的状态或者临时的 DOM 状态（比如，表单控件的值）**的情况。

To give Vue a hint so that it can track each node's identity, and thus reuse and reorder existing elements, you need to provide a unique `key` attribute for each item. An ideal value for `key` would be the unique id of each item. This special attribute is a rough equivalent to `track-by` in 1.x, but it works like an attribute, so you need to use `v-bind` to bind it to dynamic values (using shorthand here):
为了要复用和重排已有的元素，你需要为每一个列表项都提供唯一的 `key` 属性，以此帮助 Vue 来追踪每一个节点。`key` 的理想值是每一个列表项的唯一 id。`key` 属性的作用跟 Vue 1.x 的 `track-by` 大致类似，不过因为它是特殊属性，所以你需要使用 `v-bind` 来将它绑定到动态值（这里使用了缩写）：

``` html
<div v-for="item in items" :key="item.id">
  <!-- content -->
  <!-- 内容 -->
</div>
```

It is recommended to provide a `key` with `v-for` whenever possible, unless the iterated DOM content is simple, or you are intentionally relying on the default behavior for performance gains.
在使用 `v-for` 时，应该尽可能地提供 `key` 属性，除非你要迭代的 DOM 内容很简单，或者你有意要用默认的行为来获取性能提升。

Since it's a generic mechanism for Vue to identify nodes, the `key` also has other uses that are not specifically tied to `v-for`, as we will see later in the guide.
因为在 Vue 中，识别节点是一个很通用的机制，所以除了 `v-for` 还有别的地方会用到 `key`，我们会在教程后面的章节里看到。

## 数组变化检测 (Array Change Detection)

### 突变方法 （Mutation Methods）

Vue wraps an observed array's mutation methods so they will also trigger view updates. The wrapped methods are:
Vue 会封装被观察数组的突变方法，这样它们就可以触发视图更新。这些被封装的方法是：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

You can open the console and play with the previous examples' `items` array by calling their mutation methods. For example: `example1.items.push({ message: 'Baz' })`.
你可以打开控制台，试一下对之前示例代码中的 `items` 数组调用突变方法。比如 `example1.items.push({ message: '雪梨' })`。

### 数组替换 (Replacing an Array)

Mutation methods, as the name suggests, mutate the original array they are called on. In comparison, there are also non-mutating methods, e.g. `filter()`, `concat()` and `slice()`, which do not mutate the original Array but **always return a new array**. When working with non-mutating methods, you can just replace the old array with the new one:
突变方法，顾名思义，就是会修改原来调用的数组。相对地，也有一些非突变的方法，比如 `filter`，`concat` 和 `slice`，它们不会改动原有的数组而**总是返回一个新的数组**。当使用非突变方法时，你只需要用返回的新数组替换旧数组：

``` js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/苹果/)
})
```

You might think this will cause Vue to throw away the existing DOM and re-render the entire list - luckily, that is not the case. Vue implements some smart heuristics to maximize DOM element reuse, so replacing an array with another array containing overlapping objects is a very efficient operation.
你可能会觉得，这样做会让 Vue 抛弃原来的 DOM，将整个列表重新渲染 —— 幸运的是，Vue 实现了一些巧妙的规则来最大化 DOM 元素的复用，所以用一个包含重叠元素的数组替换掉原来数组，在 Vue 中是一个非常高效的操作。

### 警告 (Caveats)

Due to limitations in JavaScript, Vue **cannot** detect the following changes to an array:
因为 JavaScript 的限制，Vue **无法**探测到以下对于数组的改动：

1. When you directly set an item with the index, e.g. `vm.items[indexOfItem] = newValue`
1. 直接用索引来设置一个数组项的值，比如 `vm.items[indexOfItem] = newValue`
2. When you modify the length of the array, e.g. `vm.items.length = newLength`
2. 修改数组的长度，比如 `vm.items.length = newLength`

To overcome caveat 1, both of the following will accomplish the same as `vm.items[indexOfItem] = newValue`, but will also trigger state updates in the reactivity system:
要避免第一种情况，以下两种做法都可以达到 `vm.items[indexOfItem] = newValue` 的效果，并且触发状态更新：

``` js
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
```
``` js
// Array.prototype.splice`
example1.items.splice(indexOfItem, 1, newValue)
```

To deal with caveat 2, you can also use `splice`:
要避免第二种情况，你还是可以使用 `splice`：

``` js
example1.items.splice(newLength)
```

## 显示过滤/排序后的结果 (Displaying Filtered/Sorted Results)

Sometimes we want to display a filtered or sorted version of an array without actually mutating or resetting the original data. In this case, you can create a computed property that returns the filtered or sorted array.
有时我们会希望显示一个经过过滤或排序的数组，而又不想改动原来的数据。在这种情况下，可以创建一个计算属性来返回过滤或者排序后的数组。

For example:
例如：

``` html
<li v-for="n in evenNumbers">{{ n }}</li>
```

``` js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

Alternatively, you can also just use a method where computed properties are not feasible (e.g. inside nested `v-for` loops):
在计算属性不能用时（比如在嵌套的 `v-for` 循环里），你也可以直接使用方法：

``` html
<li v-for="n in even(numbers)">{{ n }}</li>
```

``` js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
