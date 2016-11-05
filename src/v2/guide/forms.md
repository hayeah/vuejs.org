---
title: 表单控件绑定 (Form Input Bindings)
type: guide
order: 10
---

## 基本用法 （Basic Usage）

You can use the `v-model` directive to create two-way data bindings on form input and textarea elements. It automatically picks the correct way to update the element based on the input type. Although a bit magical, `v-model` is essentially syntax sugar for updating data on user input events, plus special care for some edge cases.
你可以使用 `v-model` 指令在表单 `input` 和 `textarea` 元素上创建双向数据邦定，Vue 会根据控件的类型选择正确的方法自动更新相应的元素。尽管这看起来很神奇，`v-model` 实际上是基于用户输入事件更新数据的语法糖，并对极端的情况给予特殊处理。

<p class="tip">`v-model` doesn't care about the initial value provided to an input or a textarea. It will always treat the Vue instance data as the source of truth.
`v-model` 不关心在 `input` 或 `textarea` 值中的初始值，它只会认定 Vue 实例数据为真实的数据来源。</p>

### Text

``` html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

{% raw %}
<div id="example-1" class="demo">
  <input v-model="message" placeholder="edit me">
  <p>Message is: {{ message }}</p>
</div>
<script>
new Vue({
  el: '#example-1',
  data: {
    message: ''
  }
})
</script>
{% endraw %}

### 多行文本 (Multiline text)

``` html
<span>Multiline message is:</span>
<p style="white-space: pre">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

{% raw %}
<div id="example-textarea" class="demo">
  <span>Multiline message is:</span>
  <p style="white-space: pre">{{ message }}</p>
  <br>
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
</div>
<script>
new Vue({
  el: '#example-textarea',
  data: {
    message: ''
  }
})
</script>
{% endraw %}


{% raw %}
<p class="tip">Interpolation on textareas (<code>&lt;textarea&gt;{{text}}&lt;/textarea&gt;</code>) won't work. Use <code>v-model</code> instead.</p>
{% endraw %}

### Checkbox

Single checkbox, boolean value:
单个勾选框，布尔值：

``` html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
{% raw %}
<div id="example-2" class="demo">
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
</div>
<script>
new Vue({
  el: '#example-2',
  data: {
    checked: false
  }
})
</script>
{% endraw %}

Multiple checkboxes, bound to the same Array:
多个钩选框，绑定到同一数组：

``` html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
```

``` js
new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```

{% raw %}
<div id="example-3" class="demo">
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
<script>
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
</script>
{% endraw %}

### Radio


``` html
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
<br>
<span>Picked: {{ picked }}</span>
```
{% raw %}
<div id="example-4" class="demo">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
<script>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
</script>
{% endraw %}

### Select

Single select:
单选：

``` html
<select v-model="selected">
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Selected: {{ selected }}</span>
```
{% raw %}
<div id="example-5" class="demo">
  <select v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-5',
  data: {
    selected: null
  }
})
</script>
{% endraw %}

Multiple select (bound to Array):
多选（绑定到同一数组）：

``` html
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Selected: {{ selected }}</span>
```
{% raw %}
<div id="example-6" class="demo">
  <select v-model="selected" multiple style="width: 50px">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
</script>
{% endraw %}

Dynamic options rendered with `v-for`:
由 `v-for` 渲染出来的动态选项：

``` html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```
``` js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```
{% raw %}
<div id="example-7" class="demo">
  <select v-model="selected">
    <option v-for="option in options" v-bind:value="option.value">
      {{ option.text }}
    </option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-7',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
</script>
{% endraw %}

## 绑定值 (Value Bindings)

For radio, checkbox and select options, the `v-model` binding values are usually static strings (or booleans for checkbox):
对于单选按钮、勾选框和选择框选项，`v-model` 绑定的值通常是静态字符串（对于勾选框来说是布尔值）：

``` html
<!-- `picked` is a string "a" when checked -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` is either true or false -->
<input type="checkbox" v-model="toggle">

<!-- `selected` is a string "abc" when selected -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

But sometimes we may want to bind the value to a dynamic property on the Vue instance. We can use `v-bind` to achieve that. In addition, using `v-bind` allows us to bind the input value to non-string values.
但有时我们想把值绑定到 Vue 实例上的一个动态属性上。我们可以使用 `v-bind` 实现数据动态绑定，并且绑定的这个属性的值可以是非字符串的值。

### Checkbox

``` html
<input
  type="checkbox"
  v-model="toggle"
  v-bind:true-value="a"
  v-bind:false-value="b"
>
```

``` js
// when checked:
vm.toggle === vm.a
// when unchecked:
vm.toggle === vm.b
```

### Radio

``` html
<input type="radio" v-model="pick" v-bind:value="a">
```

``` js
// when checked:
vm.pick === vm.a
```

### Select Options

``` html
<select v-model="selected">
  <!-- inline object literal -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

``` js
// when selected:
typeof vm.selected // -> 'object'
vm.selected.number // -> 123
```

## 修饰器 (Modifiers)

### `.lazy`

By default, `v-model` syncs the input with the data after each `input` event. You can add the `lazy` modifier to instead sync after `change` events:
默认地，`v-model` 在每次 `input` 事件同步输入框值与数据。你也可以添加 `lazy` 修饰符，使数据同步在 `change` 事件后发生：

``` html
<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg" >
```

### `.number`

If you want user input to be automatically typecast as a number, you can add the `number` modifier to your `v-model` managed inputs:
如果你想让用户输入自动被类型转换为数值，你可以在 `v-model` 后添加 `number` 修饰符到相应的输入绑定控件中。

``` html
<input v-model.number="age" type="number">
```

This is often useful, because even with `type="number"`, the value of HTML input elements always returns a string.
这通常很有用，因为即使在元素中添加了 `type="number"`，在 HTML 中 `input` 元素的值总会返回一个字符串。

### `.trim`

If you want user input to be trimmed automatically, you can add the `trim` modifier to your `v-model` managed inputs:
如果你想用户用户输入被自动去除首尾空白字符，你可以在 `v-model` 后添加 `trim` 修饰符到相应的输入绑定控件中：

```html
<input v-model.trim="msg">
```

## `v-model` with Components

> If you're not yet familiar with Vue's components, just skip this for now.

HTML's built-in input types won't always meet your needs. Fortunately, Vue components allow you to build reusable inputs with completely customized behavior. These inputs even work with `v-model`! To learn more, read about [custom inputs](components.html#Form-Input-Components-using-Custom-Events) in the Components guide.







