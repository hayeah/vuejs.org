---
title: 计算属性和监视器 (Computed Properties and Watchers)
type: guide
order: 5
---

## 计算属性 (Computed Properties)

In-template expressions are very convenient, but they are really only meant for simple operations. Putting too much logic into your templates can make them bloated and hard to maintain. For example:
在模板中绑定表达式是非常便利的，但是它们实际上只用于简单的操作。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

``` html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

At this point, the template is no longer simple and declarative. You have to look at it for a second before realizing that it displays `message` in reverse. The problem is made worse when you want to include the reversed message in your template more than once.
这个时候，模版不再是单一的去做一件事情了。当你看一会这串代码才会意识到原来它展示的是反转之后的 `message` 。当你希望在你的模版中包含多次这种反转代码时，问题就变得更糟了。

That's why for any complex logic, you should use a **computed property**.
这就是为什么 Vue.js 将绑定表达式限制为一个表达式。如果需要多于一个表达式的逻辑，应当使用 **计算属性** 。

### 基础例子 (Basic Example)

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

### 计算缓存 vs Methods (Computed Caching vs Methods)

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

Instead of a computed property, we can define the same function as a method instead. For the end result, the two approaches are indeed exactly the same. However, the difference is that **computed properties are cached based on their dependencies.** A computed property will only re-evaluate when some of its dependencies have changed. This means as long as `message` has not changed, multiple access to the `reversedMessage` computed property will immediately return the previously computed result without having to run the function again.
我们可以定义一个相同功能的方法来代替计算属性。从最终显示的结果来看，这两种方法确实是完全相同的。然而，不同的是 **计算属性基于它的依赖关系被缓存。** 当它依赖关系中的某些值发生改变时这个计算属性将会仅仅对这些改变的值进行重新求值。也就是说，只要 `message` 不发生改变，多次请求 `reversedMessage` 的话计算属性将会立刻返回上次计算的结果而不用重新计算。

This also means the following computed property will never update, because `Date.now()` is not a reactive dependency:
这也意味着下面这个计算属性将不会更新，因为 `Date.now()` 不是一个被动依赖：


``` js
computed: {
  now: function () {
    return Date.now()
  }
}
```

In comparison, a method invocation will **always** run the function whenever a re-render happens.
相比之下，方法调用将会 **总是** 执行，只要发生重新渲染。


Why do we need caching? Imagine we have an expensive computed property **A**, which requires looping through a huge Array and doing a lot of computations. Then we may have other computed properties that in turn depend on **A**. Without caching, we would be executing **A**’s getter many more times than necessary! In cases where you do not want caching, use a method instead.
为什么我们需要缓存呢？想象一下我们有一个复杂的计算属性 **A**，这个属性是我们遍历一个庞大的数组经过大量的计算得到的。然后我们可能有其他的计算属性反过来依赖 **A**。如果没有缓存，我们可以边喝着咖啡边计算 **A** 的值了。当然，你可以在不需要缓存的情况下用一个方法来代替。

### 计算属性 vs 观察属性 (Computed vs Watched Property)

Vue does provide a more generic way to observe and react to data changes on a Vue instance: **watch properties**. When you have some data that needs to change based on some other data, it is tempting to overuse `watch` - especially if you are coming from an AngularJS background. However, it is often a better idea to use a computed property rather than an imperative `watch` callback. Consider this example:
Vue.js 提供了一个更加通用的方法，它用于观察 Vue 实例上的数据变动。当一些数据需要根据其它数据变化时，`watch` 很诱人 —— 特别是如果你来自 AngularJS。不过，通常更好的办法是使用计算属性而不是一个命令式的 `watch` 回调。考虑下面例子：

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

### 计算 setter (Computed Setter)

Computed properties are by default getter-only, but you can also provide a setter when you need it:
计算属性默认只是 getter，不过在需要时你也可以提供一个 setter：

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
现在在调用 `vm.fullName = 'John Doe'` 时，setter 会被调用，`vm.firstName` 和 `vm.lastName` 也会有相应更新。

## 监视器 (Watchers)

While computed properties are more appropriate in most cases, there are times when a custom watcher is necessary. That's why Vue provides a more generic way to react to data changes through the `watch` option. This is most useful when you want to perform asynchronous or expensive operations in response to changing data.
计算属性在大多数情况下会更合适，然而有时候一个自定义的监视器也是有必要的。这就是为什么 Vue.js 提供了一个更加通用的方法即通过 `watch` 选项直接影响数据改变的原因。当你想执行异步或复杂的操作以响应不断变化的数据时，这是最有用的。

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
    // _.debounce 是 lodash 库提供的方法，用来限制
    // 一个特定的复杂的操作多久一次可以执行
    // 既然这样, 我们要限制我们访问的 yesno.wtf/api
    // 等待直到用户完成输入之前的Ajax请求。 了解更多
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
      // 用户键入后等待 500 毫秒后执行
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
在这种情况下，使用 `watch` 选项允许我们执行一个异步操作（访问一个API），限制我们执行这个操作，并设置等待状态直到我们得到一个最终的答案。这是任何一个计算属性做不到的。

In addition to the `watch` option, you can also use the imperative [vm.$watch API](../api/#vm-watch).
除了 `watch` 选项之外，你也可以使用这个命令[vm.$watch API](../api/#vm-watch)。
