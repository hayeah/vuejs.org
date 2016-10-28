# Vue.js 2.0 翻译

这个厂库只用来协作翻译

[vue2 文档厂库](https://github.com/hayeah/vuejs.org)

## 翻译规范（参考了 [vue 1.0 翻译规范](https://github.com/vuejs/cn.vuejs.org/blob/lang-zh/CONTRIBUTING.md#翻译) ）

* 使用中文的符号
* 汉字，字母，数字等之间以空格隔开。
* 专有词注意大小写，如 HTML，CSS，JavaScript。
* 代码只翻译注释。

[阮一峰关于中文技术文档的规范](https://github.com/ruanyf/document-style-guide)

## 翻译字典（更新中）

英文 | 中文 | 备注 |
---- | ---- | ---- |
(single) source of truth | (单一)数据源
attribute | 属性 | 主体： DOM 元素
benchmark | 测试基准
companion library | 插件库
component | 组件
console | 控制台
core | 内核
declarative | 声明式
directive | 指令
example | 示例
fallback content | 替换内容 | 根据 MDN 文档
filter | 过滤器
for example | 例如
functional | 函数式 | e.g. `functional component` ：函数式组件
guide | 教程
incrementally adoptable | 增量式使用（转为动词）
interpolation | 插值
item | 项 | e.g. 列表项，数组项
learning curve | 学习曲线
listener, listen | 侦听器；侦听
mutation | 突变
non-trivial app | 正常应用
overhead | 额外开销
parent scope | 父级作用域
progressive | 渐进式
prop | 属性 | 主体：Vue 组件
property | 属性 | 主体：javascript 对象
reactive | 响应式的
render function | 渲染函数
see also | 另见
single page application, spa | 单页应用
transition effect | 过渡动画
transition | 过渡动画 | [小右的文章](http://www.html-js.com/article/1833)
virtual DOM | 虚拟 DOM
wrap | 包裹

参考资料：
- [小右的专栏](http://www.html-js.com/article/column/99)
- [小右的另一个专栏](http://www.zhihu.com.question.39879941.cmiiu.com/?people/evanyou/posts)
- [小右对 Vue 2 的介绍@Qcon](http://www.infoq.com/cn/articles/vue-2-progressive-front-end-solution)
- [vuejs-1.0 中文文档](http://cn.vuejs.org/)
 
## 协作形式

翻译者：

* 请从标记为 `待领取` 的 issues 中领取任务，写什么时候能完成。
* 如果超过预估时间无法完成请在 issue 中 at 协调员，说明接替者信息或请协调员重新分配该任务；
* 完成任务后提交 PR
* 若 PR 被标记为 `待修改`，根据反馈修改后删除标记，分配给协调员
* 请保留英文原文，方便校对。

协调员：[@hymRedemption](https://github.com/hymRedemption)

* 将无状态的 PR 分配给校对员，标记为待校对
* 若 `待合并` 的 PR 可以合并，则直接合并。合并后在 README 更新进度表，关闭相关的 issues
* 若 `待合并` 的 PR 不能合并，分配给 PR 提交者并将 PR 标记为 `待修改`

校对员: [@Egoist](https://github.com/egoist), [@mrwiredancer](https://github.com/Mr-Wiredancer)

* 对 `待校对` 的 PR 进行校对
* 校对通过则将 PR 改为`待合并`，并分配给协调员
* 校对不通过则将 PR 改为 `待修改`，并分配给 PR 提交者

## 进度表 (最后更新：2016-10-28 15:22)

文章 | 翻译 | 校对 | 状态 |
---- | ---- | ---- | ---- |
api/index#Global Config | [mrwiredancer](https://github.com/Mr-Wiredancer) | egoist | ~~已完成~~
api/index#Global API | pandazki |[mrwiredancer](https://github.com/Mr-Wiredancer) |  ~~已完成~~
api/index#Options/Data |[milkmeowo](https://github.com/milkmeowo)| egoist| ~~已完成~~
api/index#Options/DOM |[milkmeowo](https://github.com/milkmeowo)| [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
api/index#Options/Lifecycle Hooks |[milkmeowo](https://github.com/milkmeowo)| hayeah |  ~~已完成~~
api/index#Options/Assets|[milkmeowo](https://github.com/milkmeowo)| [mrwiredancer](https://github.com/Mr-Wiredancer)|  ~~已完成~~
api/index#Options/misc |[milkmeowo](https://github.com/milkmeowo)| hayeah |  ~~已完成~~
api/index#Instance Properties | [dmxj](https://github.com/dmxj) |[mrwiredancer](https://github.com/Mr-Wiredancer)| ~~已完成~~
api/index#Instance Methods/Data | [alfredcc](https://github.com/alfredcc) |egoist| ~~已完成~~
api/index#Instance Methods/Eevents |[dmxj](https://github.com/dmxj)| [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
api/index#Instance Methods/Lifecycle |[dmxj](https://github.com/dmxj)|[mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
api/index#Directives |[dmxj](https://github.com/dmxj)|[mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
api/index#Special Attributes |[aliencyl7](https://github.com/aliencyl7)| [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
api/index#Built-in Components |[milkmeowo](https://github.com/milkmeowo)| [mrwiredancer](https://github.com/Mr-Wiredancer)|~~已完成~~
api/index#VNode Interface |[milkmeowo](https://github.com/milkmeowo)| | ~~已完成~~
api/index#Server-Side Rendering |[milkmeowo](https://github.com/milkmeowo)| hayeah | ~~已完成~~
guide/installation |  [mrwiredancer](https://github.com/Mr-Wiredancer) | egoist | ~~已完成~~
guide/index | [mrwiredancer](https://github.com/Mr-Wiredancer) |pandazki | ~~已完成~~
guide/instance | [baurine](https://github.com/baurine)| [mrwiredancer](https://github.com/Mr-Wiredancer)| ~~已完成~~
guide/syntax |[baurine](https://github.com/baurine)|milkmeowo| ~~已完成~~
guide/computed |[gongph](https://github.com/gongph) |[mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/class-and-style | fszer | [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/conditional |[MechanicianW](https://github.com/MechanicianW) | hayeah | ~~已完成~~
guide/list | [mrwiredancer](https://github.com/Mr-Wiredancer) | coderkwong | ~~已完成~~
guide/events | [mrwiredancer](https://github.com/Mr-Wiredancer) | [milkmeowo](https://github.com/milkmeowo)| ~~已完成~~
guide/forms | jacelynfish | milmeowo | ~~已完成~~
guide/components |[MechanicianW](https://github.com/MechanicianW) |[mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/transitions | jacelynfish  | codekwong, mrwiredancer | ~~已完成~~
guide/transitioning-state |[gongph](https://github.com/gongph)| [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/render-function | [pandazki](https://github.com/pandazki) | [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/reactivity |[pandazki](https://github.com/pandazki) |[mrwiredancer](https://github.com/Mr-Wiredancer) |~~已完成~~
guide/custom-directive |[lsslu](https://github.com/lsslu)| milkmeowo, [mrwiredancer](https://github.com/Mr-Wiredancer) |~~已完成~~
guide/mixins | [leon0204](https://github.com/leon0204) | hayeah | ~~已完成~~
guide/plugins |[leon0204](https://github.com/leon0204) | [milkmeowo](https://github.com/milkmeowo), hayeah|~~已完成~~
guide/single-file-components | [mrwiredancer](https://github.com/Mr-Wiredancer) | [pandazki](https://github.com/pandazki) | ~~已完成~~
guide/deployment | [mrwiredancer](https://github.com/Mr-Wiredancer) | coderkwong | ~~已完成~~
guide/routing |[lsslu](https://github.com/lsslu)| [milkmeowo](https://github.com/milkmeowo), [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/state-management | mrwiredancer | coderkwong | ~~已完成~~
guide/unit-testing |[Yogi-Jiang](https://github.com/Yogi-Jiang) | hayeah | ~~已完成~~
guide/ssr | gongph | [mrwiredancer](https://github.com/Mr-Wiredancer) | ~~已完成~~
guide/migration |[RiXiong](https://github.com/RiXiong)| mrwiredancer|~~已完成~~
guide/migration-vue-router |[mrwiredancer](https://github.com/Mr-Wiredancer) | [pandazki](https://github.com/pandazki) | ~~已完成~~
guide/migration-vuex | [mrwiredancer](https://github.com/Mr-Wiredancer) | coderkwong | ~~已完成~~
guide/comparison | [mrwiredancer](https://github.com/Mr-Wiredancer) | [milkmeowo](https://github.com/milkmeowo) | ~~已完成~~
guide/join |[leon0204](https://github.com/leon0204)| [mrwiredancer](https://github.com/Mr-Wiredancer)| ~~已完成~~
