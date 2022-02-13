---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
title: 单元测试从 0 - 1
---

# 单元测试从 0 - 1

<div class="pt-12">
  单元测试概念和实践
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
今天讲的可能有点务虚。我尽量结合示例讲的干一点。
-->

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# 大纲

- ## 单元测试是什么
- ## 为什么需要
- ## 框架选型
- ## 一个 demo
- ## 一些实践
- ## 测什么

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
h2 {
  margin-top: 30px;
}
</style>

---
layout: center
---

# 单元测试是什么

---

其实很难定义什么是单元测试，但可以从以下特点去感受下单元测试。

<div v-click class="mt-40px">

## 单元

一个系统最’原子’的行为单位

</div>

<div v-click>

## 要快

完成时间要小于 100ms

</div>

<div v-click>

## 要能快速定位问题

独立地对某一段代码进行测试，对用例进行分组（describe/it）

</div>

<!--
首先解决是什么的问题：《修改代码的艺术》

* 单元：一个系统最’原子’的行为单位，在过程式或函数式编程中单元一般值函数，在面向对象中就是类。

* 要快： 一个需要耗时十分之一秒才能执行完的羊元测试就已算是一个慢的单元测试了. 

  - 数据库有交互，
  - 进行了网络间通信，
  - 调用了文件系统，
  - 需要你对环境作特定的准备(如编辑配直文件)才能运行
  - 各种耗时的操作都不应该在单测中出现。

* 要能快速定位问题：测试用例覆盖路径（单元）越小越容易定位问题的所在。所以要尽量 mock 掉依赖。

  单元测试 做到了大型测试所不能做到的那些事情。利用单元测试可以独立地对某一段代码进行测试，分组-测试用例
-->

---
layout: two-cols
---

# 一些概念

<v-clicks>

- describe、it/test
- assert/expect 断言
- mock、stub
- coverage: 覆盖率
- 两种范式：BDD、TDD

</v-clicks>

::right::

<div v-click="1" class="absolute w-full h-full bg-white">

```ts {1,3,8}
describe('counter', () => {

  it('counter +', async () => {
    await $button.trigger('click')
    expect($button.text()).toContain('count is: 1')
  })

  it('counter ++', async () => {
    await $button.trigger('click')
    await $button.trigger('click')
    expect($button.text()).toContain('count is: 2')
  })

})
```

</div>

<div v-click="2" class="absolute w-full h-full bg-white">

```ts {5,11}
describe('counter', () => {

  it('counter +', async () => {
    await $button.trigger('click')
    expect($button.text()).toContain('count is: 1')
  })

  it('counter ++', async () => {
    await $button.trigger('click')
    await $button.trigger('click')
    expect($button.text()).toContain('count is: 2')
  })

})
```

</div>

<div v-click="3" class="absolute w-full h-full bg-white">

```ts
jest.mock('../../services/user', () => {
  const originalModule = jest.requireActual('../../services/user');

  //Mock the default export and named export 'foo'
  return {
    __esModule: true,
    ...originalModule,
    getUsersByIds: jest.fn(async () => {
      return [{
        id: 44,
        name: 'weineel-44',
      }, {
        id: 55,
        name: 'weineel-55',
      }]
    }),
  };
})

it('load', async () => {
  getUsersByIds([44])
  expect(getUsersByIds).toHaveBeenCalledWith([44])
})
```

</div>

<div v-click="4" class="absolute w-full h-full bg-white">

<img src="/coverage.png" class="object-contain w-1/2 -ml-40px" />

</div>

<!-- 
1. 为什么分组？

以便在某些特定条件下运行某些特定的测试，并在其他条件下运行另一些测试。我们还可以迅速定位错误。

如果认为在某段代码中存在着一个错误而且又可以在测试用具中使用这段代码的话，我们通常能够迅速地编写出一段测试，看看我们所推测的错误是不是真的在那里。

2. 断言有很多方式，python 中就是简单的 assert

3. mock、stub 已经很少区分了，而且意义不是很大

> mock / stub 是单元测试关键中的关键, 不需要测的依赖部分都 mock 掉。

严格区分 mock 和 stub

* stub 用来代替实际的依赖，修改其返回值得以控制被测代码的走向
* 同时 stub 的作用可以隔离被依赖代码，防止被依赖代码的修改导致被测代码单元测试的修改。因为被依赖代码应该有独立的测试代码。

Mock 方式

* mock 代码
* json-server
* express 自己实现服务
* 是否可以整个 mock axios? 可以使用 jest mock。

BDD

* specification(spec)
* 断言的描述性

-->

---
layout: center
---

# 为什么需要

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

<v-clicks>

## 健壮性

## 重构的底气

## 更优雅的代码

</v-clicks>

<style>

h2 {
  padding-top: 80px;
}

</style>

<!--

## 更优雅的代码

* 单元测试写完了，代码也变得优雅很多，单元测试代码的简洁，可以从侧面体现了接口的设计合理。
* 是更宽泛的要求，尽量面向接口测试，而不是实现细节。
* 难以测试的代码，极有可能是不符合OOP设计原则的，不满足基本的SOLID原则和设计模式。
* 提高对于面向对象的理解，发现代码难以测试，就可以重新考虑设计问题（设计模式），从而提高设计能力、编码能力。

-->

---
layout: center
---

# 框架选型

---
layout: image-left
image: https://source.unsplash.com/collection/94734566/1920x1080
---

<v-clicks>

## Jest

## Mocha、Chai/should.js

## Vitest

</v-clicks>

<style>

h2 {
  padding-top: 80px;
}

</style>

<!--

## Jest

* demo 用的 Jest，个人感觉有点慢。
* 最近又发现它不维护了。

[https://github.com/facebook/jest/pull/11529#issuecomment-1027091448](https://github.com/facebook/jest/pull/11529#issuecomment-1027091448)

## Mocha、Chai/should.js

老牌框架，配置相对复杂。

## Vitest

* 专门给 vite 打造的开箱即用测试框架
* 看官网介绍：速度更快，更高层次的封装，集大成者
* 还在开发中，不建议用在生产环境

-->

---
layout: center
---

# 一个 demo

---
layout: two-cols
---

<div class="pr-20px">
<div class="mb-20px">
  <Counter />
</div>

```ts
<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button type="button" @click="count++">count is: {{ count }}</button>
</template>

```
</div>

::right::

```ts
import { DOMWrapper, mount } from '@vue/test-utils'
import { Counter } from '../index'

let $wrapper
let $button: DOMWrapper<Element>

beforeEach(() => {
  $wrapper = mount(Counter, {})
  $button = $wrapper.find('button')
})

it('counter +', async () => {
  await $button.trigger('click')

  expect($button.text())
    .toContain('count is: 1')
})

it('counter ++', async () => {
  await $button.trigger('click')
  await $button.trigger('click')

  expect($button.text())
    .toContain('count is: 2')
})
```

---
layout: center
---

# 一些实践

---

<v-clicks>

## 什么时候开始单元测试？

## 经常听说的不可测性是什么意思？

## 为什么代码一重构，单元测试就崩溃。

## 已经测过的内容无需再测？

## 要不要测样式？还是只需要测逻辑

## 怎么分解任务才好测？

</v-clicks>

<style>
h2 {
  margin-top: 36px;
}
</style>

<!--

## 什么时候开始单元测试？

* 越早越好
* 单元测试尽量和编码的时候一起写，如果等代码实现完了，会因为要写的测试太多，而产生畏惧，容易放弃
* 项我们 V7 给它加 单测几乎不可能了，而且很多代码的实现是不考虑单测的，充斥大量不可测代码。

## 经常听说的不可测性是什么意思？

* 很难测、几乎无法测、成本很高，不是无法测，而是测起来很麻烦成本高，多写很多 mock 代码，本来半小时写好的测试需要写2小时。
* 被测单元有副作用

  * 直接在函数内部路由跳转：路由跳转可以写在单独的函数（jump2xxx）里，这样可以很方便的mock 掉，然后检查是否被调用过。
  * 修改全局变量：单元测试可以检查函数返回值很难检查到函数外部变量，所以单元测试更喜欢纯函数，所以尽量不要使用全局变量，维护在 store 中是一种好的方法。

* 当前时间、直接使用随机数生成函数：可以加一层写一个单独的函数去实现对应的功能，方便被 mock。
* 组件中使用this.$route.*：单元测试环境是没有路由的，这个对象很难被 mock，也有办法成本较高，所以最好的方式是添加路由的 prop 配置。
* 等

## 为什么代码一重构，单元测试就崩溃？

* 单测是重构的基石，重构不应该影响单测。
* 这很大程度上是由于测试对实现细节的依赖过于紧密。一般来说，单元测试最好是面向接口行为来设计，因为这是一个更宽泛的要求。优先测试接口，再根据实际情况看是否要验证实现，因为实现可能会因为逻辑变更而失效（导致维护成本变大）
* 

## 已经测过的内容无需再测？

* 这个是为了圈定边界
* 所依赖的项应该，其自身已经有单元测试了
* 第三方依赖不需要测
* 识别出能 mock 依赖

## 要不要测样式？还是只需要测逻辑？

这个应该在 e2e 中测试，不要无限扩展 单测 的范围

## 怎么分解任务才好测？

比较开放，比较大的问题，没有实践经验，需要真正落地时慢慢积累经验。

-->

---
layout: center
---

# 测什么?

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

一个需要持续更新的 check list

<v-clicks>

## 1. 默认值

## 2. 数据接口函数

## 3. 改bug前先写对应的单元测试用例

</v-clicks>

<style>
h2 {
  margin-top: 80px;
}
</style>

<!--

## 默认值

组件、函数参数、config等的默认值不应被意外修改

## 数据接口函数

1. 作为依赖时，两种情况分开测。
  * 测试通过交互后传递的参数、调用的次数等，不测返回值，返回值应该是后端单元测试(或者是接口测试要做的)要做的。
  * mock 函数的返回值（分多个用例，覆盖尽量多的后续分支），无需关注传参，测试展示情况，和后续交互。

2. 直接测试 service 函数（可以作为 API 测试，其实应该作为 后端 单元测试的一部分），可以提前识别后端接口变化导致的问题。

## 改 bug 前先写对应的单元测试

-->

---
layout: center
---

# 任何问题？
