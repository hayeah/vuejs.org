# Vue.js 2.0 翻译

这个厂库只用来协作翻译

[vue2 文档厂库](https://github.com/hayeah/vuejs.org)

## 翻译规范（参考了 [vue 1.0 翻译规范](https://github.com/vuejs/cn.vuejs.org/blob/lang-zh/CONTRIBUTING.md#翻译) ）

* 使用中文的符号
* 汉字，字母，数字等之间以空格隔开。
* 专有词注意大小写，如 HTML，CSS，JavaScript。
* 代码只翻译注释。

## 翻译字典（更新中）

英文 | 中文 | 备注 |
---- | ---- | ---- |
See also | 另见
component | 组件
functional | 函数化| e.g. `functional component` ：函数化组件
reactive | 响应式的 


参考 [vuejs-1.0 中文文档](http://cn.vuejs.org/)

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

## 进度表 (最后更新：2016-10-5 08:52)

文章 | 翻译 | 校对 | 状态 |
---- | ---- | ---- | ---- |
api/index#Global Config | mrwiredancer | egoist | ~~已完成~~
api/index#Global API | pandazki |mrwiredancer |  ~~已完成~~
api/index#Options/Data |[milkmeowo](https://github.com/milkmeowo)| egoist| ~~已完成~~
api/index#Options/DOM |[milkmeowo](https://github.com/milkmeowo)| Mr-Wiredancer | ~~已完成~~
api/index#Options/Lifecycle Hooks |[milkmeowo](https://github.com/milkmeowo)| hayeah |  ~~已完成~~
api/index#Options/Assets|[milkmeowo](https://github.com/milkmeowo)| Mr-Wiredancer|  ~~已完成~~
api/index#Options/misc |[milkmeowo](https://github.com/milkmeowo)| hayeah |  ~~已完成~~
api/index#Instance Properties | [dmxj](https://github.com/dmxj) |mrwiredancer| ~~已完成~~
api/index#Instance Methods/Data | [alfredcc](https://github.com/alfredcc) |egoist| ~~已完成~~
api/index#Instance Methods/Eevents |[dmxj](https://github.com/dmxj)| |`翻译中`
api/index#Instance Methods/Lifecycle |[dmxj](https://github.com/dmxj)| | `翻译中`
api/index#Directives |[dmxj](https://github.com/dmxj)| |`翻译中`
api/index#Special Attributes |[aliencyl7](https://github.com/aliencyl7)| | **待校对**
api/index#Built-in Components |[milkmeowo](https://github.com/milkmeowo)| mrwiredancer|~~已完成~~
api/index#VNode Interface |[milkmeowo](https://github.com/milkmeowo)| | ~~已完成~~
api/index#Server-Side Rendering |[milkmeowo](https://github.com/milkmeowo)| hayeah | ~~已完成~~
guide/installation |  [mrwiredancer](https://github.com/Mr-Wiredancer) | egoist | ~~已完成~~
guide/index | [mrwiredancer](https://github.com/Mr-Wiredancer) | | `翻译中`
guide/instance | [baurine](https://github.com/baurine)| mrwiredancer| **校对中**
guide/syntax |[baurine](https://github.com/baurine)|egoist| ~~已完成~~
guide/computed |[gongph](https://github.com/gongph) |mrwiredancer | ~~已完成~~
guide/class-and-style |
guide/conditional |[MechanicianW](https://github.com/MechanicianW) | hayeah | ~~已完成~~
guide/list |
guide/events | [rayax86](https://github.com/rayax86) | | `翻译中`
guide/forms |
guide/components |[MechanicianW](https://github.com/MechanicianW) |egoist | **校对中**
guide/transitions |
guide/transitioning-state |[gongph](https://github.com/gongph)| | `翻译中`
guide/render-function | [pandazki](https://github.com/pandazki
guide/reactivity |[pandazki](https://github.com/pandazki) |mrwiredancer | **校对中**
guide/custom-directive |[lsslu](https://github.com/lsslu)| milkmeowo |**校对中** 
guide/mixins | [leon0204](https://github.com/leon0204) | mrwiredancer | **校对中**
guide/plugins |[leon0204](https://github.com/leon0204) | milkmeowo|**校对中**
guide/single-file-components | [seaify](https://github.com/seaify) | | `翻译中`
guide/routing |[lsslu](https://github.com/lsslu)| milkmeowo |**校对中** 
guide/state-management |
guide/unit-testing |[Yogi-Jiang](https://github.com/Yogi-Jiang) | mrwiredancer | **校对中**
guide/ssr |
guide/migration |[RiXiong](https://github.com/RiXiong)| mrwiredancer|**校对中**
guide/comparison |
guide/join |[leon0204](https://github.com/leon0204)| mrwiredancer| **校对中**
