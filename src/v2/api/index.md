---
type: api
---

## 全局配置 (Global Config)

`Vue.config` is an object containing Vue's global configurations. You can modify its properties listed below before bootstrapping your application:
`Vue.config` 是一个对象，包含 Vue 的全局配置。可以在启动应用之前修改以下属性：

### silent

- **类型：** `boolean`

- **默认值：** `false`

- **用法：**

  ``` js
  Vue.config.silent = true
  ```

  Suppress all Vue logs and warnings.
  禁用 Vue.js 的所有日志和警告。

### optionMergeStrategies

- **类型：** `{ [key: string]: Function }`

- **默认值：** `{}`

- **用法：**

  ``` js
  Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {
    return child + 1
  }

  const Profile = Vue.extend({
    _my_option: 1
  })

  // Profile.options._my_option = 2
  ```

  Define custom merging strategies for options.
  为选项定义自定义的合并策略。

  The merge strategy receives the value of that option defined on the parent and child instances as the first and second arguments, respectively. The context Vue instance is passed as the third argument.
  合并策略会接受在父实例和子实例中定义的选项值，以它们来分别作为第一和第二个参数。Vue 实例所在的上下文会作为第三个参数传递。

- **另见**：[Custom Option Merging Strategies](../guide/mixins.html#Custom-Option-Merge-Strategies)

### devtools

- **类型：** `boolean`

- **默认值：** `true` (`false` in production builds)

- **用法：**

  ``` js
  // make sure to set this synchronously immediately after loading Vue
  // 要保证在加载 Vue 之后同步地立刻设置
  Vue.config.devtools = true
  ```

  Configure whether to allow [vue-devtools](https://github.com/vuejs/vue-devtools) inspection. This option's default value is `true` in development builds and `false` in production builds. You can set it to `true` to enable inspection for production builds.
  配置是否允许 [vue-devtools](https://github.com/vuejs/vue-devtools) 进行检查。开发版默认为 `true`，生产版默认为 `false`。你可以在生产版中将它设为 `true` 来启用检查。

### errorHandler

- **类型：** `Function`

- **默认值：** Error is thrown in place

- **用法：**

  ``` js
  Vue.config.errorHandler = function (err, vm) {
    // handle error
    // 处理错误
  }
  ```

  Assign a handler for uncaught errors during component render and watchers. The handler gets called with the error and the Vue instance.
  为渲染过程和监视器中的未捕捉错误，分配一个处理器。这个处理器被调用的时候会得到错误和 Vue 实例。

  > [Sentry](https://sentry.io), an error tracking service, provides [official integration](https://sentry.io/for/vue/) using this option.

### ignoredElements

- **Type:** `Array<string>`

- **Default:** `[]`

- **Usage:**

  ``` js
  Vue.config.ignoredElements = [
    'my-custom-web-component', 'another-web-component'
  ]
  ```

  Make Vue ignore custom elements defined outside of Vue (e.g., using the Web Components APIs). Otherwise, it will throw a warning about an `Unknown custom element`, assuming that you forgot to register a global component or misspelled a component name.

### keyCodes

- **类型：** `{ [key: string]: number }`

- **默认值：** `{}`

- **用法：**

  ``` js
  Vue.config.keyCodes = {
    v: 86,
    f1: 112,
    mediaPlayPause: 179
  }
  ```

  Define custom key alias(es) for v-on.
  为 v-on 定义自定义的按键别名。

## 全局 API (Global API)

<h3 id="Vue-extend">Vue.extend( options )</h3>

- **参数：**
  - `{Object} options`

- **用法：**

  Create a "subclass" of the base Vue constructor. The argument should be an object containing component options.
  创建基础 Vue 构造器的“子类”。参数为包含组件选项的对象。

  The special case to note here is the `data` option - it must be a function when used with `Vue.extend()`.
  这里要注意的特例是，在使用 `Vue.extend()` 时，`data` 选项必须是一个函数。

  ``` html
  <div id="mount-point"></div>
  ```

  ``` js
  // create constructor
  // 创建的构造函数
  var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
      return {
        firstName: 'Walter',
        lastName: 'White',
        alias: 'Heisenberg'
      }
    }
  })
  // create an instance of Profile and mount it on an element
  // 创建一个 Profile 实例然后将它挂载到一个元素上
  new Profile().$mount('#mount-point')
  ```

  Will result in:
  结果：

  ``` html
  <p>Walter White aka Heisenberg</p>
  ```

- **另见：** [组件](../guide/components.html)

<h3 id="Vue-nextTick">Vue.nextTick( callback, [context] )</h3>

- **参数：**
  - `{Function} callback`
  - `{Object} [context]`

- **用法：**

  Defer the callback to be executed after the next DOM update cycle. Use it immediately after you've changed some data to wait for the DOM update.
  将回调推迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用这个方法，等待 DOM 更新。

  ``` js
  // modify data
  // 修改数据
  vm.msg = 'Hello'
  // DOM not updated yet
  // DOM 尚未更新
  Vue.nextTick(function () {
    // DOM updated
    // DOM 已更新
  })
  ```

- **See also:** [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)
- **另见：** [异步更新队列](../guide/reactivity.html#异步更新队列)

<h3 id="Vue-set">Vue.set( object, key, value )</h3>

- **参数：**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Returns:** the set value.
- **返回值：** 设置的值.

- **用法：**

  Set a property on an object. If the object is reactive, ensure the property is created as a reactive property and trigger view updates. This is primarily used to get around the limitation that Vue cannot detect property additions.
  设置对象的属性。如果对象是响应式的，保证属性是响应式属性并触发视图更新。这个方法主要用于解决 Vue 无法检测到属性添加的局限性。

  **Note the object cannot be a Vue instance, or the root data object of a Vue instance.**
  **注意，这个对象不可以是 Vue 的实例或者 Vue 实例的根对象。**

- **另见：** [深入响应式原理](../guide/reactivity.html)

<h3 id="Vue-delete">Vue.delete( object, key )</h3>

- **参数：**
  - `{Object} object`
  - `{string} key`

- **用法：**

  Delete a property on an object. If the object is reactive, ensure the deletion triggers view updates. This is primarily used to get around the limitation that Vue cannot detect property deletions, but you should rarely need to use it.
  删除对象的属性。如果对象是响应式的，保证属性是响应式属性并触发视图更新。这个方法主要用于解决 Vue 无法检测到属性删除的局限性，但是你应该很少会用到它。

  **Note the object cannot be a Vue instance, or the root data object of a Vue instance.**
  **注意，这个对象不可以是 Vue 的实例或者 Vue 实例的根对象。**

- **另见：** [深入响应式原理](../guide/reactivity.html)

<h3 id="Vue-directive">Vue.directive( id, [definition] )</h3>

- **参数：**
  - `{string} id`
  - `{Function | Object} [definition]`

- **用法：**

  Register or retrieve a global directive.
  注册或获取全局指令。

  ``` js
  // register
  // 注册
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })

  // register (simple function directive)
  // 注册 （简单函数指令）
  Vue.directive('my-directive', function () {
    // this will be called as `bind` and `update`
    // 将被作为 `bind` 和 `update` 调用
  })

  // getter, return the directive definition if registered
  // getter, 返回已注册的指令
  var myDirective = Vue.directive('my-directive')
  ```

- **另见：** [自定义指令](../guide/custom-directive.html)

<h3 id="Vue-filter">Vue.filter( id, [definition] )</h3>

- **参数：**
  - `{string} id`
  - `{Function} [definition]`

- **用法：**

  Register or retrieve a global filter.
  注册或获取全局过滤器。

  ``` js
  // register
  // 注册
  Vue.filter('my-filter', function (value) {
    // return processed value
    // 返回处理后的值
  })

  // getter, return the filter if registered
  // getter, 返回已注册的过滤器
  var myFilter = Vue.filter('my-filter')
  ```

<h3 id="Vue-component">Vue.component( id, [definition] )</h3>

- **参数：**
  - `{string} id`
  - `{Function | Object} [definition]`

- **用法：**

  Register or retrieve a global component. Registration also automatically sets the component's `name` with the given `id`.
  注册或获取全局组件。注册组件还会自动将组件的 `name` 设置成传递进来的 `id`。

  ``` js
  // register an extended constructor
  // 注册组件，传入一个扩展的构造器
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // register an options object (automatically call Vue.extend)
  // 注册组件，传入一个选项对象（自动调用 Vue.extend）
  Vue.component('my-component', { /* ... */ })

  // retrieve a registered component (always return constructor)
  // 获取注册的组件（始终返回构造器）
  var MyComponent = Vue.component('my-component')
  ```

- **另见：** [组件](../guide/components.html)

<h3 id="Vue-use">Vue.use( plugin )</h3>

- **参数：**
  - `{Object | Function} plugin`

- **用法：**

  Install a Vue.js plugin. If the plugin is an Object, it must expose an `install` method. If it is a function itself, it will be treated as the install method. The install method will be called with Vue as the argument.
  安装 Vue.js 插件。如果传入的插件是一个对象，则必须暴露一个 `install` 方法。如果是一个函数，它会被直接作为安装方法。该安装方法会已 Vue 为参数被调用。

  When this method is called on the same plugin multiple times, the plugin will be installed only once.
  即使安装方法在相同的插件上调用多次，该插件也只会被安装一次。

- **另见：** [插件](../guide/plugins.html)

<h3 id="Vue-mixin">Vue.mixin( mixin )</h3>

- **参数：**
  - `{Object} mixin`

- **用法：**

  Apply a mixin globally, which affects every Vue instance created afterwards. This can be used by plugin authors to inject custom behavior into components. **Not recommended in application code**.
  全局应用一个混合，将影响所有 Vue 实例。插件作者可以用它向组件注入自定义逻辑。**不推荐在应用代码中使用。**

- **See also:** [Global Mixins](../guide/mixins.html#Global-Mixin)
- **另见：** [全局混合](../guide/mixins.html#Global-Mixin)

<h3 id="Vue-compile">Vue.compile( template )</h3>

- **参数：**
  - `{string} template`

- **用法：**

  Compiles a template string into a render function. **Only available in the standalone build.**
  将模板字符串编译成一个渲染函数。**仅在独立构建时有效。**

  ``` js
  var res = Vue.compile('<div><span>{{ msg }}</span></div>')

  new Vue({
    data: {
      msg: 'hello'
    },
    render: res.render,
    staticRenderFns: res.staticRenderFns
  })
  ```

- **另见：** [Render Functions](../guide/render-function.html)

## 选项 / 数据 (Options / Data)

### data

- **类型:** `Object | Function`

- **Restriction:** Only accepts `Function` when used in a component definition.
- **限制:** 在组件定义中只能是函数

- **Details:**
- **详细:**

  The data object for the Vue instance. Vue will recursively convert its properties into getter/setters to make it "reactive". **The object must be plain**: native objects such as browser API objects and prototype properties are ignored. A rule of thumb is that data should just be data - it is not recommended to observe objects with its own stateful behavior.
  Vue 实例的数据对象。 Vue 会递归地将它全部属性转为 getter/setter，从而让它能响应数据变化。**这个对象必须是普通对象**: 例如浏览器API对象这类原生对象的原型属性会被忽略。谨记一条规则，数据应该只是单纯的数据，而不推荐观察有状态的对象。

  Once observed, you can no longer add reactive properties to the root data object. It is therefore recommended to declare all root-level reactive properties upfront, before creating the instance.
  一旦开始监听数据变化, 你再也不能在根数据对象上添加响应属性。 因此建议在创建实例之前首先声明所有根响应属性。

  After the instance is created, the original data object can be accessed as `vm.$data`. The Vue instance also proxies all the properties found on the data object, so `vm.a` will be equivalent to `vm.$data.a`.
  在实例创建之后, 可以用 `vm.$data` 访问原始数据对象。 Vue 实例也代理了数据对象的所有属性，所以 `vm.a` 等同于 `vm.$data.a`.

  Properties that start with `_` or `$` will **not** be proxied on the Vue instance because they may conflict with Vue's internal properties and API methods. You will have to access them as `vm.$data._property`.
  名字以 `_` 或 `$` 开始的属性**不会**被 Vue 实例代理，因为它们可能与 Vue 的内置属性与 API 方法冲突。可以用 `vm.$data._property` 访问它们。

  When defining a **component**, `data` must be declared as a function that returns the initial data object, because there will be many instances created using the same definition. If we still use a plain object for `data`, that same object will be **shared by reference** across all instances created! By providing a `data` function, every time a new instance is created, we can simply call it to return a fresh copy of the initial data.
  在定义**组件**时，由于同一定义将创建多个实例，所以 `data` 必须是一个函数，返回原始数据对象。如果我们仍然使用一个普通对象作为 `data` ，则创建的所有实例将指向同一个对象！但是换成函数后，每当创建一个实例时，会调用这个函数，返回一个新的原始数据对象的副本，实例指向这个副本对象。

  If required, a deep clone of the original object can be obtained by passing `vm.$data` through `JSON.parse(JSON.stringify(...))`.
  如果有需要，可以通过将 `vm.$data` 传入 `JSON.parse(JSON.stringify(...))` 得到原始数据对象。

- **示例:**

  ``` js
  var data = { a: 1 }

  // direct instance creation
  // 直接创建一个实例
  var vm = new Vue({
    data: data
  })
  vm.a // -> 1
  vm.$data === data // -> true

  // must use function when in Vue.extend()
  // 在 Vue.extend() 中 data 必须是函数
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

  <p class="tip">Note that __you should not use an arrow function with the `data` property__ (e.g. `data: () => { return { a: this.myProp }}`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.myProp` will be undefined.<br>注意，__你不应该用箭头函数给 `data` 属性赋值__（比如 `data: () => { return { a: this.myProp }}`）。因为箭头函数会绑定父上下文，所以 `this` 并不是你所期待的 Vue 实例，而 `this.myProp` 的值会是 undefined。</p>

- **另见:** [深入响应式原理](../guide/reactivity.html)

### props

- **类型:** `Array<string> | Object`

- **Details:**
- **详细:**

  A list/hash of attributes that are exposed to accept data from the parent component. It has a simple Array-based syntax and an alternative Object-based syntax that allows advanced configurations such as type checking, custom validation and default values.
  用来接收父组件数据的 list/hash 属性。可以是数组或对象，对象用于高级配置，如类型检查，自定义验证，默认值等。

- **示例:**

  ``` js
  // simple syntax
  // 简单语法
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })

  // object syntax with validation
  // 对象语法，指定验证要求
  Vue.component('props-demo-advanced', {
    props: {
      // just type check
      // 只检测类型
      height: Number,
      // type check plus other validations
      // 类型检测 + 其他验证
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
    }
  })
  ```

- **另见:** [Props](../guide/components.html#Props)

### propsData

- **类型:** `{ [key: string]: any }`

- **Restriction:** only respected in instance creation via `new`.
- **限制:** only respected in instance creation via `new`.

- **Details:**

  Pass props to an instance during its creation. This is primarily intended to make unit testing easier.
  在创建实例的过程传递 props。主要作用是方便测试。

- **示例:**

  ``` js
  var Comp = Vue.extend({
    props: ['msg'],
    template: '<div>{{ msg }}</div>'
  })

  var vm = new Comp({
    propsData: {
      msg: 'hello'
    }
  })
  ```

### computed

- **类型:** `{ [key: string]: Function | { get: Function, set: Function } }`

- **Details:**

  Computed properties to be mixed into the Vue instance. All getters and setters have their `this` context automatically bound to the Vue instance.
  实例的计算属性。getter 和 setter 的 `this` 自动地绑定到实例。

  <p class="tip">Note that __you should not use an arrow function to define a computed property__ (e.g. `aDouble: () => this.a * 2`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be undefined.<br>注意，__你不应该用箭头函数来定义一个计算属性__（比如 `aDouble: () => this.a * 2`）。因为箭头函数会绑定父上下文，所以 `this` 并不是你所期待的 Vue 实例，而 `this.a` 的值会是 undefined。</p>

  Computed properties are cached, and only re-computed on reactive dependency changes.
  计算属性会被缓存，只有在响应依赖改变的时候才会重计算。

- **Example:**
- **示例：**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // get only, just need a function
      // 仅读取，值只须为函数
      aDouble: function () {
        return this.a * 2
      },
      // both get and set
      // 读取和设置
      aPlus: {
        get: function () {
          return this.a + 1
        },
        set: function (v) {
          this.a = v - 1
        }
      }
    }
  })
  vm.aPlus   // -> 2
  vm.aPlus = 3
  vm.a       // -> 2
  vm.aDouble // -> 4
  ```

- **另见:**
  - [Computed Properties](../guide/computed.html)
  - [计算属性](../guide/computed.html)

### methods

- **类型:** `{ [key: string]: Function }`

- **Details:**
- **详细:**

  Methods to be mixed into the Vue instance. You can access these methods directly on the VM instance, or use them in directive expressions. All methods will have their `this` context automatically bound to the Vue instance.
  实例方法。实例可以直接访问这些方法，也可以用在指令表达式内。方法的 `this` 自动绑定到实例。

  <p class="tip">Note that __you should not use an arrow function to define a method__ (e.g. `plus: () => this.a++`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.a` will be undefined.<br>注意，__你不应该用箭头函数来定义一个方法__（比如 `plus: () => this.a++`)。因为箭头函数会绑定父上下文，所以 `this` 并不是你所期待的 Vue 实例，而 `this.a` 的值会是 undefined。</p>

- **示例：**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function () {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

- **See also:** [Methods and Event Handling](../guide/events.html)
- **另见:** [方法与事件处理器](../guide/events.html)

### watch

- **类型:** `{ [key: string]: string | Function | Object }`

- **Details:**
- **详细:**

  An object where keys are expressions to watch and values are the corresponding callbacks. The value can also be a string of a method name, or an Object that contains additional options. The Vue instance will call `$watch()` for each entry in the object at instantiation.
  一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是包含其他选项的对象。在实例化时为每个键调用 `$watch()` 。

- **Example:**
- **示例：**

  ``` js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3
    },
    watch: {
      a: function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // string method name
      // 方法名
      'b': 'someMethod',
      // deep watcher
      // 深度 watcher
      'c': {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      }
    }
  })
  vm.a = 2 // -> new: 2, old: 1
  ```

  <p class="tip">Note that __you should not use an arrow function to define a watcher__ (e.g. `searchQuery: newValue => this.updateAutocomplete(newValue)`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.updateAutocomplete` will be undefined.<br>注意，__你不应该用箭头函数来定义监视器__（比如 `searchQuery: newValue => this.updateAutocomplete(newValue)`）。因为尖头函数会绑定父上下文，所以 `this` 并不是你所期待的 Vue 实例，而 `this.updateAutocomplete` 的值会是 undefined。</p>

- **另见:** [实例方法 - vm.$watch](#vm-watch)

## 选项 / DOM (Options / DOM)

### el

- **类型：** `string | HTMLElement`

- **Restriction:** only respected in instance creation via `new`.
- **限制：** 只有在通过 `new` 新建实例的时候才使用

- **Details：**
- **详细：**

  Provide the Vue instance an existing DOM element to mount on. It can be a CSS selector string or an actual HTMLElement.
  为实例提供挂载元素。值可以是 CSS 选择器，或实际 HTML 元素。

  After the instance is mounted, the resolved element will be accessible as `vm.$el`.
  在实例挂载之后，可以通过 `vm.$el` 访问关联元素。

  If this option is available at instantiation, the instance will immediately enter compilation; otherwise, the user will have to explicitly call `vm.$mount()` to manually start the compilation.
  如果在初始化时指定了这个选项，实例将立即进入编译过程; 否则，需要调用 `vm.$mount()` 手动开始编译。

  <p class="tip">
  The provided element merely serves as a mounting point. Unlike in Vue 1.x, the mounted element will be replaced with Vue-generated DOM in all cases. It is therefore not recommended to mount the root instance to `<html>` or `<body>`.
  提供的元素仅仅作为一个挂载点。 不同于 Vue 1.x， Vue 2.x 在任何情况下，被挂载的元素会被 Vue 生成的 DOM 替换掉。因此不建议把根实例挂载在 `<html>` 或 `<body>`。</p>

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见：** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### template

- **类型：** `string`

- **Details:**
- **详细：**

  A string template to be used as the markup for the Vue instance. The template will **replace** the mounted element. Any existing markup inside the mounted element will be ignored, unless content distribution slots are present in the template.
  Vue 实例的字符串模版。这个模版会**替换**被挂载的元素。除非模版中有分发内容的 slot，否则被挂载元素的内容都将全部被忽略。

  If the string starts with `#` it will be used as a querySelector and use the selected element's innerHTML as the template string. This allows the use of the common `<script type="x-template">` trick to include templates.
  如果这个字符串以 `#` 开头，那么它会被当作选择器来使用，然后用选中的元素的 innerHTML 作为模版。 常用的一个技巧是用 `<script type="x-template">` 来包含模版。

  <p class="tip">From a security perspective, you should only use Vue templates that you can trust. Never use user-generated content as your template.
  从安全角度出发，你应该只使用你能信赖的 Vue 模版。永远不用使用用户生成的内容作为你的模版。</p>

- **另见：**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [生命周期图示](../guide/instance.html#Lifecycle-Diagram)
  - [Content Distribution](../guide/components.html#Content-Distribution-with-Slots)
  - [内容分发](../guide/components.html#Content-Distribution-with-Slots)

### render

  - **类型：** `Function`

  - **Details:**
  - **详细：**

    An alternative to string templates allowing you to leverage the full programmatic power of JavaScript. The render function receives a `createElement` method as it's first argument used to create `VNode`s.
    字符串模版的替代品，可以让你充分发挥 JavaScript 的编程能力。render 函数接收一个 `createElement` 方法作为它的第一个参数来创建 `VNode`。

    If the component is a functional component, the render function also receives an extra argument `context`, which provides access to contextual data since functional components are instance-less.
    如果组件是一个函数式组件, render 函数也接收一个额外的参数 `context`，它会为缺少实例的函数式组件提供上下文数据。

  - **另见：**
    - [Render Functions](../guide/render-function)
    - [渲染函数](../guide/render-function)

## 选项 / 生命周期钩子 (Options / Lifecycle Hooks)

All lifecycle hooks automatically have their `this` context bound to the instance, so that you can access data, computed properties, and methods. This means __you should not use an arrow function to define a lifecycle method__ (e.g. `created: () => this.fetchTodos()`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.fetchTodos` will be undefined.
所有的生命周期钩子都会将它们的 `this` 自动绑定到实例，所以你可以访问通过 `this` 来访问数据，计算属性和方法。这意味着 __你不应该用箭头函数给 `data` 属性赋值__（比如 `created: () => this.fetchTodos()`）。因为箭头函数会绑定父上下文，所以 `this` 并不是你所期待的 Vue 实例，而 `this.fetchTodos` 的值会是 undefined。

### beforeCreate

- **类型:** `Function`

- **Details:**
- **详细:**

  Called synchronously after the instance has just been initialized, before data observation and event/watcher setup.
  在实例初始化之前同步调用。此时数据观察还没开始，事件和监听器也还没设置。

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### created

- **类型:** `Function`

- **Details:**
- **详细:**

  Called synchronously after the instance is created. At this stage, the instance has finished processing the options which means the following have been set up: data observation, computed properties, methods, watch/event callbacks. However, the mounting phase has not been started, and the `$el` property will not be available yet.
  在实例创建之后同步调用。此时实例已经结束解析选项，这意味着已建立：数据绑定，计算属性，方法，监听器/事件回调。但是还没有开始挂载，所以 `$el` 还不存在。

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### beforeMount

- **类型:** `Function`

- **Details:**
- **详细:**

  Called right before the mounting begins: the `render` function is about to be called for the first time.
  在挂载前调用： `render` 函数第一次被调用。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### mounted

- **类型:** `Function`

- **Details:**
- **详细:**

  Called after the instance has just been mounted where `el` is replaced by the newly created `vm.$el`. If the root instance is mounted to an in-document element, `vm.$el` will also be in-document when `mounted` is called.
  在挂载完成， `el` 被新建的 `vm.$el` 替换之后调用。如果根实例是挂载在文档中的元素，那么当 `mounted` 被调用的时候 `vm.$el` 也会在文档中

  > 译者注: 因为 Vue 2.0 使用了虚拟 Dom，所以挂载之后不一定在文档中(in-document)，也有可能是在上下文中(contextual)。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### beforeUpdate

- **类型:** `Function`

- **Details:**
- **详细:**

  Called when the data changes, before the virtual DOM is re-rendered and patched.
  当数据改变，在虚拟 Dom 重新渲染和打上补丁之前调用。

  You can perform further state changes in this hook and they will not trigger additional re-renders.
  在这个钩子调用的时候，你可以执行进一步状态改变，这些改变不会触发其它额外的重渲染。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### updated

- **类型:** `Function`

- **Details:**
- **详细:**

  Called after a data change causes the virtual DOM to be re-rendered and patched.
  在数据改变，虚拟 Dom 重新渲染和打补丁之后调用。

  The component's DOM will be in updated state when this hook is called, so you can perform DOM-dependent operations in this hook. However, in most cases you should avoid changing state in this hook, because it may lead to an infinite update loop.
  当钩子被调用的时候，组件的 DOM 将会处于更新状态, 因此你可以在这钩子中执行 DOM 相关操作. 但是，在绝大多数情况下，你应该避免在这钩子中改变状态，因为这有可能会导致死循环。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### activated

- **类型:** `Function`

- **Details:**
- **详细:**

  Called when a kept-alive component is activated.
  当一个 kept-alive 组件被激活的时候调用

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **另见:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### deactivated

- **类型:** `Function`

- **Details:**
- **详细:**

  Called when a kept-alive component is deactivated.
  当一个 kept-alive 组件被移除的时候调用

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **另见:**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### beforeDestroy

- **类型:** `Function`

- **Details:**
- **详细:**

  Called right before a Vue instance is destroyed. At this stage the instance is still fully functional.
  在 Vue 实例销毁前调用。在这个阶段实例仍然是完全功能的。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

### destroyed

- **类型:** `Function`

- **Details:**
- **详细:**

  Called after a Vue instance has been destroyed. When this hook is called, all directives of the Vue instance have been unbound, all event listeners have been removed, and all child Vue instances have also been destroyed.
  在实例被销毁后调用。当钩子被调用的时候，解绑所有指令，移除所有事件监听器，销毁所有的子实例。

  **This hook is not called during server-side rendering.**
  **这钩子在服务端渲染的时候不会被调用**

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

## 选项 / 资源 (Options / Assets)

### directives

- **类型：** `Object`

- **Details:**
- **详细：**

  A hash of directives to be made available to the Vue instance.
  一个哈希表，包含了对 Vue 实例可见的指令。

- **另见：**
  - [Custom Directives](../guide/custom-directive.html)
  - [自定义指令](../guide/custom-directive.html)
  - [Assets Naming Convention](../guide/components.html#Assets-Naming-Convention)
  - [资源命名约定](../guide/components.html#Assets-Naming-Convention)

### filters

- **类型：** `Object`

- **Details:**
- **详细：**

  A hash of filters to be made available to the Vue instance.
  一个哈希表，包含了对 Vue 实例可见的过滤器。

- **另见：**
  - [`Vue.filter`](#Vue-filter)
  - [`Vue.filter`](#Vue-filter)


### components

- **类型：** `Object`

- **Details:**
- **详细：**

  A hash of components to be made available to the Vue instance.
  一个哈希表，包含了对 Vue 实例可见的组件。

- **另见：**
  - [Components](../guide/components.html)
  - [组件](../guide/components.html)

## 选项 / 杂项 (Options / Misc)

### parent

- **类型：** `Vue 实例`

- **Details:**
- **详细：**

  Specify the parent instance for the instance to be created. Establishes a parent-child relationship between the two. The parent will be accessible as `this.$parent` for the child, and the child will be pushed into the parent's `$children` array.
  指定实例的父实例，在两者之间建立父子关系。子实例可以用 `this.$parent` 访问父实例，子实例被推入父实例的 `$children` 数组中。

  <p class="tip">
  Use `$parent` and `$children` sparringly - they mostly serve as an escape-hatch. Prefer using props and events for parent-child communication.
  尽可能不去使用 `$parent` 和 `$children`。它们更多地是提供另外一种方法来实现父子间通信而已。在父子间通信中更**推荐**使用 props 和 events。
  </p>

### mixins

- **类型：** `Array<Object>`

- **Details:**
- **详细：**

  The `mixins` option accepts an array of mixin objects. These mixin objects can contain instance options just like normal instance objects, and they will be merged against the eventual options using the same option merging logic in `Vue.extend()`. e.g. If your mixin contains a created hook and the component itself also has one, both functions will be called.
  一个包含混合对象（mixin objects）的数组。这些混合对象可以像普通实例对象一样包含实例选项，它们将合并成一个最终选项对象，合并策略同 `Vue.extend()`。比如，如果混合对象包含一个 `created` 钩子，组件自身也包含一个，两个钩子函数都会被调用。

  Mixin hooks are called in the order they are provided, and called before the component's own hooks.
  混合对象的钩子会按它们的出现顺序被调用，但是在组件自己的钩子之前被调用。

- **示例：**

  ``` js
  var mixin = {
    created: function () { console.log(1) }
  }
  var vm = new Vue({
    created: function () { console.log(2) },
    mixins: [mixin]
  })
  // -> 1
  // -> 2
  ```

- **另见：** [Mixins](../guide/mixins.html)

### name

- **类型：** `string`

- **Restriction:** only respected when used as a component option.
- **限制：** 只在组件选项中使用。

- **Details:**
- **详细：**

  Allow the component to recursively invoke itself in its template. Note that when a component is registered globally with `Vue.component()`, the global ID is automatically set as its name.
  允许组件在它的模板内递归地调用它自己。注意如果组件是由 `Vue.component()` 全局注册，全局 ID 自动作为它的名字。


  Another benefit of specifying a `name` option is debugging. Named components result in more helpful warning messages. Also, when inspecting an app in the [vue-devtools](https://github.com/vuejs/vue-devtools), unnamed components will show up as `<AnonymousComponent>`, which isn't very informative. By providing the `name` option, you will get a much more informative component tree.
  指定 `name` 选项的另一个好处是方便检查。 有命名的组件可以打印出更多有用的信息。同时，当使用 [vue-devtools](https://github.com/vuejs/vue-devtools) 调试的时候, 未命名的组件只会显示 `<AnonymousComponent>` 这对于调试来说没任何意义。传入 `name` 选项后， 你可以得到一个更有用的组件树结构，同时可以知道正在检查哪个组件。

### extends

- **类型：** `Object | Function`

- **Details:**
- **详细：**

  Allows declaratively extending another component (could be either a plain options object or a constructor) without having to use `Vue.extend`. This is primarily intended to make it easier to extend between single file components.
  使用声明的方式来扩展另一个组件（可以是选项对象或者构造器），而不必使用 `Vue.extend`。主要作用是更容易的扩展单文件组件。

  This is similar to `mixins`, the difference being that the component's own options takes higher priority than the source component being extended.
  这类似于 `mixins`，不同的是组件的选项比待扩展的源组件的选项优先。

- **示例：**

  ``` js
  var CompA = { ... }

  // extend CompA without having to call Vue.extend on either
  // 扩展 CompA，不用调用 Vue.extend
  var CompB = {
    extends: CompA,
    ...
  }
  ```

### delimiters

- **类型：** `Array<string>`

- **默认值：** `{% raw %}["{{", "}}"]{% endraw %}`

- **Details:**
- **详细：**

  Change the plain text interpolation delimiters. **This option is only available in the standalone build.**
  修改文本插值的定界符。 **这选项只在独立编译版本有效**

- **示例：**

  ``` js
  new Vue({
    delimiters: ['${', '}']
  })

  // Delimiters changed to ES6 template string style
  // 定界符改为ES6 模板字符串形式
  ```

### functional

- **类型：** `boolean`

- **Details:**
- **详细：**

  Causes a component to be stateless (no `data`) and instanceless (no `this` context). They are simply a `render` function that returns virtual nodes making them much cheaper to render.
  使组件变成一个无状态（没有 `data`），无实例（没有 `this` 上下文）的函数化组件 这导致使用 `render` 函数把它们渲染成虚拟节点的时候变得更加高效。

- **See also:** [Functional Components](../guide/render-function.html#Functional-Components)
- **另见：** [函数化组件](../guide/render-function.html#Functional-Components)

##  实例属性 (Instance Properties)

### vm.$data

- **类型：** `Object`

- **Details:**
- **详细：**

  The data object that the Vue instance is observing. The Vue instance proxies access to the properties on its data object.
  Vue 实例绑定的数据对象。Vue 实例代理了这个数据对象的属性的访问。

- **另见：** [Options - data](#data)

### vm.$el

- **类型：** `HTMLElement`

- **只读**

- **Details:**
- **详细：**

  The root DOM element that the Vue instance is managing.
  Vue 实例所管理管理的根 DOM 元素。

### vm.$options

- **类型：** `Object`

- **只读**

- **Details:**
- **详细：**

  The instantiation options used for the current Vue instance. This is useful when you want to include custom properties in the options:
  当前实例的初始化选项。当你想在选项中包含自定义属性时，这很有用：

  ``` js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // -> 'foo'
    }
  })
  ```

### vm.$parent

- **类型：** `Vue 实例`

- **只读**

- **Details:**
- **详细：**

  The parent instance, if the current instance has one.
  如果当前 Vue 实例有父实例的话，这个代表它的父实例。

### vm.$root

- **Type:** `Vue instance`
- **类型：** `Vue 实例`

- **只读**

- **Details:**
- **详细：**

  The root Vue instance of the current component tree. If the current instance has no parents this value will be itself.
  当前组件树的根 Vue 实例。如果当前实例没有父实例，那么这个值就是当前 Vue 实例本身。

### vm.$children

- **类型：** `Array<Vue instance>`

- **只读**

- **Details:**
- **详情：**

  The direct child components of the current instance. **Note there's no order guarantee for `$children`, and it is not reactive.** If you find yourself trying to use `$children` for data binding, consider using an Array and `v-for` to generate child components, and use the Array as the source of truth.
  当前实例的直接子组件。**注意，这里不保证 `$children` 是有序的, 而且它不是反应式的。**如果你想要尝试使用 `$children` 来进行数据绑定，可以考虑使用一个数组和`v-for`来生成子组件，然后使用这个数组来作为数据来源。

### vm.$slots

- **类型：** `Object`

- **只读**

- **Details:**
- **详情：**

  Used to access content [distributed by slots](../guide/components.html#Content-Distribution-with-Slots). Each [named slot](../guide/components.html#Named-Slots) has its own corresponding property (e.g. the contents of `slot="foo"` will be found at `vm.$slots.foo`). The `default` property contains any nodes not included in a named slot.
  用来访问 [用 slot 分发](/guide/components.html#Content-Distribution-with-Slots) 的内容。每一个 [命名 slot](../guide/components.html#Named-Slots) 都有对应的属性（比如 `slot="foo"` 的内容就是在 `vm.$slots.foo`）。`default` 属性包含了一个命名 slot 中没有的节点。

  Accessing `vm.$slots` is most useful when writing a component with a [render function](../guide/render-function.html).
  当你在用 [render 函数](../guide/render-function.html) 来编写一个组件时，访问 `vm.$slots` 时最为有用的。

- **示例：**：

  ```html
  <blog-post>
    <h1 slot="header">
      About Me
    </h1>

    <p>Here's some page content, which will be included in vm.$slots.default, because it's not inside a named slot.</p>

    <p slot="footer">
      Copyright 2016 Evan You
    </p>

    <p>If I have some content down here, it will also be included in vm.$slots.default.</p>.
  </blog-post>
  ```

  ```js
  Vue.component('blog-post', {
    render: function (createElement) {
      var header = this.$slots.header
      var body   = this.$slots.default
      var footer = this.$slots.footer
      return createElement('div', [
        createElement('header', header)
        createElement('main', body)
        createElement('footer', footer)
      ])
    }
  })
  ```

- **另见：**
  - [`<slot>` Component](#slot)
  - [`<slot>` 组件](#slot)
  - [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)
  - [使用 slot 分发内容](../guide/components.html#Content-Distribution-with-Slots)
  - [Render Functions](../guide/render-function.html)
  - [Render 函数](../guide/render-function.html)

### vm.$refs

- **类型：** `Object`

- **只读**

- **Details:**
- **详细：**

  An object that holds child components that have `ref` registered.
  一个对象，包含了注册过 `ref` 的子组件。

- **另见：**
  - [Child Component Refs](../guide/components.html#Child-Component-Refs)
  - [子组件索引](../guide/components.html#Child-Component-Refs)
  - [ref](#ref)

### vm.$isServer

- **类型：** `boolean`

- **只读**

- **Details:**
- **详细：**

  Whether the current Vue instance is running on the server.
  当前 Vue 实例是否运行在服务器端。

- **See also:** [Server-Side Rendering](../guide/ssr.html)
- **另见：** [服务器端渲染](../guide/ssr.html)

## 实例方法 ／ 数据 (Instance Methods / Data)

<h3 id="vm-watch">vm.$watch( expOrFn, callback, [options] )</h3>

- **参数：**
  - `{string | Function} expOrFn`
  - `{Function} callback`
  - `{Object} [options]`
  - `{boolean} deep`
  - `{boolean} immediate`

- **Returns:** `{Function} unwatch`
- **返回值：** `{Function} unwatch`

- **用法：**

  Watch an expression or a computed function on the Vue instance for changes. The callback gets called with the new value and the old value. The expression only accepts simple dot-delimited paths. For more complex expression, use a function instead.
  观察 Vue 实例变化的一个表达式或计算函数。回调的参数为新值和旧值。表达式只可以是用 `.` 分隔的路径。如果要使用更复杂的表达式，你应该使用一个函数。

<p class="tip">Note: when mutating (rather than replacing) an Object or an Array, the old value will be the same as new value because they reference the same Object/Array. Vue doesn't keep a copy of the pre-mutate value.
注意：在修改（不是替换）对象或数组时，旧值将与新值相同，因为他们索引同一个对象／数组。Vue 不会保留修改之前值的副本。</p>


- **示例：**

  ``` js
  // keypath
  // 键路径
  vm.$watch('a.b.c', function (newVal, oldVal) {
    // do something
    // 做点什么
  })

  // function
  // 函数
  vm.$watch(
    function () {
      return this.a + this.b
    },
    function (newVal, oldVal) {
      // do something
      // 做点什么
    }
  )
  ```

  `vm.$watch` returns an unwatch function that stops firing the callback:
  `vm.$watch` 返回一个取消观察函数，用来停止触发回调：

  ``` js
  var unwatch = vm.$watch('a', cb)
  // later, teardown the watcher
  // 之后取消观察
  unwatch()
  ```

- **Option: deep**

  To also detect nested value changes inside Objects, you need to pass in `deep: true` in the options argument. Note that you don't need to do so to listen for Array mutations.
  为了发现对象内部值的变化，可以在选项参数中指定 deep: true。注意监听数组的变动不需要这么做。

  ``` js
  vm.$watch('someObject', callback, {
    deep: true
  })
  vm.someObject.nestedValue = 123
  // callback is fired
  // 触发回调
  ```

- **Option: immediate**

  Passing in `immediate: true` in the option will trigger the callback immediately with the current value of the expression:
  在选项参数中指定 `immediate: true` 将立即以表达式的当前值触发回调：

  ``` js
  vm.$watch('a', callback, {
    immediate: true
  })
  // callback is fired immediately with current value of `a`
  // 立即以 `a` 的当前值触发回调
  ```

<h3 id="vm-set">vm.$set( object, key, value )</h3>

- **参数：**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Returns:** the set value.
- **返回值：**设置的值

- **用法**

  This is the **alias** of the global `Vue.set`.

  这是全局 `Vue.set` 的 **别名**

- **另见：** [Vue.set](#Vue-set)

<h3 id="vm-delete">vm.$delete( object, key )</h3>

- **参数：**
  - `{Object} object`
  - `{string} key`

- **用法：**

  This is the **alias** of the global `Vue.delete`.
  这是全局 `Vue.delete` 的 **别名**

- **另见：** [Vue.delete](#Vue-delete)

## 实例方法/事件 (Instance Methods / Events)

<h3 id="vm-on">vm.$on( event, callback )</h3>

- **参数：**
  - `{string} event`
  - `{Function} callback`

- **用法：**

  Listen for a custom event on the current vm. Events can be triggered by `vm.$emit`. The callback will receive all the additional arguments passed into these event-triggering methods.
  监听当前实例上的自定义事件。事件可以由 `vm.$emit`触发。这些触发事件的方法所接收到的额外参数，都会传递给回调 `callback`。

- **示例：**

  ``` js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // -> "hi"
  ```

<h3 id="vm-once">vm.$once( event, callback )</h3>

- **参数：**
  - `{string} event`
  - `{Function} callback`

- **用法：**

  Listen for a custom event, but only once. The listener will be removed once it triggers for the first time.
  监听自定义事件，但这是一次性的。当自定义事件被触发一次后，监听器就会被移除。

<h3 id="vm-off">vm.$off( [event, callback] )</h3>

- **参数：**
  - `{string} [event]`
  - `{Function} [callback]`

- **用法：**

  Remove event listener(s).
  删除事件监听器。

  - If no arguments are provided, remove all event listeners;
  - 如果没有参数，则删除所有的监听器;

  - If only the event is provided, remove all listeners for that event;
  - 如果只提供了事件，则删除这个事件所有的监听器;

  - If both event and callback are given, remove the listener for that specific callback only.
  - 如果同时提供了事件与回调，则只删除这个回调。

<h3 id="vm-emit">vm.$emit( event, [...args] )</h3>

- **参数:**
  - `{string} event`
  - `[...args]`

  Trigger an event on the current instance. Any additional arguments will be passed into the listener's callback function.
  在当前实例上触发事件。所有的额外参数都会传给监听器的回调函数。

## 实例方法/生命周期 (Instance Methods / Lifecycle)

<h3 id="vm-mount">vm.$mount( [elementOrSelector] )</h3>

- **参数：**
  - `{Element | string} [elementOrSelector]`
  - `{boolean} [hydrating]`

- **Returns:** `vm` - the instance itself
- **返回值：** `vm` —— 实例自身

- **用法：**

  If a Vue instance didn't receive the `el` option at instantiation, it will be in "unmounted" state, without an associated DOM element. `vm.$mount()` can be used to manually start the mounting of an unmounted Vue instance.
  如果 Vue 实例在实例化时没有接收 `el` 选项, 则它处于“未挂载”状态，没有关联的 DOM 元素。这时，可以使用 `vm.$mount()` 手动挂载。

  If `elementOrSelector` argument is not provided, the template will be rendered as an off-document element, and you will have to use native DOM API to insert it into the document yourself.
  如果没提供 `elementOrSelector` 参数，模板渲染出来的元素不会被插入到文档中。你将需要使用原生的 DOM API 来将它插入文档中。

  The method returns the instance itself so you can chain other instance methods after it.
  这个方法返回实例自身，因而可以链式调用其它实例方法。

- **示例：**

  ``` js
  var MyComponent = Vue.extend({
    template: '<div>Hello!</div>'
  })

  // create and mount to #app (will replace #app)
  // 创建一个 Vue 实例并将它挂载到 #app （会替换掉 #app 的内容）
  new MyComponent().$mount('#app')

  // the above is the same as:
  // 跟上面的代码一样的效果：
  new MyComponent({ el: '#app' })

  // or, render off-document and append afterwards:
  // 或者，先在文档外渲染，之后再将它插入文档：
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)
  ```

- **另见:**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Server-Side Rendering](../guide/ssr.html)

<h3 id="vm-forceUpdate">vm.$forceUpdate()</h3>

- **用法:**

  Force the Vue instance to re-render. Note it does not affect all child components, only the instance itself and child components with inserted slot content.
  强制 Vue 实例重新渲染。这个方法不会影响所有的子组件，它只会让实例自身，还有那些带有插槽内容的子组件重新渲染。

<h3 id="vm-nextTick">vm.$nextTick( callback )</h3>

- **参数:**
  - `{Function} callback`

- **用法:**

  Defer the callback to be executed after the next DOM update cycle. Use it immediately after you've changed some data to wait for the DOM update. This is the same as the global `Vue.nextTick`, except that the callback's `this` context is automatically bound to the instance calling this method.
  将回调推迟到下一个 DOM 更新循环再执行。在你修改数据之后立即使用这个方法，等待 DOM 更新。这跟全局的 `Vue.nextTick` 几乎一模一样，唯一不同的是调用这个方法时，回调的 `this` 自动绑定实例。

- **示例:**

  ``` js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // modify data
        // 修改数据
        this.message = 'changed'
        // DOM is not updated yet
        // DOM 还没有更新
        this.$nextTick(function () {
          // DOM is now updated
          // 到这里 DOM 已经更新了
          // `this` is bound to the current instance
          // `this` 已经绑定到当前的实例
          this.doSomethingElse()
        })
      }
    }
  })
  ```


- **另见:**
  - [Vue.nextTick](#Vue-nextTick)
  - [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)
  - [异步更新队列](../guide/reactivity.html#Async-Update-Queue)

<h3 id="vm-destroy">vm.$destroy()</h3>

- **用法:**

  Completely destroy a vm. Clean up its connections with other existing vms, unbind all its directives, turn off all event listeners.
  完全销毁实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。

  Triggers the `beforeDestroy` and `destroyed` hooks.
  触发 `beforeDestroy` 和 `destroyed` 钩子.

  <p class="tip">In normal use cases you shouldn't have to call this method yourself. Prefer controlling the lifecycle of child components in a data-driven fashion using `v-if` and `v-for`.</p>
  <p class="tip">在正常使用情况下，你不应该自己调用这个方法。更好地控制子组件的生命周期，是以数据驱动的方式使用 `v-if` 和 `v-for`。</p>

- **See also:** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
- **另见:** [生命周期图示](../guide/instance.html#Lifecycle-Diagram)

## 指令 (Directives)

### v-text

- **Expects:** `string`
- **类型:** `string`

- **Details:**
- **详细:**

  Updates the element's `textContent`. If you need to update the part of `textContent`, you should use `{% raw %}{{ Mustache }}{% endraw %}` interpolations.
  更新元素的 `textContent`。如果你需要更新 `textContent` 的一部分，你应该使用 `{% raw %}{{ Mustache }}{% endraw %}` 插值。

- **示例:**

  ```html
  <span v-text="msg"></span>
  <!-- same as -->
  <!-- 跟上面一样 -->
  <span>{{msg}}</span>
  ```

- **See also:** [Data Binding Syntax - interpolations](../guide/syntax.html#Text)
- **另见:** [数据绑定语法 - 插值](../guide/syntax.html#Text)

### v-html

- **Expects:** `string`
- **类型:** `string`

- **Details:**
- **详细:**

  Updates the element's `innerHTML`. **Note that the contents are inserted as plain HTML - they will not be compiled as Vue templates**. If you find yourself trying to compose templates using `v-html`, try to rethink the solution by using components instead.
  更新元素的 `innerHTML`。**注意，内容会以普通 HTML 的方式插入 —— 它们将不会被当作 Vue 模板来编译**。如果你想用 `v-html` 去组合模板，你应该考虑使用组件。

  <p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use `v-html` on trusted content and **never** on user-provided content.
  在网站上动态渲染 HTML 是非常危险的，因为容易遭受 [XSS攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。请只对可信的内容使用 `v-html` ，**永远不要** 用在用户提交的内容上。</p>

- **示例:**

  ```html
  <div v-html="html"></div>
  ```

- **See also:** [Data Binding Syntax - interpolations](../guide/syntax.html#Raw-HTML)
- **另见:** [数据绑定语法 —— 插值](../guide/syntax.html#Raw-HTML)

### v-if

- **Expects:** `any`
- **类型:** `any`

- **用法:**

  Conditionally render the element based on the truthy-ness of the expression value. The element and its contained directives / components are destroyed and re-constructed during toggles. If the element is a `<template>` element, its content will be extracted as the conditional block.
  根据表达式的值的条件真假渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>`，将提出它的内容作为条件块。

  This directive triggers transitions when its condition changes.
  当指令条件改变将触发转换。

- **See also:** [Conditional Rendering - v-if](../guide/conditional.html)
- **另见:** [条件渲染 - v-if](../guide/conditional.html)

### v-show

- **Expects:** `any`
- **类型:** `any`

- **用法:**

  Toggle's the element's `display` CSS property based on the truthy-ness of the expression value.
  根据表达式的值，切换元素的 `display` CSS 属性。

  This directive triggers transitions when its condition changes.
  当条件改变时，该指令将触发转换。

- **See also:** [Conditional Rendering - v-show](../guide/conditional.html#v-show)
- **另见:** [条件渲染 - v-show](../guide/conditional.html#v-show)

### v-else

- **Does not expect expression**
- **不需要表达式**

- **Restriction:** previous sibling element must have `v-if`.
- **限制:** 前一兄弟元素必须有 `v-if`.

- **用法:**

  Denote the "else block" for `v-if`.
  表示 `v-if` 的 "else 块"。

  ```html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **另见:**
  - [Conditional Rendering - v-else](../guide/conditional.html#v-else)
  - [条件渲染 - v-else](../guide/conditional.html#v-else)

### v-for

- **Expects:** `Array | Object | number | string`
- **类型:** `Array | Object | number | string`

- **用法:**

  Render the element or template block multiple times based on the source data. The directive's value must use the special syntax `alias in expression` to provide an alias for the current element being iterated on:
  基于源数据将元素或模板块多次渲染。指令的值必须使用特定语法 `alias in expression`，为当前遍历的元素提供别名：

  ``` html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatively, you can also specify an alias for the index (or the key if used on an Object):
  另外也可以为数组索引指定别名（如果值是对象可以为键指定别名）：

  ``` html
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, key, index) in object"></div>
  ```

  The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder elements, you need to provide an ordering hint with the `key` special attribute:
  默认地，`v-for` 会尝试填补元素位置而不去移动它们。如果要强制给元素排序，你需要使用 `key` 特殊属性来提供排序的依据：

  ``` html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  The detailed usage for `v-for` is explained in the guide section linked below.
  `v-for` 的详细用法可以在下面链接的教程里找到。

- **另见:**
  - [List Rendering](../guide/list.html)
  - [列表渲染](../guide/list.html)
  - [key](../guide/list.html#key)
  - [键](../guide/list.html#key)

### v-on

- **Shorthand:** `@`
- **缩写:** `@`

- **Expects:** `Function | Inline Statement`
- **类型:** `Function | Inline Statement`

- **参数:** `event (required)`

- **修饰符:**
  - `.stop` - call `event.stopPropagation()`.
  - `.stop` - 调用 `event.stopPropagation()`.
  - `.prevent` - call `event.preventDefault()`.
  - `.prevent` - 调用 `event.preventDefault()`.
  - `.capture` - add event listener in capture mode.
  - `.capture` - 添加事件侦听器时使用 capture 模式.
  - `.self` - only trigger handler if event was dispatched from this element.
  - `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  - `.{keyCode | keyAlias}` - only trigger handler on certain keys.
  - `.{keyCode | keyAlias}` - 只在指定按键上触发回调。
  - `.native` - listen for a native event on the root element of component.
  - `.native` - 在组件的根元素上监听本地事件。

- **用法:**

  Attaches an event listener to the element. The event type is denoted by the argument. The expression can either be a method name or an inline statement, or simply omitted when there are modifiers present.
  为元素绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果有修饰符也可以省略。

  When used on a normal element, it listens to **native DOM events** only. When used on a custom element component, it also listens to **custom events** emitted on that child component.
  用在普通元素上时，只能监听 **原生 DOM 事件**。用在自定义元素组件上时，还会监听子组件触发的 **自定义事件**。

  When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.
  在监听原生 DOM 事件时，方法以原生事件作为唯一的参数。如果使用内联语句，语句可以访问特殊的 `$event` 属性：`v-on:click="handle('ok',$event)"`。

- **示例:**

  ```html
  <!-- method handler -->
  <!-- 方法名 -->
  <button v-on:click="doThis"></button>

  <!-- inline statement -->
  <!-- 内联语句 -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <!-- 缩写 -->
  <button @click="doThis"></button>

  <!-- stop propagation -->
  <!-- 阻止事件传播 -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <!-- 禁用事件的默认行为 -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <!-- 单纯禁用事件的默认行为 -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <!-- 链式修饰符 -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <!-- 键修饰符(键别名) -->
  <input @keyup.enter="onEnter">

  <!-- key modifier using keyCode -->
  <!-- 键修饰符(键码值) -->
  <input @keyup.13="onEnter">
  ```

  Listening to custom events on a child component (the handler is called when "my-event" is emitted on the child):
  在子组件上监听自定义事件（当子组件触发 "my-event" 时将调用事件处理器）：

  ```html
  <my-component @my-event="handleThis"></my-component>

  <!-- inline statement -->
  <!-- 内联语句 -->
  <my-component @my-event="handleThis(123, $event)"></my-component>

  <!-- native event on component -->
  <!-- 组件的原生事件 -->
  <my-component @click.native="onClick"></my-component>
  ```

- **另见:**
  - [Methods and Event Handling](../guide/events.html)
  - [方法与事件处理器](../guide/events.html)
  - [Components - Custom Events](../guide/components.html#Custom-Events)
  - [组件 —— 自定义事件](../guide/components.html#Custom-Events)

### v-bind

- **缩写:** `:`

- **Expects:** `any (with argument) | Object (without argument)`
- **类型:** `any (with argument) | Object (without argument)`

- **参数:** `attrOrProp (optional)`

- **Modifiers:**
- **修饰符:**
  - `.prop` - Used for binding DOM attributes.
  - `.prop` - 用于绑定DOM属性。

- **用法:**

  Dynamically bind one or more attributes, or a component prop to an expression.
  动态绑定元素个属性或组件 prop。

  When used to bind the `class` or `style` attribute, it supports additional value types such as Array or Objects. See linked guide section below for more details.
  在绑定 `class` 或 `style` 时，支持其它类型的值，如数组或对象。下面链接的教程里有更详细的介绍。

  When used for prop binding, the prop must be properly declared in the child component.
  在绑定 prop 时，prop 必须在子组件中声明。

  When used without an argument, can be used to bind an object containing attribute name-value pairs. Note in this mode `class` and `style` does not support Array or Objects.
  没有参数时，可以用来绑定到一个包含键值对的对象。在使用这个方法的时候，`class` 和 `style` 绑定不支持数组和对象。

- **示例:**

  ```html
  <!-- bind an attribute -->
  <!-- 绑定一个属性 -->
  <img v-bind:src="imageSrc">

  <!-- shorthand -->
  <!-- 缩写 -->
  <img :src="imageSrc">

  <!-- class binding -->
  <!-- class 绑定 -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">

  <!-- style binding -->
  <!-- style 绑定 -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <!-- 绑定一个对象 -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- DOM attribute binding with prop modifier -->
  <!-- 用 prop 修饰符绑定 DOM 属性 -->
  <div v-bind:text-content.prop="text"></div>

  <!-- prop binding. "prop" must be declared in my-component. -->
  <!-- prop 绑定。my-component 必须先声明 prop -->
  <my-component :prop="someThing"></my-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

- **另见:**
  - [Class and Style Bindings](../guide/class-and-style.html)
  - [Class 和 Style 绑定](../guide/class-and-style.html)
  - [Components - Component Props](../guide/components.html#Props)
  - [组件 Props](../guide/components.html#Props)

### v-model

- **Expects:** varies based on value of form inputs element or output of components
- **类型:** 随着表单输入控件的值的不同或组件输出的不同而不同

- **Limited to:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - components
- **限制:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - 组件

- **修饰符:**
  - [`.lazy`](../guide/forms.html#lazy) - listen to `change` events instead of `input`
  - [`.lazy`](../guide/forms.html#lazy) - 监听 `change` 事件而不是`input`事件
  - [`.number`](../guide/forms.html#number) - cast input string to numbers
  - [`.number`](../guide/forms.html#number) - 将输入的字符串转换成数值
  - [`.trim`](/guild/forms.html#trim) - trim input
  - [`.trim`](/guild/forms.html#trim) - 去除输入内容的首位空格

- **用法:**

  Create a two-way binding on a form input element or a component. For detailed usage, see guide section linked below.
  在组件或表单控件上创建一个双向绑定。下面链接的教程中有更详细的用法。

- **另见:**
  - [Form Input Bindings](../guide/forms.html)
  - [表单控件绑定](../guide/forms.html)
  - [Components - Form Input Components using Custom Events](../guide/components.html#Form-Input-Components-using-Custom-Events)
  - [组件 —— 表单控件组件使用自定义事件](../guide/components.html#Form-Input-Components-using-Custom-Events)

### v-pre

- **Does not expect expression**
- **不需要表达式**

- **用法**

  Skip compilation for this element and all its children. You can use this for displaying raw mustache tags. Skipping large numbers of nodes with no directives on them can also speed up compilation.
  不去编译这个元素和它的子元素。可以用来显示原生 Mustache 标签。跳过大量没有指令的节点会加快编译。

- **示例:**

  ```html
  <span v-pre>{{ this will not be compiled }}</span>
   ```

### v-cloak

- **Does not expect expression**
- **不需要表达式**

- **用法:**

  This directive will remain on the element until the associated Vue instance finishes compilation. Combined with CSS rules such as `[v-cloak] { display: none }`, this directive can be used to hide un-compiled mustache bindings until the Vue instance is ready.
  直到关联实例完成编译，这个指令都会保持在元素上。如果和像 `[v-cloak] { display: none }` 这样的 CSS 规则一起使用时，这个指令可以隐藏未编译的 Mustache 标签，直到实例完成编译。

- **示例:**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  The `<div>` will not be visible until the compilation is done.
  `<div>` 不会显示，直到编译完成。

### v-once

- **Does not expect expression**
- **不需要表达式**

- **Details:**
- **详细:**

  Render the element and component **once** only. On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. This can be used to optimize update performance.
  只渲染元素和组件 **一次**。在后面的重新渲染中，这个元素/组件以及它们所有的子元素都会被当作静态内容去忽略掉。这个指令可以用来优化更新的性能。

  ```html
  <!-- single element -->
  <!-- 单个元素 -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- the element have children -->
  <!-- 带有子元素的元素 -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- component -->
  <!-- 组件 -->
  <my-component v-once :comment="msg"></my-component>
  <!-- v-for directive -->
  <!-- v-for 指令 -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

- **另见:**
  - [Data Binding Syntax - interpolations](../guide/syntax.html#Text)
  - [数据绑定语法 —— 插值](../guide/syntax.html#Text)
  - [Components - Cheap Static Components with v-once](../guide/components.html#Cheap-Static-Components-with-v-once)
  - [组件 —— 使用v-once的轻量级静态组件](../guide/components.html#Cheap-Static-Components-with-v-once)

## 特殊属性 (Special Attributes)

### key

- **Expects:** `string`
- **特性：** `string`

  The `key` special attribute is primarily used as a hint for Vue's virtual DOM algorithm to identify VNodes when diffing the new list of nodes against the old list. Without keys, Vue uses an algorithm that minimizes element movement and tries to patch/reuse elements of the same type in-place as much as possible. With keys, it will reorder elements based on the order change of keys, and elements with keys that are no longer present will always be removed/destroyed.
  当 Vue 的虚拟 DOM 算法差分计算新的节点列表和旧的节点列表，来识别 VNodes 时，key 这个特殊属性会为该算法提供线索。在不使用 keys 的情况下，Vue 会使用一个算法来减少元素的移动，并且尽可能在适当的位置，来修补/重新使用相同类型的元素。在使用 keys 的情况下，Vue 会基于 keys 的变化顺序来重新排列元素，那些包含 keys 但不再出现的元素通常将被移除/销毁。

  Children of the same common parent must have **unique keys**. Duplicate keys will cause render errors.
  具有相同父元素的子元素必须使用唯一 keys。重复 keys 会导致渲染报错。

  The most common use case is combined with `v-for`:
  最常见的用例是结合 `v-for` 一起使用。

  ``` html
  <ul>
    <li v-for="item in items" :key="item.id">...</li>
  </ul>
  ```

  It can also be used to force replacement of an element/component instead of reusing it. This can be useful when you want to:
  也可以使用 key 属性来强行替换一个元素/组件而不是重新使用元素/组件。这种做法会很有用，当你想尝试以下做法：

  - Properly trigger lifecycle hooks of a component
  - 适当地触发一个组件的生命周期钩子
  - Trigger transitions
  - 触发过渡

  For example:

  ``` html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  When `text` changes, the `<span>` will always be replaced instead of patched, so a transition will be triggered.
  当 text 变化时，<span> 元素也会跟着一起被替换而不是被修补，因此会触发一个过渡。

### ref

- **Expects:** `string`
- **特性：** `string`


  `ref` is used to register a reference to an element or a child component. The reference will be registered under the parent component's `$refs` object. If used on a plain DOM element, the reference will be that element; if used on a child component, the reference will be component instance:
  使用 ref 属性来注册一个元素和子组件的引用。该引用被挂载到父组件的 `$refs` 对象下。如果在一个原生 DOM 元素上使用 ref ，该引用将会变成该元素；如果在一个子元素上使用 ref ,这个引用会变成该组件的实例：

  ``` html
  <!-- vm.$refs.p will the DOM node -->
  <!-- vm.$refs.p 将会变成 DOM 节点 -->
  <p ref="p">hello</p>

  <!-- vm.$refs.child will be the child comp instance -->
  <!-- vm.$refs.child 将会变成子元素的实例 -->
  <child-comp ref="child"></child-comp>
  ```

  When used on elements/components with `v-for`, the registered reference will be an Array containing DOM nodes or component instances.
  当在含有 v-for 的元素/组件上使用 ref 属性时，被注册的引用会变成一个包含各 DOM 节点或者组件实例的数组。

  An important note about the ref registration timing: because the refs themselves are created as a result of the render function, you cannot access them on the initial render - they don't exist yet! `$refs` is also non-reactive, therefore you should not attempt to use it in templates for data-binding.
  需要注意的一点是关于 ref 的注册时间点： 因为 refs 自身是作为 render 函数执行完后所创造的一个结果值，所以开发者不能再初始 render 时去使用它们，因为 refs 还不存在。`$refs` 也是不可交互的，因此开发者不要尝试在模板中为了数据绑定去使用它。

- **另见：** [Child Component Refs](../guide/components.html#Child-Component-Refs)

### slot

- **Expects:** `string`
- **特性：** `string`

  Used on content inserted into child components to indicate which named slot the content belongs to.
  在插入到子组件的内容上使用 slot 属性，来表明哪个命名的 slot 是归该内容所属。

  For detailed usage, see the guide section linked below.
  想知道更详细的用法，可参考以下链接

- **另见：**[Named Slots](../guide/components.html#Named-Slots)

## 内置组件 (Built-In Components)

### component

- **属性：**
  - `is` - string | ComponentDefinition(组件声明) | ComponentConstructor(组件构造器)
  - `inline-template` - boolean

- **用法：**

  A "meta component" for rendering dynamic components. The actual component to render is determined by the `is` prop:
  渲染动态组件的元组件（meta component）。真正渲染的组件是 `is` 属性定义的组件

  ```html
  <!-- a dynamic component controlled by -->
  <!-- the `componentId` property on the vm -->
  <!-- 动态组件 -->
  <!-- 由实例的 `componentId` 属性控制 -->
  <component :is="componentId"></component>

  <!-- can also render registered component or component passed as prop -->
  <!-- 其他已注册的组件 -->
  <component :is="$options.components.child"></component>
  ```

- **See also:** [Dynamic Components](../guide/components.html#Dynamic-Components)
- **另见：** [动态组件](../guide/components.html#Dynamic-Components)

### transition

- **属性:**
  - `name` - string, Used to automatically generate transition CSS class names. e.g. `name: 'fade'` will auto expand to `.fade-enter`, `.fade-enter-active`, etc. Defaults to `"v"`.
  - `name` - string，自动生成 CSS 过渡动画样式名字 例如： `name: 'fade'` 将会生成 `.fade-enter`， `.fade-enter-active`等等。 默认是 `"v"`。
  - `appear` - boolean, Whether to apply transition on initial render. Defaults to `false`.
  - `appear` - boolean，初始渲染的时候是否执行过渡动画。默认 `false`.
  - `css` - boolean, Whether to apply CSS transition classes. Defaults to `true`. If set to `false`, will only trigger JavaScript hooks registered via component events.
  - `css` - boolean，是否使用 CSS 过渡动画样式。 默认 `true`。 如果设置为 `false`，只会触发在组件事件中注册的 JavaScript 钩子。
  - `type` - string, Specify the type of transition events to wait for to determine transition end timing. Available values are `"transition"` and `"animation"`. By default, it will automatically detect the type that has a longer duration.
  - `type` - string，指定过渡事件的类型，以确定过渡结束时间，可用的值有 `"transition"` and `"animation"`。它会将持续时间较长的类型作为默认值。
  - `mode` - string, Controls the timing sequence of leaving/entering transitions. Available modes are `"out-in"` and `"in-out"`; defaults to simultaneous.
  - `mode` - string， 控制离开/进入过渡动画的时间顺序。可用的模式有 `"out-in"` and `"in-out"`； 默认是同时。
  - `enter-class` - string
  - `leave-class` - string
  - `enter-active-class` - string
  - `leave-active-class` - string
  - `appear-class` - string
  - `appear-active-class` - string

- **事件：**
  - `before-enter`
  - `enter`
  - `after-enter`
  - `before-leave`
  - `leave`
  - `after-leave`
  - `before-appear`
  - `appear`
  - `after-appear`

- **用法：**

  `<transition>` serve as transition effects for **single** element/component. The `<transition>` does not render an extra DOM element, nor does it show up in the inspected component hierarchy. It simply applies the transition behavior to the wrapped content inside.
  `<transition>` 用于 **单** 元素/组件的过渡效果。`<transition>` 不会渲染额外的 DOM 元素, 也不显示在组件的层次结构中。它只对内部的包裹内容产生过渡行为。

  ```html
  <!-- simple element -->
  <!-- 简单元素 -->
  <transition>
    <div v-if="ok">toggled content</div>
  </transition>

  <!-- dynamic component -->
  <!-- 动态组件 -->
  <transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </transition>

  <!-- event hooking -->
  <!-- 事件钩子 -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete">
      <div v-show="ok">toggled content</div>
    </transition>
  </div>
  ```

  ``` js
  new Vue({
    ...
    methods: {
      transitionComplete: function (el) {
        // for passed 'el' that DOM element as the argument, something ...
        // 传 DOM 元素 'el' 作为参数, 其他操作
      }
    }
    ...
  }).$mount('#transition-demo')
  ```

- **See also:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)
- **另见：** [过渡： 进入，离开，和列表](../guide/transitions.html)

### transition-group

- **属性：**
  - `tag` - string, defaults to `span`.
  - `tag` - string, 默认值 `span`.
  - `move-class` - overwrite CSS class applied during moving transition.
  - `move-class` - 重写离开时的 CSS 过渡样式
  - exposes the same props as `<transition>` except `mode`.
  - 除了 `mode` 其他属性和 `<transition>` 一样。

- **事件：**
  - exposes the same events as `<transition>`.
  - 和 `<transition>` 的事件一样。

- **用法**

  `<transition-group>` serve as transition effects for **multiple** elements/components. The `<transition-group>` renders a real DOM element. By default it renders a `<span>`, and you can configure what element is should render via the `tag` attribute.
  `<transition-group>` 用于 **多** 元素/组件的过渡效果。 `<transition-group>` 会渲染真实的 DOM 元素。 默认它会渲染一个 `<span>` 标签， 当然你也可以通过 `tag` 属性来配置它渲染什么标签元素。

  Note every child in a `<transition-group>` must be **uniquely keyed** for the animations to work properly.
  注意 `<transition-group>` 里的每一个子元素必须要有唯一的 `key` 属性标识，动画才能正常工作。

  `<transition-group>` supports moving transitions via CSS transform. When a child's position on screen has changed after an updated, it will get applied a moving CSS class (auto generated from the `name` attribute or configured with the `move-class` attribute). If the CSS `transform` property is "transition-able" when the moving class is applied, the element will be smoothly animated to its destination using the [FLIP technique](https://aerotwist.com/blog/flip-your-animations/).
  `<transition-group>` 支持 CSS transform 来定义切换时的过渡效果。当更新之后，子元素在屏幕中的位置发生变化时，它会调用离开时的 CSS 过渡样式（自动通过 `name` 属性或者 `move-class` 属性来生成）。当调用离开时 CSS 过渡样式时，并且CSS 的 `transform` 属性是 "可过渡"， 元素会使用 [FLIP technique](https://aerotwist.com/blog/flip-your-animations/) 圆滑过渡到它的目标位置。

  ```html
  <transition-group tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </transition-group>
  ```

- **See also:** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)
- **另见：** [过渡： 进入，离开，和列表](../guide/transitions.html)

### keep-alive

- **用法：**

  When wrapped around a dynamic component, `<keep-alive>` caches the inactive component instances without destroying them. Similar to `<transition>`, `<keep-alive>` is an abstract component: it doesn't render a DOM element itself, and doesn't show up in the component parent chain.
  当包裹着一个动态组件时，`<keep-alive>` 会把不活动的组件实例缓存起来，而不销毁它。 跟 `<transition>` 相似，`<keep-alive>` 是个抽象组件: 他不会真实渲染 DOM 元素，也不会在组件父链上出现。

  When a component is toggled inside `<keep-alive>`, its `activated` and `deactivated` lifecycle hooks will be invoked accordingly.
  当在 `<keep-alive>` 里面的组件进行切换的时候， 它的 `activated` 和 `deactivated` 生命周期钩子将会相应地被调用。

  Primarily used with preserve component state or avoid re-rendering.
  主要用于保存组件之前的状态和避免重新渲染

  ```html
  <!-- basic -->
  <!-- 基本用法 -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>

  <!-- multiple conditional children -->
  <!-- 多个有条件的子组件 -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>

  <!-- used together with <transition> -->
  <!-- 和 <transition> 一起使用-->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

  <p class="tip">`<keep-alive>` does not work with functional components because they do not have instances to be cached.
  `<keep-alive>` 在函数化组件中不会生效。因为它根本就没有实例可以被缓存。</p>

- **See also:** [Dynamic Components - keep-alive](../guide/components.html#keep-alive)
- **另见：** [动态组件 - keep-alive](../guide/components.html#keep-alive)

### slot

- **属性：**
  - `name` - string, Used for named slot.
  - `name` - string, 用来命名 slot。

- **用法：**

  `<slot>` serve as content distribution outlets in component templates. `<slot>` itself will be replaced.
  `<slot>` 作为组件模版中的内容分发点。 `<slot>` 本身会被替换掉。

  For detailed usage, see the guide section linked below.
  有关详细用法，参见下面的指南部分。

- **See also:** [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)
- **另见：** [使用 Slots 内容分发](../guide/components.html#Content-Distribution-with-Slots)

## VNode 接口 (VNode Interface)

- Please refer to the [VNode class declaration](https://github.com/vuejs/vue/blob/next/src/core/vdom/vnode.js).
- 请参阅 [VNode 类声明](https://github.com/vuejs/vue/blob/next/src/core/vdom/vnode.js).

## 服务端渲染 (Server-Side Rendering)

- Please refer to the [vue-server-renderer package documentation](https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer).
- 请参阅 [Vue 服务端渲染文档](https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer).
