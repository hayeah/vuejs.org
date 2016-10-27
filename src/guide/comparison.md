---
title: 对比其它框架 (Comparison with Other Frameworks)
type: guide
order: 28
---

This is definitely the most difficult page in the guide to write, but we do feel it's important. Odds are, you've had problems you tried to solve and you've used another library to solve them. You're here because you want to know if Vue can solve your specific problems better. That's what we hope to answer for you.
这绝对是教程里最难写的一篇文章，但同时我们觉得非常重要。你很可能出于解决某些问题的需要，使用了其它框架。而你会看这篇文章是因为，你想知道 Vue 能不能更好地解决你的问题。所以我们希望在这里解答你的疑问。

We also try very hard to avoid bias. As the core team, we obviously like Vue a lot. There are some problems we think it solves better than anything else out there. If we didn't believe that, we wouldn't be working on it. We do want to be fair and accurate though. Where other libraries offer significant advantages, such as React's vast ecosystem of alternative renderers or Knockout's browser support back to IE6, we try to list these as well.
同时，我们会尽力做到客观公正。作为 Vue 的核心团队，我们当然非常喜欢 Vue。在解决某些问题时，我们认为 Vue 比其它任何方案都强。如果我们不相信这一点的话，我们就不会开发 Vue 了。不过我们确实希望做到公正而准确。所以，我们也会如实列出其它库的优点，比如 React 的庞大的生态系统和丰富的自定义渲染器，或者 Knockout 对于低至 IE6 的浏览器支持。

We'd also like **your** help keeping this document up-to-date because the JavaScript world moves fast! If you notice an inaccuracy or something that doesn't seem quite right, please let us know by [opening an issue](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).
我们还希望得到**你的**帮助，来让这篇文档持续更新，因为 JavaScript 发展得太快了！如果你看到文档中有任何纰漏，请在 Github 仓库[提交 issue](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide)。

## React

React and Vue share many similarities. They both:
React 和 Vue 有很多相似点。它们都：

- utilize a virtual DOM
- 应用了虚拟 DOM
- provide reactive and composable view components
- 提供了响应式，可组合的视图组件
- maintain focus in the core library, with concerns such as routing and global state management handled by companion libraries
- 专注内核，而将诸如路由和全局状态管理的问题，交给插件库来处理。

Being so similar in scope, we've put more time into fine-tuning this comparison than any other. We want to ensure not only technical accuracy, but also balance. We point out where React outshines Vue, for example in the richness of their ecosystem and abundance of their custom renderers.
因为 React 和 Vue 如此相似，所以我们花了最多的时间对它们进行细致的比较。我们不只想保证技术上的准确性，还想兼顾两者的优点。我们会指出 React 胜过 Vue 的地方，比如繁荣的生态系统，以及丰富的自定义渲染器。

The React community [has been instrumental](https://github.com/vuejs/vuejs.org/issues/364) in helping us achieve this balance, with special thanks to Dan Abramov from the React team. He was extremely generous with his time and considerable expertise to help us refine this document until we were [both happy](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244575740) with the final result.
React 社区[积极帮助](https://github.com/vuejs/vuejs.org/issues/364)我们来达到这种平衡，特别是 React 团队的 Dan Abramov。他非常慷慨地花费了大量时间精力来帮助我们完善这篇文档，直到两个社区对于最终结果都[非常满意](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244575740)。

With that said, we hope you can feel confident in the fairness of the review below as we explore the differences between these two libraries.
综上所述，你可以对本文的公正性感到放心。接下来，我们会讨论 React 和 Vue 之间的差异。

### 性能 (Performance Profiles)

In every real-world scenario that we've tested so far, Vue outperforms React by a fair margin. If your eyebrows are raising right now, read further. We'll breakdown why (and even include a benchmark developed in collaboration with the React team).
目前为止，在我们测试的每一个真实场景中，Vue 的性能都比 React 好上不少。如果你有所怀疑，请继续往下读。我们会告诉你具体的原因，甚至还包括了一个跟 React 团队一起开发的测试基准。

#### 渲染性能 (Render Performance)

When rendering UI, manipulating the DOM is typically the most expensive operation and unfortunately, no library can make those raw operations faster. The best we can do is:
在渲染用户界面时，操作 DOM 通常是最为耗费资源的。而且很遗憾的是，没有一个库可以将这些原生操作变得更快。但我们可以做到：

1. Minimize the number of necessary DOM mutations. Both React and Vue use virtual DOM abstractions to accomplish this and both implementations work about equally well.
1. 降低必要的 DOM 更新次数。React 和 Vue 都使用了虚拟 DOM 来做到这一点，在这个层面两者做得一样好。
2. Add as little overhead as possible on top of those DOM manipulations. This is an area where Vue and React differ.
2. 尽可能降低 DOM 操作的额外开销。这是 Vue 和 React 有所差异的地方。

In React, let's say the additional overhead of rendering an element is 1 and the overhead of an average component is 2. In Vue, the overhead of an element would be more like 0.1, but the overhead of an average component would be 4, due to the setup required for our reactivity system.
假设在 React 中，渲染一个元素的额外开销是 1， 而渲染一个组件的平均 额外开销是 2。那么在 Vue 中，渲染一个元素的额外开销大概是 0.1，而渲染一个组件的平均额外开销是 4，这是因为 Vue 的响应式系统需要做一些配置工作。

This means that in typical applications, where there are many more elements than components being rendered, Vue will outperform React by a significant margin. In extreme cases however, such as using 1 normal component to render each element, Vue will usually be slower. This isn't the end of the story though.
这意味着在典型的，渲染元素多于组件的应用中，Vue 的性能会远远好于 React。不过在极端情况下，比如只用一个普通组件来渲染每一个元素，Vue 通常会比 React 慢。让我们继续往下看。

Both Vue and React also offer functional components, which are stateless and instanceless - and therefore, require less overhead. When these are used in performance-critical situations, Vue is once again faster. To demonstrate this, we built a simple [benchmark project](https://github.com/chrisvfritz/vue-render-performance-comparisons) that just renders 10,000 list items 100 times. We encourage you to try it yourself, as the results will vary depending on the hardware and browser used - and actually, they'll vary even between runs due to the nature of JavaScript engines.
Vue 和 React 都支持函数式组件，这些组件是无状态而且不需要实例化的，因此额外开销会更小。在追求性能的场合使用函数式组件时，Vue 再一次胜出。我们构建了一个简单的[测试基准项目](https://github.com/chrisvfritz/vue-render-performance-comparisons)来展示这一点，这个项目会将 10000 个列表项渲染 100 次。我们鼓励你尝试在自己的机器上运行这个项目，因为它的结果会因硬件和浏览器的差异有所不同。实际上，哪怕在同样的机器和浏览器上运行，每次运行的结果也会有所不同，这个差异是 JavaScript 引擎本身所导致的。

If you're feeling lazy though, below are the numbers from one run in Chrome 52 on a 2014 MacBook Air. To avoid cherry-picking, both benchmarks were actually run 20 separate times, with results from the best runs included below:
但如果你不想自己动手，那你可以看一下，我们在一台 2014 产的 MacBook Air 上，用 Chrome 52 版本运行的结果。为了避免样本偏差，我们把两个库的代码都分别运行了 20 次：

{% raw %}
<table class="benchmark-table">
  <thead>
    <tr>
      <th></th>
      <th>Vue</th>
      <th>React</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>最快 (Fastest)</th>
      <td>23ms</td>
      <td>63ms</td>
    </tr>
    <tr>
      <th>中位数 (Median)</th>
      <td>42ms</td>
      <td>81ms</td>
    </tr>
    <tr>
      <th>平均数 (Average)</th>
      <td>51ms</td>
      <td>94ms</td>
    </tr>
    <tr>
      <th>第 95 百分位数 (95th Perc.)</th>
      <td>73ms</td>
      <td>164ms</td>
    </tr>
    <tr>
      <th>最慢 (Slowest)</th>
      <td>343ms</td>
      <td>453ms</td>
    </tr>
    </tr>
  </tbody>
</table>
{% endraw %}

#### 更新性能 (Update Performance)

In React, you need to implement `shouldComponentUpdate` everywhere and use immutable data structures to achieve fully optimized re-renders. In Vue, a component's dependencies are automatically tracked so that it only updates when one of those dependencies change. The only further optimization that sometimes can be helpful in Vue is adding a `key` attribute to items in long lists.
在 React 里，要彻底优化视图更新，你需要在各种地方实现 `shouldComponentUpdate`，并且使用不可变数据结构。而 Vue 会自动跟踪组件依赖，因此只有在依赖变化时，才会更新视图。Vue 中唯一可能进一步优化的地方，就是给长列表的项添加 `key` 属性。

This means updates in unoptimized Vue will be much faster than unoptimized React and actually, due to the improved render performance in Vue, even fully-optimized React will usually be slower than Vue is out-of-the-box.
这意味着，在未优化的情况下，Vue 的视图更新比 React 更快。实际上，因为 Vue 本身已经对渲染性能进行了优化，所以开箱即用的 Vue 代码也比彻底优化过的 React 代码要更快。

#### 开发环境中的性能 (In Development)

Obviously, performance in production is the most important and that's what we've been discussing so far. Performance in development still matters though. The good news is that both Vue and React remain fast enough in development for most normal applications.
显然，在生产环境中，性能是最重要的考量，这也是为什么我们一直在聊性能的原因。而在开发环境中，性能也同样重要。好消息是，对于大多数普通应用，Vue 和 React 在开发环境中的性能都足够好。

However, if you're prototyping any high-performance data visualizations or animations, you may find it useful to know that in scenarios where Vue can't handle more than 10 frames per second in development, we've seen React slow down to about 1 frame per second.
不过，如果你正在构建对性能要求很高的数据可视化或者动画原型，在某些情况下，Vue 最多只能达到 10 帧/秒， 而 React 甚至只有 1 帧/秒。

This is due to React's many heavy invariant checks, which help it to provide many excellent warnings and error messages. We agree that these are important in Vue, but have tried to keep a closer eye on performance while we implement these checks.
这是因为 React 做了大量的条件检查，这让 React 能提供大量有用的警告和错误信息。虽然我们认为这对 Vue 也同样重要，但我们在实现检查机制时，把更多的注意力放在了性能上。

### HTML 和 CSS (HTML & CSS)

In React, everything is Just JavaScript, which sounds very simple and elegant - until you dig deeper. The unfortunate reality is that reinventing HTML and CSS within JavaScript can cause a lot of pain. In Vue, we instead embrace web technologies and build on top of them. To show you what that means, we'll dive into some examples.
在 React 中，一切都只是 JavaScript，听起来简单而优雅，不过那只是在你深入了解它之前。现实是，用 JavaScript 去重构 HTML 和 CSS 会非常蛋疼。相对地，Vue 选择了拥抱现有的 web 技术，在它们的基础上去构建。我们会用一些例子来解释这是什么意思。

#### JSX vs Templates

In React, all components express their UI within render functions using JSX, a declarative XML-like syntax that works within Javascript. Here's an example, [vetted by the React community](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244582684):
在 React 里，所有的组件都会在 render 函数中使用 JSX 来表现用户界面。JSX 是一个在 JavaScript 中类似 XML 的声明式语法。可以看看这段由 [React 社区](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244582684) 贡献的示例代码。


``` jsx
render () {
  let { items } = this.props

  let children
  if (items.length > 0) {
    children = (
      <ul>
        {items.map(item =>
          <li key={item.id}>{item.name}</li>
        )}
      </ul>
    )
  } else {
    children = <p>No items found.</p>
  }

  return (
    <div className='list-container'>
      {children}
    </div>
  )
}
```

Render functions with JSX have a few advantages:
将 render 函数和 JSX 一起使用有以下优点：

- You can use the power of a full programming language (JavaScript) to build your view.
- 你能够用一个完整的编程语言 (JavaScript) 去构建你的视图。
- The tooling support (e.g. linting, type checking, editor autocompletion) for JSX is in some ways more advanced than what's currently available for Vue templates.
- 在某些方面，JSX 的工具支持（比如 linting，类型检查，编辑器的自动补全）比现在的 Vue 模版更好。

In Vue, we also have [render functions](render-function.html) and even [support JSX](render-function.html#JSX), because sometimes you need that power. Render functions are not recommended for most components however.
Vue 也提供 [render 函数](render-function.html)，甚至还 [支持 JSX](render-function.html#JSX)，因为有时候你的确需要用到。不过，对于大部分组件，我们不推荐使用 render 函数。

Instead, we offer templates as a simpler alternative:
取而代之地，我们提供了模版，作为更简单的替代方案：

``` html
<template>
  <div class="list-container">
    <ul v-if="items.length">
      <li v-for="item in items">
        {{ item.name }}
      </li>
    </ul>
    <p v-else>No items found.</p>
  </div>
</template>
```

优点如下：

- Many fewer implementation and stylistic decisions have to be made while writing a template
- 在编写模版时，不需要考虑太多实现和风格的细节
- A template will always be declarative
- 模版永远是声明式的
- Any valid HTML is valid in a template
- 模版完全兼容 HTML，任何合法的 HTML 在模版中也是合法的
- It reads more like English (e.g. for each item in items)
- 更符合英语阅读习惯 （比如 for each item in items）
- Advanced versions of JavaScript are not required to increase readability
- 不需要高级版本的 JavaScript 来增加代码的可读性

This is not only much easier for the developer that's writing it, but designers and less experienced developers will also find it much easier parsing and contributing code.
这不仅仅让工程师更加轻松，而且让设计师和新手工程师也更容易阅读和贡献代码。

It doesn't end there though. By embracing HTML rather than trying to reinvent it within JavaScript, Vue also allows you to use preprocessors such as Pug (formerly known as Jade) in your templates.
还有，因为 Vue 拥抱 HTML 而不是用 JavaScript 来重新发明轮子，所以你可以在模版中使用像 Pug (曾经的 Jade) 这样的预处理器。

The React ecosystem also has [a project](https://wix.github.io/react-templates/) that allows you to write templates, but there are a few disadvantages:
React 的生态系统里也有一个让你可以写模版的 [项目](https://wix.github.io/react-templates/)，不过它有以下缺陷：

- It's not nearly as feature-rich as Vue's templating system
- 功能不如 Vue 的模版系统那样丰富
- It requires separating your HTML from component files
- HTML 和组件必需用不同文件来分离
- Because it's a 3rd party library rather than officially supported, it may or may not be kept up-to-date with React core into the future
- 因为它是第三方库而不是官方库，未来不一定会与 React 内核保持一致

#### 组件中的 CSS (Component-Scoped CSS)

Unless you spread components out over multiple files (for example with [CSS Modules](https://github.com/gajus/react-css-modules)), scoping CSS in React comes with caveats. Very basic CSS works great out-of-the-box, but some more complex features such as hover states, media queries, and pseudo-selectors all either require heavy dependencies to reinvent what CSS already does - or they simply don't work.
除非你将组件分布在不同的文件（比如用 [CSS Modules](https://github.com/gajus/react-css-modules)），否则在 React 中限定 CSS 的作用域会产生警告。基本的 CSS 没什么问题，但一些复杂一点的特性，比如悬停状态，媒体查询，伪类选择器等，要么需要大量的依赖来重造 CSS，要么就直接无法使用。

Vue on the other hand, gives you full access to CSS within [single-file components](single-file-components.html):
而 Vue 可以让你在 [单文件组件](single-file-components.html) 中完全掌控 CSS 的作用域：

``` html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

The optional `scoped` attribute automatically scopes this CSS to your component by adding a unique attribute (such as `data-v-1`) to elements and compiling `.list-container:hover` to something like `.list-container[data-v-1]:hover`.
可选的 `scoped` 属性会自动将 CSS 局限在你的组件里，它会给元素加上一个唯一的属性（比如 `data-v-1`），然后将 `.list-container:hover` 编译成类似 `.list-container[data-v-1]:hover` 的代码。

Finally, just as with HTML, you also have the option of writing your CSS using any preprocessors (or postprocessors) you'd like. This allows you to perform design-centric operations such as color manipulation during your build process, rather than importing specialized JavaScript libraries that would increase the size of your build and complexity of your application.
最后，就像 HTML 一样，你也可以选择用预处理器（或者后处理器）来编写你的 CSS。这样你就可以在构建阶段做一些设计方面的操作，比如选择色彩，而不需要引入专门的 JavaScript 库。这样可以减小构建的体积，降低应用的复杂度。

### 规模 (Scale)

#### 大规模使用 (Scaling Up)

For large applications, both Vue and React offer robust routing solutions. The React community has also been very innovative in terms of state management solutions (e.g. Flux/Redux). These state management patterns and [even Redux itself](https://github.com/egoist/revue) can be easily integrated into Vue applications. In fact, Vue has even taken this model a step further with [Vuex](https://github.com/vuejs/vuex), an Elm-inspired state management solution that integrates deeply into Vue that we think offers a superior development experience.
Vue 和 React 都对大型应用提供了健壮的路由解决方案。React 社区在状态管理方面的解决方案（比如 Flux/Redux）也非常创新。这些状态管理的模式以及 [Redux 本身](https://github.com/egoist/revue) 都可以轻松地集成在 Vue 应用中。实际上，Vue 更进一步地在 [Vuex](https://github.com/vuejs/vuex) 中采用了这个模型，它受到 Elm 启发，深度地集成到 Vue 中，我们相信它提供了一个更好的开发体验。

Another important difference between these offerings is that Vue's companion libraries for state management and routing (among [other concerns](https://github.com/vuejs)) are all officially supported and kept up-to-date with the core library. React instead chooses to leave these concerns to the community, creating a more fragmented ecosystem. Being more popular though, React's ecosystem is considerably richer than Vue's.
另一个重要的差异是，Vue 的这些用于状态管理和路由的插件库官方支持的，保证和 Vue 内核保持一致。而 React 选择了把这些问题交给社区，从而创造了一个更为碎片化的生态系统。不过也因为这样 React 生态系统比 Vue 更为多样化。

Finally, Vue offers a [CLI project generator](https://github.com/vuejs/vue-cli) that makes it trivially easy to start a new project using your choice of build system, including [Webpack](https://github.com/vuejs-templates/webpack), [Browserify](https://github.com/vuejs-templates/browserify), or even [no build system](https://github.com/vuejs-templates/simple). React is also making strides in this area with [create-react-app](https://github.com/facebookincubator/create-react-app), but it currently has a few limitations:
最后，Vue 提供了一个[生成项目的命令行工具](https://github.com/vuejs/vue-cli)，让你可以根据不同构建系统来轻松创建新项目，支持 [Webpack](https://github.com/vuejs-templates/webpack)，[Browserify](https://github.com/vuejs-templates/browserify)，甚至是 [无构建系统](https://github.com/vuejs-templates/simple)。React 也在这方面迈出了一大步，具体可以参考 [create-react-app 项目](https://github.com/facebookincubator/create-react-app)，不过目前它还有以下缺点：

- It does not allow any configuration during project generation, while Vue's project templates allow Yeoman-like customization.
- 它不支持项目生成阶段的配置，而 Vue 的项目模版允许像类似 Yeoman 的自定义选项。
- It only offers a single template that assumes you're building a single-page application, while Vue offers a wide variety of templates for various purposes and build systems.
- 它只提供了一个单一的模版，并且这个模版会假设你是在构建一个单页应用。而 Vue 则提供了种类繁多的模版，让你在不同的构建系统和情境下使用。
- It cannot generate projects from user-built templates, which can be especially useful for enterprise environments with pre-established conventions.
- 它无法从用户创建的模版生成项目，而这个支持这对于有既成约定的企业环境来说额外重要。

It's important to note though that many of these limitations are intentional design decisions made by the create-react-app team and they do have their advantages. For example, as long your project's needs are very simple and you never need to "eject" to customize your build process, you'll be able to update it as a dependency. You can read more about the [differing philosophy here](https://github.com/facebookincubator/create-react-app#philosophy).
不过要注意，这些局限很多都是 create-react-app 团队有意为之的，而这样的设计有它的优点。比如，如果你的项目需求非常简单，而你不需要跳出限制来自定义你的构建过程，可以像更新依赖一样更新这个工具。你可以在这里阅读一下这套[不一样的设计思路](https://github.com/facebookincubator/create-react-app#philosophy)。

#### 小规模使用 (Scaling Down)

React is renowned for its steep learning curve. Before you can really get started, you need to know about JSX and probably ES2015+, since many examples use React's class syntax. You also have to learn about build systems, because although you could technically use Babel Standalone to live-compile your code, it's not recommended for production.
React 难上手是出了名的。在你能够开始之前，你需要先了解 JSX，可能还要一点 ES2015+，因为许多 React 的示例代码都使用了 class 语法。你还需要学习构建系统，虽然技术上你可以只使用 Babel 来热编译你的代码，但在生产环境这并不是很好的做法。

While Vue scales up just as well as, if not better than React, it also scales down just as well as jQuery. That's right - all you have to do is drop a single script tag into a page:
如果说在大规模使用时，Vue 做的跟 React 一样好，那么小规模使用时 Vue 可以做得像 jQuery 一样好。没错，你只需要把一个 script 标签加到页面中就可以了：

``` html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

Then you can start writing Vue code and even ship the minified version to production without feeling guilty or having to worry about performance problems.
然后你就可以开始写 Vue 代码，甚至可以把最小化的版本直接推到生产环境，完全不需要担心性能问题。

Since you don't need to know about JSX, ES2015, or build systems to get started with Vue, it also typically takes developers less than a day reading [the guide](/guide) to learn enough to build non-trivial applications.
要上手 Vue，你不需要了解 JSX，ES2015 或者构建系统。，通常你只需要花不到一天来阅读 [教程](/guide)，就可以开始写一个实际应用。

### 原生渲染 (Native Rendering)

ReactNative enables you to write native-rendered apps for iOS and Android using the same React component model. This is great in that as a developer, you can apply your knowledge of a framework across multiple platforms. On this front, Vue has an official collaboration with [Weex](https://alibaba.github.io/weex/), a cross-platform UI framework developed by Alibaba Group, which uses Vue as its JavaScript framework runtime. This means with Weex, you can use the same Vue component syntax to author components that can not only be rendered in the Browser, but also natively on iOS and Android!
ReactNative 让你可以使用同样的 React 组件模型，编写原生渲染的 iOS 和 Android 应用。这对于开发者来说很棒，因为你可以用一个框架写跨平台的应用。在这一点上，Vue 已经和阿里巴巴的 [Weex](https://alibaba.github.io/weex/) 达成官方合作，Weex 会使用 Vue 来作为 JavaScript 的框架运行时。这意味着你可以在 Weex 中使用 Vue 的语法来编写部件，这些部件不仅可以在浏览器上运行，也可以在 iOS 和 Android 平台上以原生的方式运行。

At this moment, Weex is still in active development and is not as mature and battle-tested as ReactNative, but its development is driven by the production needs of the largest e-commerce business in the world, and the Vue team will also actively collaborate with the Weex team to ensure a smooth experience for Vue developers.
在写这篇文章的时候，Weex 仍然处于活跃的开发状态，还没有达到 ReactNative 的成熟度，不过它的开发是由世界上最大的电子商务公司的生产需求所驱动的，而 Vue 团队也会积极地和 Weex 团队合作，确保为开发者带来流畅的体验。

### 与 MobX 一起使用 (With MobX)

MobX has become quite popular in the React community and it actually uses a nearly identical reactivity system to Vue. To a limited extent, the React + MobX workflow can be thought of as a more verbose Vue, so if you're using that combination and are enjoying it, jumping into Vue is probably the next logical step.
MobX 在 React 社区中已经相当流行，而且它使用了几乎跟 Vue 一摸一样的响应式系统。在某种程度上，可以把 React + Mobx 当作一个繁琐版的 Vue。所以如果你觉得这个组合用得很爽，那么使用 Vue 是一个很自然的选择。

## Angular 1

Some of Vue's syntax will look very similar to Angular (e.g. `v-if` vs `ng-if`). This is because there were a lot of things that Angular got right and these were an inspiration for Vue very early in its development. There are also many pains that come with Angular however, where Vue has attempted to offer a significant improvement.
Vue 的一些语法看起来和 Angular 很类似（比如 `v-if` 和 `ng-if`）。这是因为 Angular 有很多做的很好的地方，而 Vue 在早期开发阶段从 Angular 获得了很多灵感。不过，使用 Angular 也有很多蛋疼的地方，而 Vue 在这些方面提供了大幅度的改善。

### 复杂性 (Complexity)

Vue is much simpler than Angular 1, both in terms of API and design. Learning enough to build non-trivial applications typically takes less than a day, which is not true for Angular 1.
无论是 API 还是设计思路，Vue 比 Angular 1 都要简单得多。使用 Vue 构建一个实际应用，只需要花不到一天的时间来学习，而 Angular 1 就不行了。

### 灵活性和模块化 (Flexibility and Modularity)

Angular 1 has strong opinions about how your applications should be structured, while Vue is a more flexible, modular solution. While this makes Vue more adaptable to a wide variety of projects, we also recognize that sometimes it's useful to have some decisions made for you, so that you can just get started coding.
Angular 1 对于你的项目结构有着很强的制约，而 Vue 则更为灵活且模块化。虽然这让 Vue 可以适应很多不同类型的项目，不过我们也意识到，有时候开发者不希望考虑太多，只是想直接开始写代码。

That's why we offer a [Webpack template](https://github.com/vuejs-templates/webpack) that can set you up within minutes, while also granting you access to advanced features such as hot module reloading, linting, CSS extraction, and much more.
这就是为什么我们提供了一个 [Webpack 模版](https://github.com/vuejs-templates/webpack)，它可以在几分钟之内帮你配置好环境，同时提供了模块热加载，linting，CSS 提取等高级功能。

### 数据绑定 (Data binding)

Angular 1 uses two-way binding between scopes, while Vue enforces a one-way data flow between components. This makes the flow of data easier to reason about in non-trivial applications.
Angular 1 在不同域之间采用双向绑定，而 Vue 强制在组件之间使用单向数据流。这样会让应用的数据流更加清晰。

### 指令和组件 (Directives vs Components)

Vue has a clearer separation between directives and components. Directives are meant to encapsulate DOM manipulations only, while components are self-contained units that have their own view and data logic. In Angular, there's a lot of confusion between the two.
Vue 清楚地区分了指令和组件。指令是用来封装 DOM 操作的，而组件是独立的存在，包含了自己的视图和数据逻辑。在 Angular 中，这两者经常会混在一起。

### 性能 (Performance)

Vue has better performance and is much, much easier to optimize because it doesn't use dirty checking. Angular 1 becomes slow when there are a lot of watchers, because every time anything in the scope changes, all these watchers need to be re-evaluated again. Also, the digest cycle may have to run multiple times to "stabilize" if some watcher triggers another update. Angular users often have to resort to esoteric techniques to get around the digest cycle, and in some situations, there's simply no way to optimize a scope with many watchers.
Vue 有着更好的性能，并且更容易优化，因为 Vue 没有使用脏检查。Angular 1 在有大量监视器的时候会变的很慢，因为每一次在域内的变化，都会导致所有的监视器重新计算。而且，如果某些监视器触发了其它更新，那么 digest cycle 要多次计算才能得出最终的结果。Angular 的开发者经常需要用到一些黑魔法来绕开 digest cycle，而在某些情况下，根本没有办法对拥有多个监视器的域进行优化。

Vue doesn't suffer from this at all because it uses a transparent dependency-tracking observation system with async queueing - all changes trigger independently unless they have explicit dependency relationships.
而 Vue 就没有这个问题，因为 Vue 使用了一个透明的，基于异步队列的依赖跟踪观察系统，所有的变化都会独立触发，除非它们有明确的依赖关系。

Interestingly, there are quite a few similarities in how Angular 2 and Vue are addressing these Angular 1 issues.
有趣的是，Angular 2 和 Vue 用了不少相似的方式，来解决这些 Angular 1 的问题。

## Angular 2

We have a separate section for Angular 2 because it really is a completely new framework. For example, it features a first-class component system, many implementation details have been completely rewritten, and the API has also changed quite drastically.
我们给 Angular 2 一个单独的篇幅，因为它是一个跟 Angular 1 完全不同的框架。比如，它具备一流的组件系统，许多实现细节被彻底重写，API 也发生了大幅度的改变。

### TypeScript

While Angular 1 could be used for smaller applications, Angular 2 has shifted focus to best facilitate large enterprise applications. As part of this, it almost requires TypeScript, which can be very useful for developers that desire the type safety of languages such as Java and C#.
虽然 Angular 1 还能用于构建一些小型应用，Angular 2 已经把注意力放在支持大型的企业级应用上。出于这个原因，Typescript 几乎变成了 Angular 2 的必需品，它对于那些渴望类型安全机制（比如 Java 和 C#）的开发者非常有用。

Vue is also well-suited to [enterprise environments](https://github.com/vuejs/awesome-vue#enterprise-usage) and can even be used with TypeScript via our [official typings](https://github.com/vuejs/vue/tree/dev/types) and [user-contributed decorators](https://github.com/itsFrank/vue-typescript), though it's definitely optional in our case.
Vue 对于[企业环境](https://github.com/vuejs/awesome-vue#enterprise-usage)也十分适用，你甚至可以通过[官方的类型声明](https://github.com/vuejs/vue/tree/dev/types) 和 [用户贡献的装饰符](https://github.com/itsFrank/vue-typescript)，来和 TypeScript 搭配使用。不过 TypeScript 对于 Vue 来说不是必需的。

### 文件大小和性能 (Size and Performance)

In terms of performance, both frameworks are exceptionally fast and there isn't enough data from real world use cases to make a verdict. However if you are determined to see some numbers, Vue 2.0 seems to be ahead of Angular 2 according to this [3rd party benchmark](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html).
从性能的角度，两个框架都非常快，目前我们并没有足够的数据来说明这两者之间的差异。如果你确实想看一些这方面的数据，根据这个 [第三方的测试基准](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html)，Vue 2.0 好像的确胜过 Angular 2。

Size wise, although Angular 2 with offline compilation and tree-shaking is able to get its size down considerably, a full-featured Vue 2.0 with compiler included (23kb) is still lighter than a tree-shaken bare-bone example of Angular 2 (50kb). And do note the Angular 2 app's size is small due to tree-shaking, which removes code for features that you are not using. It will eventually grow back to its actual size as you import and use more features from the framework.
从文件大小的角度，虽然 Angular 2 用离线编译和 tree-shaking 可以大大降低文件大小。但一个包含了编译器的完整的 Vue 2.0 (23kb) 依然比 Angular 2 (50kb) 要小。但是要注意，Angular 2 的体积缩小是因为 tree-shaking，这意味着你没有用到的代码会被删除掉。如果你导入且使用了更多的 Angular 2 特性，它最终会变回原来的大小。

### 灵活性 (Flexibility)

Vue is much less opinionated than Angular 2, offering official support for a variety of build systems, with no restrictions on how you structure your application. Many developers enjoy this freedom, while some prefer having only one Right Way to build any application.
Vue 相对 Angular 2 而言更加中立，它为不同的构建系统提供了官方支持，而且没有对你的项目结构提出任何限制。许多开发者享受这种自由度，但同时也有一些开发者喜欢“只有一种正确做法”的开发方式。

### 学习曲线 (Learning Curve)

To get started with Vue, all you need is familiarity with HTML and ES5 JavaScript (i.e. plain JavaScript). With these basic skills, you can start building non-trivial applications within less than a day of reading [the guide](/guide).
要上手 Vue，你只需要熟悉 HTML 和 ES5 （也就是普通的 JavaScript）。有了这些基本知识，花上不到一天来阅读 [教程](/guide)，你就可以开始开发了。

Angular 2's learning curve is much steeper. Even without TypeScript, their [Quickstart guide](https://angular.io/docs/js/latest/quickstart.html) starts out with an app that uses ES2015 JavaScript, NPM with 18 dependencies, 4 files, and over 3,000 words to explain it all - just to say Hello World. It's an understatement to say that [Vue's Hello World](index.html#Hello-World) is considerably simpler. It's so trivial in fact, that we don't even dedicate a whole page in the guide to it.
Angular 2 的学习曲线则要陡峭的多。即使是有 TypeScript 的帮助，它们的 [上手教程](https://angular.io/docs/js/latest/quickstart.html) 也还是使用了 ES2015，拥有 18 个 NPM 依赖，4个文件，以及超过 3000 个英文单词来解释，而这仅仅是为了写 Hello World 程序而已。相比之下，[Vue 的 Hello World 程序](index.html#Hello-World) 就超级简单，我们甚至都没有花费教程里的篇幅来介绍它。

## Ember

Ember is a full-featured framework that is designed to be highly opinionated. It provides a lot of established conventions and once you are familiar enough with them, it can make you very productive. However, it also means the learning curve is high and flexibility suffers. It's a trade-off when you try to pick between an opinionated framework and a library with a loosely coupled set of tools that work together. The latter gives you more freedom but also requires you to make more architectural decisions.
Ember 是一个全能框架，有着强烈的开发主张。它提出了很多约定，如果你对它们足够熟悉，这会让你可以非常高效地进行开发。不过，这也意味着它的学习曲线非常陡峭，而且灵活性不高。在一个具有强烈主张的框架（Ember），和一系列松散耦合的工具（Vue）之间做出选择时，你必需进行权衡。后者给你更多的自由度，但是需要自己做更多的架构上的决定。

That said, it would probably make a better comparison between Vue core and Ember's [templating](https://guides.emberjs.com/v2.7.0/templates/handlebars-basics/) and [object model](https://guides.emberjs.com/v2.7.0/object-model/) layers:
也就是说，我们最好来比较一下 Vue 的内核，以及 Ember 的 [模版](https://guides.emberjs.com/v2.7.0/templates/handlebars-basics/) 与 [对象模型](https://guides.emberjs.com/v2.7.0/object-model/) 层：

- Vue provides unobtrusive reactivity on plain JavaScript objects and fully automatic computed properties. In Ember, you need to wrap everything in Ember Objects and manually declare dependencies for computed properties.
- Vue 以一种自然的方式，在普通 JavaScript 对象上提供了响应式特性。而在 Ember 里面，你需要将所有东西都封装成 Ember 对象，手动声明计算属性的依赖。

- Vue's template syntax harnesses the full power of JavaScript expressions, while Handlebars' expression and helper syntax is intentionally quite limited in comparison.
- Vue 的模版语法充分利用了 JavaScript 表达式的能力，而相比之下， Hnadlebar 则有意限制表达式和 helper 语法。

- Performance-wise, Vue outperforms Ember by a fair margin, even after the latest Glimmer engine update in Ember 2.0. Vue automatically batches updates, while in Ember you need to manually manage run loops in performance-critical situations.
- 从性能的角度，Vue 远胜 Ember，即使在 Ember 2.0 最新的 Glimmer 引擎更新之后。Vue 会自动地批量处理更新，而在追求性能的场景下， Ember 需要你手动管理运行循环。

## Knockout

Knockout was a pioneer in the MVVM and dependency tracking spaces and its reactivity system is very similar to Vue's. Its [browser support](http://knockoutjs.com/documentation/browser-support.html) is also very impressive considering everything it does, with support back to IE6! Vue on the other hand only supports IE9+.
Knockout 是 MVVM 和依赖跟踪的先驱，它的响应式系统和 Vue 十分相似。考虑到它提供的功能，Knockout 的[浏览器支持](http://knockoutjs.com/documentation/browser-support.html) 可以说相当的出色，甚至还支持 IE6！而 Vue 则只支持 IE9+。

Over time though, Knockout development has slowed and it's begun to show its age a little. For example, its component system lacks a full set of lifecycle hooks and although it's a very common use case, the interface for passing children to a component feels a little clunky compared to [Vue's](components.html#Content-Distribution-with-Slots).
不过随着时间推移，Knockout 的开发速度已经慢了下来。比如，它的组件系统缺少一整套生命周期钩子，传递子组件的接口相对于 [Vue](components.html#Content-Distribution-with-Slots) 来说也非常笨重，即使它经常会被用到。

There also seem to be philosophical differences in the API design which if you're curious, can be demonstrated by how each handles the creation of a [simple todo list](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). It's definitely somewhat subjective, but many consider Vue's API to be less complex and better structured.
在 API 设计上两者也有一些哲学上的差异，你可以在下面这个[简单 todo 列表](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89)的示例代码里看到这些差异。虽然有点主观，但很多人都认为 Vue 的 API 要更简单优雅。

## Polymer

Polymer is yet another Google-sponsored project and in fact was a source of inspiration for Vue as well. Vue's components can be loosely compared to Polymer's custom elements and both provide a very similar development style. The biggest difference is that Polymer is built upon the latest Web Components features and requires non-trivial polyfills to work (with degraded performance) in browsers that don't support those features natively. In contrast, Vue works without any dependencies or polyfills down to IE9.
Polymer 是另一个 Google 赞助的项目，实际上它也是 Vue 的一个灵感来源。Vue 的组件可以跟 Polymer 的自定义元素粗略类比，两者都有类似的开发风格。最大的区别在于 Polymer 是基于 Web Components 特性，在还没有支持这个特性的浏览器上，需要引入一个不小的 polyfill，降低了它的性能。相比之下，Vue 不需要任何依赖和 polyfill，就可以在 IE9+ 的浏览器上运行。

In Polymer 1.0, the team has also made its data-binding system very limited in order to compensate for the performance. For example, the only expressions supported in Polymer templates are boolean negation and single method calls. Its computed property implementation is also not very flexible.
在 Polymer 1.0中，出于性能考虑，开发团队将数据绑定系统设计得非常局限。比如，在 Polymer 模版中的表达式只支持布尔值的否操作和普通的函数调用。它对于计算属性的实现也比较死板。

Polymer custom elements are authored in HTML files, which limits you to plain JavaScript/CSS (and language features supported by today's browsers). In comparison, Vue's single file components allows you to easily use ES2015+ and any CSS preprocessors you want.
Polymer 的自定义元素是在 HTML 文件里面编写的，所以你只能用普通的 JavaScript/CSS （以及浏览器所支持的属性）。而 Vue 的单文件组件让你可以轻松地使用 ES2015+ 以及你所偏好的 CSS 预编译器。

When deploying to production, Polymer recommends loading everything on-the-fly with HTML Imports, which assumes browsers implementing the spec, and HTTP/2 support on both server and client. This may or may not be feasible depending on your target audience and deployment environment. In cases where this is not desirable, you will have to use a special tool called Vulcanizer to bundle your Polymer elements. On this front, Vue can combine its async component feature with Webpack's code-splitting feature to easily split out parts of the application bundle to be lazy-loaded. This ensures compatibility with older browsers while retaining great app loading performance.
在部署到生产环境时，Polymer 推荐你使用 HTML 导入加载所有资源，但浏览器必需支持相关的特性，而且浏览器和客户端都要支持 HTTP/2。考虑到目标用户和部署环境，有时候这是不现实的。在这种情况下，你需要用到一个叫做 Vulcanizer 的特殊工具来构建你的 Polymer 元素。而在这个问题上，Vue 可以用异步组件特性配合 Webpack 的代码分割特性，将打包的应用分块，实现懒加载。这既兼容了老式浏览器，又保证了应用加载的性能。

It is also totally feasible to offer deeper integration between Vue with Web Component specs such as Custom Elements and Shadow DOM style encapsulation - however at this moment we are still waiting for the specs to mature and be widely implemented in all mainstream browsers before making any serious commitments.
Vue 完全可以更深入地和 Web Component 标准融合，比如自定义元素和影子 DOM 这样的封装 - 不过我们会等到这个标准成熟，并且主流浏览器广泛实现这个标准之后，再去认真地投入精力。

## Riot

Riot 2.0 provides a similar component-based development model (which is called a "tag" in Riot), with a minimal and beautifully designed API. Riot and Vue probably share a lot in design philosophies. However, despite being a bit heavier than Riot, Vue does offer some significant advantages:
Riot 2.0 提供了一个跟 Vue 类似的基于组件的开发模型（在 Riot 中被称为 "tag"），而且它还有一个小巧优雅的 API。Riot 和 Vue 在设计思路上又很多相同的地方。虽然 Vue 比 Riot 要更重一些，但 Vue 有一些明显的优势：

- True conditional rendering. Riot renders all if branches and simply shows/hides them.
- Vue 提供了真正的条件渲染。而 Riot 渲染了所有的条件分支，只是将其它们选择性地显示和隐藏。
- A far more powerful router. Riot’s routing API is extremely minimal.
- Vue 有一个更加强大的路由系统。Riot 的路由 API 真的是非常局限。
- More mature tooling support. Vue provides official support for [Webpack](https://github.com/vuejs/vue-loader), [Browserify](https://github.com/vuejs/vueify), and [SystemJS](https://github.com/vuejs/systemjs-plugin-vue), while Riot relies on community support for build system integration.
- Vue 的工具支持更加成熟。它对 [Webpack](https://github.com/vuejs/vue-loader)，[Browserify](https://github.com/vuejs/vueify)，和 [SystemJS](https://github.com/vuejs/systemjs-plugin-vue)提供了官方支持。而 Riot 对构建系统的支持依赖于它的社区。
- [Transition effect system](transitions.html). Riot has none.
－ Vue 有[过渡效果系统](transitions.html)，而 Riot 还没有。
- Better performance. [Despite advertising](https://github.com/vuejs/vuejs.org/issues/346) use of a virtual DOM, Riot in fact uses dirty checking and thus suffers from the same performance issues as Angular 1.
- Vue 有更好的性能。尽管 Riot [对外宣称](https://github.com/vuejs/vuejs.org/issues/346) 使用了虚拟 DOM，但它实际上还是使用了脏检查机制，所以跟 Angular 1 有着同样的性能问题。
