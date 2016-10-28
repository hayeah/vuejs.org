---
title: 过渡效果 (Transition Effects)
type: guide
order: 13
---

## 概况 (Overview)

Vue provides a variety of ways to apply transition effects when items are inserted, updated, or removed from the DOM. This includes tools to:
Vue 为插入元素，更新元素，以及从 DOM 中移除元素提供了多种过渡效果。包括以下工具：

- automatically apply classes for CSS transitions and animations
- 自动应用 CSS 过渡效果和动画的类
- integrate 3rd-party CSS animation libraries, such as Animate.css
- 整合第三方 CSS 动画库如 Animate.css
- use JavaScript to directly manipulate the DOM during transition hooks
- 在过渡效果的钩子中使用 JavaScript 直接控制 DOM
- integrate 3rd-party JavaScript animation libraries, such as Velocity.js
- 整合第三方 JavaScript 动画库例如 Velocity.js

On this page, we'll only cover entering, leaving, and list transitions, but you can see the next section for [managing state transitions](transitioning-state.html).
本节教程只会涉及进入、离开和列表的过渡效果，关于如何[管理状态的过渡](transitioning-state.html)，你可以阅读下一节。

## 单一元素/组件过渡 (Transitioning Single Elements/Components)

Vue provides a `transition` wrapper component, allowing you to add entering/leaving transitions for any element or component in the following contexts:
Vue 提供一个 `transition` 包裹组件，使你能够在下列情况中，为任意元素或组件添加进入或离开的过渡效果：

- Conditional rendering (using `v-if`)
- 条件渲染（使用 `v-if`）
- Conditional display (using `v-show`)
- 条件展示（使用 `v-show`）
- Dynamic components
- 动态组件
- Component root nodes
- 组件根节点

This is what a very simple example looks like in action:
这是实际应用中的一个简单的例子：

``` html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

``` css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-active {
  opacity: 0
}
```

{% raw %}
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="demo-transition">
    <p v-if="show">hello</p>
  </transition>
</div>
<script>
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
</script>
<style>
.demo-transition-enter-active, .demo-transition-leave-active {
  transition: opacity .5s
}
.demo-transition-enter, .demo-transition-leave-active {
  opacity: 0
}
</style>
{% endraw %}

When an element wrapped in a `transition` component is inserted or removed, this is what happens:
当被包裹在 `transition` 中的元素插入 DOM，或从 DOM 中移除时，Vue 将：

1. Vue will automatically sniff whether the target element has CSS transitions or animations applied. If it does, CSS transition classes will be added/removed at appropriate timings.
1. 自动嗅探目标元素是否应用了 CSS 过渡或动画。如果有，将会在合适时添加/删除 CSS 过渡的类；

2. If the transition component provided [JavaScript hooks](#JavaScript-Hooks), these hooks will be called at appropriate timings.
2. 如果过渡组件提供了 [JavaScript 钩子](#JavaScript-Hooks)，这些钩子函数将会在适当的时候被调用；

3. If no CSS transitions/animations are detected and no JavaScript hooks are provided, the DOM operations for insertion and/or removal will be executed immediately on next frame (Note: this is a browser animation frame, different from Vue's concept of `nextTick`).
3. 如果既没有检测到 CSS 过渡/动画也没有提供 JavaScript 钩子，DOM 操作（插入/删除）将会在下一帧中立即执行。（注意：这里的帧指浏览器动画帧，与 Vue 中 `nextTick` 的概念不同）

### 过渡类名 (Transition Classes)

There are four classes applied for enter/leave transitions.
进入/离开的过渡将会使用到下列四个类名：

1. `v-enter`: Starting state for enter. Applied before element is inserted, removed after one frame.
1. `v-enter`：进入过渡的开始状态。在元素被插入前或元素移除后的下一帧应用；
2. `v-enter-active`: Active and ending state for enter. Applied before element is inserted, removed when transition/animation finishes.
2. `v-enter-active`：进入过渡的活跃和结束状态。在元素被插入前应用，当过渡/动画结束时被删除；
3. `v-leave`: Starting state for leave. Applied when leave transition is triggered, removed after one frame.
3. `v-leave`：离开过渡的开始状态。当离开过渡开始时生效，只应用一帧然后立即被删除；
4. `v-leave-active`: Active and ending state for leave. Applied when leave transition is triggered, removed when the transition/animation finishes.
4. `v-leave-active`：离开过渡的活跃和结束状态。当离开过渡开始时生效，当过渡/动画结束后立即被删除。

Each of these classes will be prefixed with the name of the transition. Here the `v-` prefix is the default when you use a `<transition>` element with no name. If you use `<transition name="my-transition">` for example, then the `v-enter` class would instead be `my-transition-enter`.
每一个类的前缀都是对应的过渡的 name 是以上类名的前缀。当 `<transition>` 没有 name 时，则默认为这里的 `v-` 前缀。如果你使用 `<transition name="my-transition">` ， `v-enter` 类名应该改为 `my-transition-enter`。

`v-enter-active` and `v-leave-active` give you the ability to specify different easing curves for enter/leave transitions, which you'll see an example of in the following section.
`v-enter-active` 和 `v-leave-active` 将使你能够为进入/离开过渡效果声明不同的缓冲曲线，你将能在下一节看到例子。

### CSS 过渡 (CSS Transitions)

One of the most common transition types uses CSS transitions. Here's a simple example:
CSS 过渡是最常用的一种过渡类型。例如：

``` html
<div id="example-1">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition name="slide-fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
```

``` css
/* Enter and leave animations can use different */
/* durations and timing functions.              */
/* 进入和离开的动画可以使用不同的 duration 和 timing 函数 */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-active {
  padding-left: 10px;
  opacity: 0;
}
```

{% raw %}
<div id="example-1" class="demo">
  <button @click="show = !show">
    Toggle
  </button>
  <transition name="slide-fade">
    <p v-if="show">hello</p>
  </transition>
</div>
<script>
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
</script>
<style>
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-active {
  padding-left: 10px;
  opacity: 0;
}
</style>
{% endraw %}

### CSS 动画 (CSS Animations)

CSS animations are applied in the same way as CSS transitions, the difference being that `v-enter` is not removed immediately after the element is inserted, but on an `animationend` event.
CSS 动画用法同 CSS 过渡，区别是在动画中 `v-enter` 类名在节点插入 DOM 后不会立即删除，而是在 `animationend` 事件触发时删除。

Here's an example, omitting prefixed CSS rules for the sake of brevity:
为了简洁，下面的示例省略了兼容浏览器的 CSS 前缀：

``` html
<div id="example-2">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-if="show">Look at me!</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})
```

``` css
.bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-out .5s;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes bounce-out {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(0);
  }
}
```

{% raw %}
<div id="example-2" class="demo">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-show="show">Look at me!</p>
  </transition>
</div>

<style>
  .bounce-enter-active {
    -webkit-animation: bounce-in .5s;
    animation: bounce-in .5s;
  }
  .bounce-leave-active {
    -webkit-animation: bounce-out .5s;
    animation: bounce-out .5s;
  }
  @keyframes bounce-in {
    0% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
  }
  @keyframes bounce-out {
    0% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
  }
  @-webkit-keyframes bounce-in {
    0% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
  }
  @-webkit-keyframes bounce-out {
    0% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
  }
</style>
<script>
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})
</script>
{% endraw %}

### 自定义过渡类名 (Custom Transition Classes)

You can also specify custom transition classes by providing the following attributes:
你也可以通过提供以下属性来声明自定义过渡类名：

- `enter-class`
- `enter-active-class`
- `leave-class`
- `leave-active-class`

These will override the conventional class names. This is especially useful when you want to combine Vue's transition system with an existing CSS animation library, such as [Animate.css](https://daneden.github.io/animate.css/).
这些会覆盖传统类名。当你想结合 Vue 的过渡系统和已有的 CSS 动画库如 [Animate.css](https://daneden.github.io/animate.css/) 时非常有用。

Here's an example:
例如：

``` html
<link href="https://unpkg.com/animate.css@3.5.1/animate.min.css" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})
```

{% raw %}
<link href="https://unpkg.com/animate.css@3.5.1" rel="stylesheet" type="text/css">
<div id="example-3" class="demo">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>
<script>
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})
</script>
{% endraw %}

### 过渡与动画一起使用 (Using Transitions and Animations Together)

Vue needs to attach event listeners in order to know when a transition has ended. It can either be `transitionend` or `animationend`, depending on the type of CSS rules applied. If you are only using one or the other, Vue can automatically detect the correct type.
Vue 需要通过添加事件监听器来获知一次过渡是否结束。这个事件可以是 `transitionend` 或 `animationend`，取决于应用的 CSS 类型。如果你一次只使用两者中的一种，Vue 将会自动检测对应的事件类型。

However, in some cases you may want to have both on the same element, for example having a CSS animation triggered by Vue, along with a CSS transition effect on hover. In these cases, you will have to explicitly declare the type you want Vue to care about in a `type` attribute, with a value of either `animation` or `transition`.
然而，在某些情况下同一个目标元素可能同时应用过渡和动画，例如某个元素能被 Vue 触发动画效果，同时在鼠标悬浮时也有一个 CSS 过渡效果。这种情况下，你需要在 `type` 属性中明确地声明你想让 Vue 处理的过渡类型， `type` 属性的值可以是 `animation` 或 `transition`。

### JavaScript 钩子 (JavaScript Hooks)

You can also define JavaScript hooks in attributes:
你也能在属性中定义 JavaScript 钩子函数：

``` html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

``` js
// ...
methods: {
  // --------
  // ENTERING
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // the done callback is optional when
  // used in combination with CSS
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // LEAVING
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // the done callback is optional when
  // used in combination with CSS
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled only available with v-show
  leaveCancelled: function (el) {
    // ...
  }
}
```

These hooks can be used in combination with CSS transitions/animations or on their own.
这些钩子函数能结合 CSS 过渡/动画，也能独立使用。

<p class="tip">When using JavaScript-only transitions, **the `done` callbacks are required for the `enter` and `leave` hooks**. Otherwise, they will be called synchronously and the transition will finish immediately.
当使用纯 JavaScript 过渡时， **需要在 `enter` 和 `leave` 中提供 `done` 回调函数**。否则 `enter` 和 `leave` 就会被同步调用，过渡立即结束。</p>

<p class="tip">It's also a good idea to explicitly add `v-bind:css="false"` for JavaScript-only transitions so that Vue can skip the CSS detection. This also prevents CSS rules from accidentally interfering with the transition.
同时也推荐为纯 JavaScript 过渡添加 `v-bind:css="false"` 以跳过 CSS 检测，这也能有效防止过渡中 CSS 的意外干扰。</p>

Now let's dive into an example. Here's a simple JavaScript transition using Velocity.js:
让我们来看一个例子。这是应用了 Velocity.js 的简单 JavaScript 过渡效果

``` html
<!--
Velocity works very much like jQuery.animate and is
a great option for JavaScript animations
-->
<!--
Velocity 跟 jQuery.animate 类似，
它是实现 JavaScript 动画的绝佳选择
-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="example-4">
  <button @click="show = !show">
    Toggle
  </button>
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
    v-bind:css="false"
  >
    <p v-if="show">
      Demo
    </p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-4',
  data: {
    show: false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
      Velocity(el, { fontSize: '1em' }, { complete: done })
    },
    leave: function (el, done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '45deg',
        translateY: '30px',
        translateX: '30px',
        opacity: 0
      }, { complete: done })
    }
  }
})
```

{% raw %}
<div id="example-4" class="demo">
  <button @click="show = !show">
    Toggle
  </button>
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">
      Demo
    </p>
  </transition>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<script>
new Vue({
  el: '#example-4',
  data: {
    show: false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.transformOrigin = 'left'
    },
    enter: function (el, done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
      Velocity(el, { fontSize: '1em' }, { complete: done })
    },
    leave: function (el, done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '45deg',
        translateY: '30px',
        translateX: '30px',
        opacity: 0
      }, { complete: done })
    }
  }
})
</script>
{% endraw %}

## 初始渲染的过渡 (Transitions on Initial Render)

If you also want to apply a transition on the initial render of a node, you can add the `appear` attribute:
如果想在节点的第一次渲染时应用过渡效果，你可以添加 `appear` 属性：

``` html
<transition appear>
  <!-- ... -->
</transition>
```

By default, this will use the transitions specified for entering and leaving. If you'd like however, you can also specify custom CSS classes:
默认地，这会使用进入/离开的过渡。但如果你喜欢，也可以声明自定义 CSS 类名：

``` html
<transition
  appear
  appear-class="custom-appear-class"
  appear-active-class="custom-appear-active-class"
>
  <!-- ... -->
</transition>
```

and custom JavaScript hooks:
和自定义 JavaScript 钩子函数：

``` html
<transition
  appear
  v-on:before-appear="customBeforeAppearHook"
  v-on:appear="customAppearHook"
  v-on:after-appear="customAfterAppearHook"
>
  <!-- ... -->
</transition>
```

## 元素之间的过渡 (Transitioning Between Elements)

We discuss [transitioning between components](#Transitioning-Between-Components) later, but you can also transition between raw elements using `v-if`/`v-else`. One of the most common two-element transitions is between a list container and a message describing an empty list:
我们将会在稍后讨论 [组件之间的过渡](#Transitioning-Between-Components)，你也可以使用 `v-if`/`v-else` 进行普通元素之间的过渡。最常见的两元素过渡之一是列表包裹容器和描述空列表的提示之间的过渡：

``` html
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

This works well, but there's one caveat to be aware of:
这事例没问题，但需要注意：

<p class="tip">When toggling between elements that have **the same tag name**, you must tell Vue that they are distinct elements by giving them unique `key` attributes. Otherwise, Vue's compiler will only replace the content of the element for efficiency. Even when technically unnecessary though, **it's considered good practice to always key multiple items within a `<transition>` component.**
当在两个标签名相同的元素之间过渡时，必须通过赋予它们唯一的 `key` 属性值以告诉 Vue 它们是不同的元素。否则，Vue 的编译器会为了高效率，单纯替换标签中的内容。即使在技术上看起来没那么必要，**在 `<transition>` 组件中总是为多个元素分别提供唯一的 `key` 值依然是最佳实践。**<
</p>

For example:
例如：

``` html
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

In these cases, you can also use the `key` attribute to transition between different states of the same element. Instead of using `v-if` and `v-else`, the above example could be rewritten as:
在这些情况下，也能使用 `key` 属性使同一元素在不同状态下过渡。除了使用 `v-if` 和 `v-else` ，上面的例子也能重写为：

``` html
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

It's actually possible to transition between any number of elements, either by using multiple `v-if`s or binding a single element to a dynamic property. For example:
在任意数量的元素中过渡也是可行的，可使用多个 `v-if`，或将单一元素绑定到一个动态属性中。例如：

``` html
<transition>
  <button v-if="docState === 'saved'" key="saved">
    Edit
  </button>
  <button v-if="docState === 'edited'" key="edited">
    Save
  </button>
  <button v-if="docState === 'editing'" key="editing">
    Cancel
  </button>
</transition>
```

Which could also be written as:
可被写为：

``` html
<transition>
  <button v-bind:key="docState">
    {{ buttonMessage }}
  </button>
</transition>
```

``` js
// ...
computed: {
  buttonMessage: function () {
    switch (docState) {
      case 'saved': return 'Edit'
      case 'edited': return 'Save'
      case 'editing': return 'Cancel'
    }
  }
}
```

### 过渡模式 (Transition Modes)

There's still one problem though. Try clicking the button below:
还有一个问题。试试点击下面的按钮：

{% raw %}
<div id="no-mode-demo" class="demo">
  <transition name="no-mode-fade">
    <button v-if="on" key="on" @click="on = false">
      on
    </button>
    <button v-else key="off" @click="on = true">
      off
    </button>
  </transition>
</div>
<script>
new Vue({
  el: '#no-mode-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-fade-enter-active, .no-mode-fade-leave-active {
  transition: opacity .5s
}
.no-mode-fade-enter, .no-mode-fade-leave-active {
  opacity: 0
}
</style>
{% endraw %}

As it's transitioning between the "on" button and the "off" button, both buttons are rendered - one transitioning out while the other transitions in. This is the default behavior of `<transition>` - entering and leaving happens simultaneously.
因为是在 “on” 和 “off” 这两个按钮间过渡，两个按钮均被渲染————一个过渡离开而另一个过渡进入。这是 `<transition>` 的默认行为————元素或组件间的进入与离开同时发生。

Sometimes this works great, like when transitioning items are absolutely positioned on top of each other:
有时这很有用，像是两个绝对定位的叠加在对方上的元素过渡时：

{% raw %}
<div id="no-mode-absolute-demo" class="demo">
  <div class="no-mode-absolute-demo-wrapper">
    <transition name="no-mode-absolute-fade">
      <button v-if="on" key="on" @click="on = false">
        on
      </button>
      <button v-else key="off" @click="on = true">
        off
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#no-mode-absolute-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-absolute-demo-wrapper {
  position: relative;
  height: 18px;
}
.no-mode-absolute-demo-wrapper button {
  position: absolute;
}
.no-mode-absolute-fade-enter-active, .no-mode-absolute-fade-leave-active {
  transition: opacity .5s;
}
.no-mode-absolute-fade-enter, .no-mode-absolute-fade-leave-active {
  opacity: 0;
}
</style>
{% endraw %}

And then maybe also translated so that they look like slide transitions:
或是使元素位移形成滑动过渡的效果：
{% raw %}
<div id="no-mode-translate-demo" class="demo">
  <div class="no-mode-translate-demo-wrapper">
    <transition name="no-mode-translate-fade">
      <button v-if="on" key="on" @click="on = false">
        on
      </button>
      <button v-else key="off" @click="on = true">
        off
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#no-mode-translate-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-translate-demo-wrapper {
  position: relative;
  height: 18px;
}
.no-mode-translate-demo-wrapper button {
  position: absolute;
}
.no-mode-translate-fade-enter-active, .no-mode-translate-fade-leave-active {
  transition: all 1s;
}
.no-mode-translate-fade-enter, .no-mode-translate-fade-leave-active {
  opacity: 0;
}
.no-mode-translate-fade-enter {
  transform: translateX(31px);
}
.no-mode-translate-fade-leave-active {
  transform: translateX(-31px);
}
</style>
{% endraw %}

Simultaneous entering and leaving transitions aren't always desirable though, so Vue offers some alternative **transition modes**:
然而同时进入离开的过渡不总是令人满意。因此 Vue 提供了一些额外的 **过渡状态**：

- `in-out`: New element transitions in first, then when complete, the current element transitions out.
-  `in-out`：新元素先过渡进入，当过渡完成后，当前元素过渡离开。
- `out-in`: Current element transitions out first, then when complete, the new element transitions in.
- `out-in`：当前元素先过渡离开，当过渡完成后，新元素过渡进入。

Now let's update the transition for our on/off buttons with `out-in`:
现在使用 `out-in` 重写开/关按钮的过渡效果：
``` html
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
  <!-- ... 按钮 ... -->
</transition>
```

{% raw %}
<div id="with-mode-demo" class="demo">
  <transition name="with-mode-fade" mode="out-in">
    <button v-if="on" key="on" @click="on = false">
      on
    </button>
    <button v-else key="off" @click="on = true">
      off
    </button>
  </transition>
</div>
<script>
new Vue({
  el: '#with-mode-demo',
  data: {
    on: false
  }
})
</script>
<style>
.with-mode-fade-enter-active, .with-mode-fade-leave-active {
  transition: opacity .5s
}
.with-mode-fade-enter, .with-mode-fade-leave-active {
  opacity: 0
}
</style>
{% endraw %}

With one simple attribute addition, we've fixed that original transition without having to add any special styling.
这一简单的添加属性解决了原本过渡只能同时进入与离开的问题，并且没有额外添加任何特殊样式。

The `in-out` mode isn't used as often, but can sometimes be useful for a slightly different transition effect. Let's try combining it with the slide-fade transition we worked on earlier:
 `in-out` 模式没有  `out-in` 的使用频率高，但有时也能在一些稍微不同的过渡效果中非常有用。尝试结合前面的滑动-褪色过渡效果：

{% raw %}
<div id="in-out-translate-demo" class="demo">
  <div class="in-out-translate-demo-wrapper">
    <transition name="in-out-translate-fade" mode="in-out">
      <button v-if="on" key="on" @click="on = false">
        on
      </button>
      <button v-else key="off" @click="on = true">
        off
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#in-out-translate-demo',
  data: {
    on: false
  }
})
</script>
<style>
.in-out-translate-demo-wrapper {
  position: relative;
  height: 18px;
}
.in-out-translate-demo-wrapper button {
  position: absolute;
}
.in-out-translate-fade-enter-active, .in-out-translate-fade-leave-active {
  transition: all .5s;
}
.in-out-translate-fade-enter, .in-out-translate-fade-leave-active {
  opacity: 0;
}
.in-out-translate-fade-enter {
  transform: translateX(31px);
}
.in-out-translate-fade-leave-active {
  transform: translateX(-31px);
}
</style>
{% endraw %}

Pretty cool, right?
很帅吧？

## 组件间的过渡 (Transitioning Between Components)

Transitioning between components is even simpler - we don't even need the `key` attribute. Instead, we just wrap a [dynamic component](components.html#Dynamic-Components):
组件间的过渡更加简单————甚至不需要 `key` 属性。我们只需包裹一个[动态组件](components.html#Dynamic-Components)：

``` html
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>
```

``` js
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  }
})
```

``` css
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-active {
  opacity: 0;
}
```

{% raw %}
<div id="transition-components-demo" class="demo">
  <input v-model="view" type="radio" value="v-a" id="a" name="view"><label for="a">A</label>
  <input v-model="view" type="radio" value="v-b" id="b" name="view"><label for="b">B</label>
  <transition name="component-fade" mode="out-in">
    <component v-bind:is="view"></component>
  </transition>
</div>
<style>
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-active {
  opacity: 0;
}
</style>
<script>
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  }
})
</script>
{% endraw %}

## 列表过渡 (List Transitions)

So far, we've managed transitions for:
至今为止，我们已经了解了以下过渡：

- Individual nodes
- 独立节点
- Multiple nodes where only 1 is rendered at a time
- 多节点，每次只渲染一个节点

So what about for when we have a whole list of items we want to render simultaneously, for example with `v-for`? In this case, we'll use the `<transition-group>` component. Before we dive into an example though, there are a few things that are important to know about this component:
那么当试图同时渲染一个列表，例如说使用 `v-for` ，该怎么办呢？在这种情况下，我们使用 `<transition-group>` 组件。在看例子前，使用该组件有几个注意事项：

- Unlike `<transition>`, it renders an actual element: a `<span>` by default. You can change the element that's rendered with the `tag` attribute.
- 与 `<transition>` 不同，它渲染出一个真实的元素：默认为 `<span>`。可以通过修改 `tag` 属性改变渲染出的元素类型。
- Elements inside are **always required** to have a unique `key` attribute
- 被包裹在里面的元素**总是需要**一个唯一的 `key` 属性值。

### 列表进入/离开过渡 (List Entering/Leaving Transitions)

Now let's dive into a simple example, transitioning entering and leaving using the same CSS classes we've used previously:
现在来看一个简单的例子，使用前面的过渡进入/离开 CSS 类名：

``` html
<div id="list-demo" class="demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
```

``` css
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-active {
  opacity: 0;
  transform: translateY(30px);
}
```

{% raw %}
<div id="list-demo" class="demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" :key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
<script>
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
</script>
<style>
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-active {
  opacity: 0;
  transform: translateY(30px);
}
</style>
{% endraw %}

There's one problem with this example. When you add or remove an item, the ones around it instantly snap into their new place instead of smoothly transitioning. We'll fix that later.
这个示例有一个问题：当你添加或移除列表项，其它的列表项会马上出现在新的位置而不是平滑过渡。我们稍后回来修复这个问题。

### 列表移动过渡 (List Move Transitions)

The `<transition-group>` component has another trick up its sleeve. It can not only animate entering and leaving, but also changes in position. The only new concept you need to know to use this feature is the addition of **the `v-move` class**, which is added when items are changing positions. Like the other classes, its prefix will match the value of a provided `name` attribute and you can also manually specify a class with the `move-class` attribute.
另一个 `<transition-group>` 组件的使用技巧是：它不仅能将进入和离开动效化，还可以让位置改变获得动效。为了使用这个特性，你唯一需要知道的新概念是** `v-move` 类**的添加。当列表项的位置发生改变，`v-move` 类便会被添加。像其它类一样，它的前缀也与组件提供的 `name` 属性值相同，你也可以使用 `move-class` 属性手动声明这个类。

This class is mostly useful for specifying the transition timing and easing curve, as you'll see below:
正如下面所示，这个类会在定义过渡时间和缓冲曲线时发挥最大作用：

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
```

``` css
.flip-list-move {
  transition: transform 1s;
}
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" :key="item">
      {{ item }}
    </li>
  </transition-group>
</div>
<script>
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
</script>
<style>
.flip-list-move {
  transition: transform 1s;
}
</style>
{% endraw %}

This might seem like magic, but under the hood, Vue is using a simple animation technique called [FLIP](https://aerotwist.com/blog/flip-your-animations/) to smoothly transition elements from their old position to their new position using transforms.
这可能看起来很神奇，但实际上 Vue 使用了一个简单的动画技术 [FLIP](https://aerotwist.com/blog/flip-your-animations/)，使元素能够从旧位置平滑过渡到新位置。

We can combine this technique with our previous implementation to animate every possible change to our list!
现在我们可以结合前面的实现，让列表的任意改变都能拥有动画效果：

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="list-complete-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list-complete" tag="p">
    <span
      v-for="item in items"
      v-bind:key="item"
      class="list-complete-item"
    >
      {{ item }}
    </span>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#list-complete-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
```

``` css
.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-active {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
<div id="list-complete-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list-complete" tag="p">
    <span v-for="item in items" :key="item" class="list-complete-item">
      {{ item }}
    </span>
  </transition-group>
</div>
<script>
new Vue({
  el: '#list-complete-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
</script>
<style>
.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-active {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
</style>
{% endraw %}

<p class="tip">One important note is that these FLIP transitions do not work with elements set to `display: inline`. As an alternative, you can use `display: inline-block` or place elements in a flex context.
特别要注意，FLIP 过渡无法应用在 `display: inline` 的元素上。备选的方案是，使用 `display: inline-block` 或把元素放在一个 flex 布局中。</p>

These FLIP animations are also not limited to a single axis. Items in a multidimensional grid can transitioned [just as easily](https://jsfiddle.net/chrisvfritz/sLrhk1bc/):
这些 FLIP 动画不局限于单个轴。在多维的网格中的列表也能[同样轻松](https://jsfiddle.net/chrisvfritz/sLrhk1bc/)地过渡：

{% raw %}
<div id="sudoku-demo" class="demo">
  <strong>Lazy Sudoku</strong>
  <p>Keep hitting the shuffle button until you win.</p>
  <button @click="shuffle">
    Shuffle
  </button>
  <transition-group name="cell" tag="div" class="sudoku-container">
    <div v-for="cell in cells" :key="cell.id" class="cell">
      {{ cell.number }}
    </div>
  </transition-group>
</div>
<script>
new Vue({
  el: '#sudoku-demo',
  data: {
    cells: Array.apply(null, { length: 81 })
      .map(function (_, index) {
        return {
          id: index,
          number: index % 9 + 1
        }
      })
  },
  methods: {
    shuffle: function () {
      this.cells = _.shuffle(this.cells)
    }
  }
})
</script>
<style>
.sudoku-container {
  display: flex;
  flex-wrap: wrap;
  width: 238px;
  margin-top: 10px;
}
.cell {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 25px;
  height: 25px;
  border: 1px solid #aaa;
  margin-right: -1px;
  margin-bottom: -1px;
}
.cell:nth-child(3n) {
  margin-right: 0;
}
.cell:nth-child(27n) {
  margin-bottom: 0;
}
.cell-move {
  transition: transform 1s;
}
</style>
{% endraw %}

### 错开列表过渡 (Staggering List Transitions)

By communicating with JavaScript transitions through data attributes, it's also possible to stagger transitions in a list:
通过在 JavaScript 过渡中使用数据属性，你还能在列表中实现错开过渡：

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="staggered-list-demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#staggered-list-demo',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<div id="example-5" class="demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>
<script>
new Vue({
  el: '#example-5',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
</script>
{% endraw %}

## 可重用过渡 (Reusable Transitions)

Transitions can be reused through Vue's component system. To create a reusable transition, all you have to do is place a `<transition>` or `<transition-group>` component at the root, then pass any children into the transition component.
通过 Vue 的组件系统，过渡也可以被重用。要创建一个可重用过渡，只需将 `<transition>` 或 `<transition-group>` 组件置于根节点，并向过渡组件中传入任意子节点。

``` js
Vue.component('my-special-transition', {
  template: '\
    <transition\
      name="very-special-transition"\
      mode="out-in"\
      v-on:before-enter="beforeEnter"\
      v-on:after-enter="afterEnter"\
    >\
      <slot></slot>\
    </transition>\
  ',
  methods: {
    beforeEnter: function (el) {
      // ...
    },
    afterEnter: function (el) {
      // ...
    }
  }
})
```

And functional components are especially well-suited to this task:
函数式组件尤其适合这种情况：

``` js
Vue.component('my-special-transition', {
  functional: true,
  render: function (createElement, context) {
    var data = {
      props: {
        name: 'very-special-transition'
        mode: 'out-in'
      },
      on: {
        beforeEnter: function (el) {
          // ...
        },
        afterEnter: function (el) {
          // ...
        }
      }
    }
    return createElement('transition', data, context.children)
  }
})
```

## 动态过渡 (Dynamic Transitions)

Yes, even transitions in Vue are data-driven! The most basic example of a dynamic transition binds the `name` attribute to a dynamic property.
Vue 中的动态过渡也是数据驱动的。动态过渡最简单的一个例子，就是将 `name` 属性被绑定到一个动态属性：

```html
<transition v-bind:name="transitionName">
  <!-- ... -->
</transition>
```

This can be useful when you've defined CSS transitions/animations using Vue's transition class conventions and simply want to switch between them.
当你使用 Vue 的过渡类名约定定义多个 CSS 过渡/动画类名并想在它们之间转换时，动态绑定显得非常有用。

Really though, any transition attribute can be dynamically bound. And it's not just attributes. Since event hooks are just methods, they have access to any data in the context. That means depending on the state of your component, your JavaScript transitions can behave differently.
实际上，任意过渡属性都能被动态绑定。而且不仅仅是属性，因为钩子只是函数，所以钩子也能从上下文获取数据。这意味着你的 JavaScript 过渡能根据组件的状态而拥有不同的行为。

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="dynamic-fade-demo">
  Fade In: <input type="range" v-model="fadeInDuration" min="0" v-bind:max="maxFadeDuration">
  Fade Out: <input type="range" v-model="fadeOutDuration" min="0" v-bind:max="maxFadeDuration">
  <transition
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">hello</p>
  </transition>
  <button v-on:click="stop = true">Stop it!</button>
</div>
```

``` js
new Vue({
  el: '#dynamic-fade-demo',
  data: {
    show: true,
    fadeInDuration: 1000,
    fadeOutDuration: 1000,
    maxFadeDuration: 1500,
    stop: false
  },
  mounted: function () {
    this.show = false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 1 },
        {
          duration: this.fadeInDuration,
          complete: function () {
            done()
            if (!vm.stop) vm.show = false
          }
        }
      )
    },
    leave: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 0 },
        {
          duration: this.fadeOutDuration,
          complete: function () {
            done()
            vm.show = true
          }
        }
      )
    }
  }
})
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<div id="dynamic-fade-demo" class="demo">
  Fade In: <input type="range" v-model="fadeInDuration" min="0" v-bind:max="maxFadeDuration">
  Fade Out: <input type="range" v-model="fadeOutDuration" min="0" v-bind:max="maxFadeDuration">
  <transition
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">hello</p>
  </transition>
  <button v-on:click="stop = true">Stop it!</button>
</div>
<script>
new Vue({
  el: '#dynamic-fade-demo',
  data: {
    show: true,
    fadeInDuration: 1000,
    fadeOutDuration: 1000,
    maxFadeDuration: 1500,
    stop: false
  },
  mounted: function () {
    this.show = false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 1 },
        {
          duration: this.fadeInDuration,
          complete: function () {
            done()
            if (!vm.stop) vm.show = false
          }
        }
      )
    },
    leave: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 0 },
        {
          duration: this.fadeOutDuration,
          complete: function () {
            done()
            vm.show = true
          }
        }
      )
    }
  }
})
</script>
{% endraw %}

Finally, the ultimate way of creating dynamic transitions is through components that accept props to change the nature of the transition(s) to be used. It may sound cheesy, but the only limit really is your imagination.
最后，创建动态过渡的终极方法是，就是通过接收属性的组件来改变过渡。虽然听来有点俗，不过唯一限制着你的，只有你的想象力。
