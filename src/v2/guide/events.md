---
title: 事件处理 (Event Handling)
type: guide
order: 9
---

## 监听事件 (Listening to Events)

We can use the `v-on` directive to listen to DOM events and run some JavaScript when they're triggered.
我们可以使用 `v-on` 指令来监听 DOM 事件，并且在事件触发时运行代码逻辑。

For example:
比如：

``` html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```
``` js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

结果：

{% raw %}
<div id="example-1" class="demo">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</script>
{% endraw %}

## 方法作为事件处理器 (Method Event Handlers)

The logic for many event handlers will be more complex though, so keeping your JavaScript in the value of the `v-on` attribute simply isn't feasible. That's why `v-on` can also accept the name of a method you'd like to call.
很多事件处理器的逻辑要比上例更为复杂，这种情况下将 JavaScript 代码放在 `v-on` 属性的值里面并不现实。所以 `v-on` 还可以接受一个方法名作为值。

For example:
比如：

``` html
<div id="example-2">
  <!-- `greet` is the name of a method defined below -->
  <!-- `greet` 是下面定义的方法 -->
  <button v-on:click="greet">Greet</button>
</div>
```

``` js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // define methods under the `methods` object
  // 在 `methods` 对象里面定义方法
  methods: {
    greet: function (event) {
      // `this` inside methods points to the Vue instance
      // 方法里的 `this` 指向 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` is the native DOM event
      // `event` 是原生的 DOM 事件
      alert(event.target.tagName)
    }
  }
})

// you can invoke methods in JavaScript too
// 你也可以在 JavaScript 里调用方法
example2.greet() // -> 'Hello Vue.js!'
```

Result:
结果：

{% raw %}
<div id="example-2" class="demo">
  <button v-on:click="greet">Greet</button>
</div>
<script>
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
      alert(event.target.tagName)
    }
  }
})
</script>
{% endraw %}

## 内联处理器中的方法 (Methods in Inline Handlers)

Instead of binding directly to a method name, we can also use methods in an inline JavaScript statement:
除了直接绑定到方法名，我们还可以在内联 JavaScript 语句中使用方法：

``` html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```
``` js
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

Result:
结果：

{% raw %}
<div id="example-3" class="demo">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
<script>
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>
{% endraw %}

Sometimes we also need to access the original DOM event in an inline statement handler. You can pass it into a method using the special `$event` variable:
有时我们也需要在一个内联处理器中得到原始的 DOM 事件。你可以用特殊的 `$event` 变量将它传递给方法：

``` html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">Submit</button>
```

``` js
// ...
methods: {
  warn: function (message, event) {
    // now we have access to the native event
    // 现在我们就可以访问原生事件了
    if (event) event.preventDefault()
    alert(message)
  }
}
```

## 事件修饰符 (Event Modifiers)

It is a very common need to call `event.preventDefault()` or `event.stopPropagation()` inside event handlers. Although we can do this easily inside methods, it would be better if the methods can be purely about data logic rather than having to deal with DOM event details.
在事件处理器中，我们经常需要调用 `event.preventDefault()` 或者 `event.stopPropagation()`。虽然在方法里面这样做很简单，但如果方法可以只负责数据逻辑，而不用处理 DOM 事件的细节，那就更好了。

To address this problem, Vue provides **event modifiers** for `v-on`. Recall that modifiers are directive postfixes denoted by a dot.
为了解决这个问题，Vue 为 `v-on` 提供了 **事件修饰符**。回想一下，修饰符就是以 `.` 开始的指令后缀。

- `.stop`
- `.prevent`
- `.capture`
- `.self`

``` html
<!-- the click event's propagation will be stopped -->
<!-- click 事件的广播会被阻止 -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<!-- submit 事件不再会刷新页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<!-- 修饰符可以链式使用 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<!-- 单独使用修饰符 -->
<form v-on:submit.prevent></form>

<!-- use capture mode when adding the event listener -->
<!-- 在添加事件监听器时使用 capture 模式 -->
<div v-on:click.capture="doThis">...</div>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<!-- 只有当 event.taget 是元素本身时，才调用事件处理器 -->
<!-- 比如，如果 event.target 是一个子元素，那么 doThat 就不会被调用 -->
<div v-on:click.self="doThat">...</div>
```

## 按键修饰符 (Key Modifiers)

When listening for keyboard events, we often need to check for common key codes. Vue also allows adding key modifiers for `v-on` when listening for key events:
监听键盘事件时，我们经常需要查找常用的键码。Vue 允许在 `v-on` 添加按键修饰符：

``` html
<!-- only call vm.submit() when the keyCode is 13 -->
<!-- 只在键码为 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

Remembering all the keyCodes is a hassle, so Vue provides aliases for the most commonly used keys:
要记住所有的键码并不轻松，所以 Vue 给最常用的按键提供了别名：

``` html
<!-- same as above -->
<!-- 跟上一段示例代码效果一样 -->
<input v-on:keyup.enter="submit">

<!-- also works for shorthand -->
<!-- 缩写也没问题 -->
<input @keyup.enter="submit">
```

Here's the full list of key modifier aliases:
这是所有的按键修饰符别名：

- enter
- tab
- delete (captures both "Delete" and "Backspace" keys)
- delete （捕捉 "Delete" 和 "Backspace" 按键）
- esc
- space
- up
- down
- left
- right

You can also [define custom key modifier aliases](../api/#keyCodes) via the global `config.keyCodes` object:
你也可以通过全局的 `config.keyCodes` 对象来[自定义按键修饰符别名](../api/#keyCodes)：

``` js
// enable v-on:keyup.f1
// 启用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```

## 为什么要在 HTML 中写监听器 (Why Listeners in HTML?)

You might be concerned that this whole event listening approach violates the good old rules about "separation of concerns". Rest assured - since all Vue handler functions and expressions are strictly bound to the ViewModel that's handling the current view, it won't cause any maintenance difficulty. In fact, there are several benefits in using `v-on`:
你可能会担心，这种事件监听的方式违背了“关注点分离”的原则。请放心，所有的 Vue 事件处理器的函数和表达式都只会绑定到处理当前视图的 ViewModel，不会给代码维护带来任何负担。事实上，使用 `v-on` 有几个好处：

1. It's easier to locate the handler function implementations within your JS code by simply skimming the HTML template.
1. 更容易在 JS 代码中定位时间处理器函数的实现，你只需要快速浏览 HTML 模版就可以了；

2. Since you don't have to manually attach event listeners in JS, your ViewModel code can be pure logic and DOM-free. This makes it easier to test.
2. 因为你不需要在 JS 手动绑定事件处理器，所以你的 ViewModel 代码只是纯粹的逻辑，和 DOM 完全解耦。这会让 JS 代码更容易测试；

3. When a ViewModel is destroyed, all event listeners are automatically removed. You don't need to worry about cleaning it up yourself.
3. 当 ViewModel 被销毁时，所有的时间处理器也会自动被销毁。你不需要手动清理。
