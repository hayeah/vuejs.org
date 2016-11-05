---
title: 单元测试 (Unit Testing)
type: guide
order: 23
---

## 安装与工具 (Setup and Tooling)

Anything compatible with a module-based build system will work, but if you're looking for a specific recommendation, try the [Karma](http://karma-runner.github.io) test runner. It has a lot of community plugins, including support for [Webpack](https://github.com/webpack/karma-webpack) and [Browserify](https://github.com/Nikku/karma-browserify). For detailed setup, please refer to each project's respective documentation, though these example Karma configurations for [Webpack](https://github.com/vuejs-templates/webpack/blob/master/template/test/unit/karma.conf.js) and [Browserify](https://github.com/vuejs-templates/browserify/blob/master/template/karma.conf.js) may help you get started.
你可以使用任何兼容模块化构建的测试工具。如果你需要个具体建议，可以试试 [Karma](http://karma-runner.github.io/0.12/index.html)。它有很多的社区插件，包括对 [Webpack](https://github.com/webpack/karma-webpack) 和 [Browserify](https://github.com/Nikku/karma-browserify) 的支持。至于具体的安装细节，请参考各个项目的文档。这里有两个karma的配置实例，能够帮助你快速入门。一个是对 [Webpack](https://github.com/vuejs-templates/webpack/blob/master/template/test/unit/karma.conf.js)，另一个是对 [Browserify](https://github.com/vuejs-templates/browserify/blob/master/template/karma.conf.js)。

## 简单断言 (Simple Assertions)

In terms of code structure for testing, you don't have to do anything special in your components to make them testable. Just export the raw options:
就测试的代码结构而言，你不需要在组件里做任何特别的事，就能进行测试。只要导入以下选项：

``` html
<template>
  <span>{{ message }}</span>
</template>

<script>
  export default {
    data () {
      return {
        message: 'hello!'
      }
    },
    created () {
      this.message = 'bye!'
    }
  }
</script>
```

When you test that component, all you have to do is import the object along with Vue to make many common assertions:
当你测试这个组件时，你只要同时引入 Vue 和这个组件，就能做很多常用的断言了。

``` js
// Import Vue and the component being tested
// 引入 Vue 和待测试的组件
import Vue from 'vue'
import MyComponent from 'path/to/MyComponent.vue'

// Here are some Jasmine 2.0 tests, though you can
// use any test runner / assertion library combo you prefer
//这里是一些Jasmine 2.0 的测试，你也可以使用任何你喜欢的测试运行器/断言库组合
describe('MyComponent', () => {
  // Inspect the raw component options
  //检测原始组件选项
  it('has a created hook', () => {
    expect(typeof MyComponent.created).toBe('function')
  })

  // Evaluate the results of functions in
  // the raw component options
  //计算在原始组件选项里的函数运行结果
  it('sets the correct default data', () => {
    expect(typeof MyComponent.data).toBe('function')
    const defaultData = MyComponent.data()
    expect(defaultData.message).toBe('hello!')
  })

  // Inspect the component instance on mount
  //检测已经装载的组件实例
  it('correctly sets the message when created', () => {
    const vm = new Vue(MyComponent).$mount()
    expect(vm.message).toBe('bye!')
  })

  // Mount an instance and inspect the render output
  //装载一个实例，检测渲染的输出结果
  it('renders the correct message', () => {
    const Ctor = Vue.extend(MyComponent)
    const vm = new Ctor().$mount()
    expect(vm.$el.textContent).toBe('bye!')
  })
})
```

## 编写可测试的组件 (Writing Testable Components)

A lot of components' render output are primarily determined by the props they receive. In fact, if a component's render output solely depends on its props, it becomes quite straightforward to test, similar to asserting the return value of a pure function with different arguments. Take an contrived example:
很多组件的渲染结果基本上都取决于它们接收到的属性。如果一个组件的渲染结果仅仅取决于它的属性，测试它就会很直观。这就像根据不同的参数，断言一个纯函数的返回值。看下面这个虚构的例子：

``` html
<template>
  <p>{{ msg }}</p>
</template>

<script>
  export default {
    props: ['msg']
  }
</script>
```

You can assert its render output with different props using the `propsData` option:
通过 `propsData` 选项，你可以断言不同属性所产生的渲染结果

``` js
import Vue from 'vue'
import MyComponent from './MyComponent.vue'

// helper function that mounts and returns the rendered text
//装载并返回被渲染文本的辅助函数
function getRenderedText (Component, propsData) {
  const Ctor = Vue.extend(Component)
  const vm = new Ctor({ propsData }).$mount()
  return vm.$el.textContent
}

describe('MyComponent', () => {
  it('render correctly with different props', () => {
    expect(getRenderedText(MyComponent, {
      msg: 'Hello'
    })).toBe('Hello')

    expect(getRenderedText(MyComponent, {
      msg: 'Bye'
    })).toBe('Bye')
  })
})
```

## 断言异步更新 (Asserting Asynchronous Updates)

Since Vue [performs DOM updates asynchronously](reactivity.html#Async-Update-Queue), assertions on DOM updates resulting from state change will have to be made in a `Vue.nextTick` callback:
由于 Vue [异步执行 DOM 更新](reactivity.html#Async-Update-Queue)，对状态变化引起的 DOM 更新的断言，需要放在一个 `Vue.nextTick` 的回调里。

``` js
// Inspect the generated HTML after a state update
// 状态更新后，检测生成的HTML
it('updates the rendered message when vm.message updates', done => {
  const vm = new Vue(MyComponent).$mount()
  vm.message = 'foo'

  // wait a "tick" after state change before asserting DOM updates
  //在状态变化之后，断言DOM更新之前，先等待一次nextTick的执行
  Vue.nextTick(() => {
    expect(vm.$el.textContent).toBe('foo')
    done()
  })
})
```

We are planning to work on a collection of common test helpers that makes it even simpler to render components with different constraints (e.g. shallow rendering that ignores child components) and assert their output.
我们正计划做一个通用的测试辅助器集合。根据不同的限制(比如不渲染子组件的浅层渲染)去渲染组件，断言输出。这样的集合能让测试变得更简单。