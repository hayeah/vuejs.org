---
title: Computed Properties and Watchers
type: guide
order: 5
---

## Computed Properties
## 计算属性

In-template expressions are very convenient, but they are really only meant for simple operations. Putting too much logic into your templates can make them bloated and hard to maintain. For example:
在模板中绑定表达式是非常便利的，但是它们实际上只用于简单的操作。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

``` html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

At this point, the template is no longer simple and declarative. You have to look at it for a second before realizing that it displays `message` in reverse. The problem is made worse when you want to include the reversed message in your template more than once.
在这种情况下，模版不再简单和清晰。在实现反向显示 `message`  之前，你不得不再次确认它。当你不止一次反向显示 message 的时候这种问题将会变得更加糟糕。

That's why for any complex logic, you should use a **computed property**.
这就是为什么 Vue.js 将绑定表达式限制为一个表达式。如果需要多于一个表达式的逻辑，应当使用 **计算属性** 。

### Basic Example
### 基础例子

``` html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
	// 一个计算属性的 getter
    reversedMessage: function () {
      // `this` points to the vm instance
	  // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

Result:

{% raw %}
<div id="example" class="demo">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Here we have declared a computed property `reversedMessage`. The function we provided will be used as the getter function for the property `vm.reversedMessage`:
这里我们声明了一个计算属性 `reversedMessage` 。我们提供的函数将用作属性 `vm.reversedMessage` 的 getter：

``` js
console.log(vm.reversedMessage) // -> 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // -> 'eybdooG'
```

You can open the console and play with the example vm yourself. The value of `vm.reversedMessage` is always dependent on the value of `vm.message`.
你可以打开浏览器控制台，修改 vm。 `vm.reverseMessage` 的值始终取决于 `vm.message` 的值。

You can data-bind to computed properties in templates just like a normal property. Vue is aware that `vm.reversedMessage` depends on `vm.message` , so it will update any bindings that depend on `vm.reversedMessage` when `vm.message` changes. And the best part is that we've created this dependency relationship declaratively: the computed getter function is pure and has no side effects, which makes it easy to test and reason about.
你可以像绑定普通属性一样在模板中绑定计算属性。Vue 知道 `vm.reversedMessage` 依赖于 `vm.message` ，因此当 `vm.message` 发生改变时，依赖于 `vm.reversedMessage` 的绑定也会更新。而且最妙的是我们是声明式地创建这种依赖关系：计算属性的 getter 是干净无副作用的，因此也是易于测试和理解的。

### Computed Caching vs Methods
### 计算缓存 vs Methods

You may have noticed we can achieve the same result by invoking a method in the expression:
你可能已经注意到了在表达式中调用一个方法我们也可以实现相同的结果：


``` html
<p>Reversed message: "{{ reverseMessage() }}"</p>
```

``` js
// in component
// 在组件里
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

Instead of a computed property, we can define the same function as a method instead. For the end result, the two approaches are indeed exactly the same. However, the difference is that **computed properties are cached based on its dependencies.** A computed property will only re-evaluate when some of its dependencies have changed. This means as long as `message` has not changed, multiple access to the `reversedMessage` computed property will immediately return the previously computed result without having to run the function again.
我们可以定义一个相同功能的 method 来代替计算属性。从最终显示的结果来看，这两种方法确实是完全相同的。然而，不同的是 **计算属性基于它的依赖关系被缓存。** 计算属性只有在它的相关依赖发生改变时才会重新评估。也就是说，只要 `message` 不发生改变，多次请求 `reversedMessage` 计算属性将会立即返回之前的结果，而不必运行函数。


This also means the following computed property will never update, because `Date.now()` is not a reactive dependency:
这也同样意味着如下计算属性将不会更新，因为 `Date.now()` 并不会被依赖：


``` js
computed: {
  now: function () {
    return Date.now()
  }
}
```

In comparison, a method invocation will **always** run the function whenever a re-render happens.
相比之下，每当重新渲染的时候，method 调用 **总会** 运行函数。


Why do we need caching? Imagine we have an expensive computed property **A**, which requires looping through a huge Array and doing a lot of computations. Then we may have other computed properties that in turn depend on **A**. Without caching, we would be executing **A**’s getter many more times than necessary! In cases where you do not want caching, use a method instead.
我们为什么需要缓存？假设我们有一个复杂的计算属性 **A**，这个计算属性需要一个巨大的数组遍历和做大量的计算。然后我们可能有其他的计算属性依赖于 **A**。如果没有缓存，我们将执行多次 **A** 的 getter 。然而这都是不必要的！如果你不希望有缓存，请用 method 替代。


### Computed vs Watched Property
### 计算属性 vs 监视器属性

Vue does provide a more generic way to observe and react to data changes on a Vue instance: **watch properties**. When you have some data that needs to change based on some other data, it is tempting to overuse `watch` - especially if you are coming from an AngularJS background. However, it is often a better idea to use a computed property rather than an imperative `watch` callback. Consider this example:
Vue.js 提供了一个更加通用的方法，它用于观察 Vue 实例上的数据变动。当一些数据需要根据其它数据变化时， `watch` 很诱人 —— 特别是如果你来自 AngularJS。不过，通常更好的办法是使用计算属性而不是一个命令式的 `watch` 回调。考虑下面例子：

``` html
<div id="demo">{{ fullName }}</div>
```

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

The above code is imperative and repetitive. Compare it with a computed property version:
上面代码是命令式的重复的。跟计算属性对比：

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Much better, isn't it?
这样更好，不是吗？

### Computed Setter
### 计算 setter

Computed properties are by default getter-only, but you can also provide a setter when you need it:
计算属性默认只是 getter ，不过在需要时你也可以提供一个 setter：

``` js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Now when you run `vm.fullName = 'John Doe'`, the setter will be invoked and `vm.firstName` and `vm.lastName` will be updated accordingly.
现在在调用 `vm.fullName = 'John Doe'` 时， setter 会被调用， `vm.firstName` 和 `vm.lastName` 也会有相应更新。

## Watchers
## 监视器

While computed properties are more appropriate in most cases, there are times when a custom watcher is necessary. That's why Vue provides a more generic way to react to data changes through the `watch` option. This is most useful when you want to perform asynchronous or expensive operations in response to changing data.
计算属性在大多数情况下会更合适，然而有时候一个自定义的 watcher 也是有必要的。这是为什么 Vue 提供一个更通用的方法通过 `watch` 的设置，来反应数据的变化。当你想执行异步或复杂的操作以响应不断变化的数据时这很有用。

For example:
例如：

``` html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

``` html
<!-- Since there is already a rich ecosystem of ajax libraries    -->
<!-- and collections of general-purpose utility methods, Vue core -->
<!-- is able to remain small by not reinventing them. This also   -->
<!-- gives you the freedom to just use what you're familiar with. -->
<!-- 因为有了一个丰富的ajax服务库和通用的方法集合类库 -->
<!-- Vue.js的核心文件才能不重复造轮子保持的这么小，这也 -->
<!-- 给你了足够的自由让你使用熟悉的东西。-->
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // whenever question changes, this function will run
	// 只要 question 发生变化，这个函数将执行
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
	// _.debounce 是一个通过 lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问yesno.wtf/api的频率
    // 一直等待直到用户发出ajax请求之前
    // 关于这个 _.debounce 函数 (和 它的同系函数
    // _.throttle), 访问: https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // This is the number of milliseconds we wait for the
      // user to stop typing.
	  // 这是我们为用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```

Result:

{% raw %}
<div id="watch-example" class="demo">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      500
    )
  }
})
</script>
{% endraw %}

In this case, using the `watch` option allows us to perform an asynchronous operation (accessing an API), limit how often we perform that operation, and set intermediary states until we get a final answer. None of that would be possible with a computed property.
在这个示例中，使用 `watch` 的设置允许我们执行异步操作（访问一个接口），限制我们多久执行该操作，并在我们获取最终结果时立刻设置状态。这是计算属性无法做到的。

In addition to the `watch` option, you can also use the imperative [vm.$watch API](/api/#vm-watch).
关于 `watch` 选项，可查看[vm.$watch API](/api/#vm-watch)。