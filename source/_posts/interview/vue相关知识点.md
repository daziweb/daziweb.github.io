---
title: Vue 常见知识点
tags:
  - Vue
categories:
  - Vue
comments: true
---

# Vue 常见知识点

## 一、什么是 mvvm

MVVM 是 Model-View-ViewModel 的缩写。mvvm 是一种设计思想。 Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来， ViewModel 是一个同步 View 和 Model 的对象。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的，因些 View 数据的变化会同步到 Model 中，而且 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而且 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM，不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

<!-- more -->

## 二、mvvm 和 mvc 区别？

mvc 和 mvvm 其实区别不大。 都是一种设计思想。主要就是 mvc 中 Controller 演变成 mvvm 中的 ViewModel。 mvvm 主要解决了 mvc 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。和当 model 频繁发生变化，开发者需要主动更新到 View。

## 三、vue 的优点是什么？

- 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的**VIew**上，当 View 变化的时候 Model 可以不变， 当 Model 变化的时候 View 也可以不变。
- 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。
- 独立开发。 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计。
- 可测试。 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

## 四、vue 生命周期的理解

**总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后。**

- 创建前/后: 在 beforeCreate 阶段，vue 实例的挂载元素 el 还没有。
- 载入前/后: 在 beforeMount 阶段，vue 实例的 \$el 和 data 都初始化了，但还是挂载之前为虚拟的 dom 节点， data.message 还未替换。在 mounted 阶段， vue 实例挂载完成，data.message 成功渲染。
- 更新前/后: 当 data 变化时，会触发 beforeUpdate 和 updated 方法。
- 销毁前/后: 在执行 destroy 方法后，对 data 的改变不会再触发周期函数，说明此时 vue 实例已经解除了事件监听以及和 dom 的绑定，但是 dom 结构依然存在。

## 五、组件之间的传值

1. 父组件与子组件传值（**父组件通过标签上面定义传值**）
   - 子组件在 props 中创建一个属性，用以接收父组件传过来的值
   - 父组件中注册子组件
   - 在子组件标签中添加子组件 props 中创建的属性
   - 把需要传给子组件的值赋给该属性

```javascript
// 父组件通过标签上面定义传值
<template>
  <Main :obj="data"></Main>
</template>

<script>
  // 引入子组件
  import Main form "./main"

  export default {
    name: "parent",
    data() {
      return {
        data: "我要向子组件传递的数据"
      }
    },
    // 初始化组件
    components: {
      Main
    }
  }
</script>


// 子组件通过 props 方法接受数据
<template>
  <div>{{ data }}</div>
</template>

<script>
  export default {
    name: "son",
    // 接受父组件传值
    props: ["data"]
  }
</script>
```

2. 子组件向父组件传递数据（**子组件通过\$emit 方法传递参数**）
   - 子组件中需要以某种方式例如点击事件的方法来触发一个自定义事件
   - 将需要传的值作为\$emit 的第二个参数，该值将作为实参传给响应自定义事件的方法
   - 在父组件中注册子组件并在子组件标签上绑定对自定义事件的监听

```javascript
<template>
  <Main v-bind:message="parentMessage" v-on:listenToChildEvent="showMsgFromChild"></Main>
</template>
<script>
  // 引入子组件
  import Main form "./main"

  export default {
    name: "parent",
    data() {
      return {
        parentMessage: "hello"
      }
    },
    methods: {
      showMsgFromChild: function(data) {
        console.log(data)
      }
    }
  }
</script>

// 子组件
<template>
  <div>{{ message }}</div>
  <button v-on:click="sendMsgToParent">向父组件传值</button>
</template>
<script>
  export default {
    name: "son",
    props: ["message"],
    methods: {
      sendMsgToParent: function() {
        this.$emit("listenToChildEvent", "this message is from child")
      }
    }
  }
</script>
```

## 六、 active-class 是哪个组件的属性

vue-router 模块的 router-link 组件。

## 七、嵌套路由怎么定义

在实际项目中我们会碰到多层嵌套的组件组合而成，但是我们如何实现嵌套路由呢？因此我们需要在 VueRouter 的参数中使用 children 配置，这样就可以很好的实现路由嵌套。index.html 只有一个路由出口。

```javascript
<div id="app">
  <!-- router-view 路由出口 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

main.js 路由的重定向，就会在页面一加载的时候，就会将 home 组件显示出来，因为重定向指向了 home 组件， redirect 的指向与 path 的必须一致。 children 里面是子路由，当然子路由里面还可以继续嵌套子路由。

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

// 引入两个组件
import home from './home.vue';
import game from './game.vue';

// 定义路由
const routes = [
  {
    path: '/',
    redirect: '/home' // 重定向 指向了 home 组件
  },
  {
    path: '/home',
    component: home,
    children: [
      {
        path: '/home/game',
        component: game
      }
    ]
  }
];

// 创建路由实例
const router = new VueRouter({ routes });

new Vue({
  el: '#app',
  router
});
```

home.vue 点击显示就会将子路由显示出来，子路由的出口必须在父路由里面，否则子路由无法显示。

## 八、路由之间跳转

- 声明式（标签跳转） `<router-link :to="index">`
- 编程式（js 跳转）`router.push('index')`

## 九、懒加载（按需加载路由）

webpack 中提供了 require.ensure() 来实现按需加载。以前引入路由是通过 import 这样的方式引入，改为 const 定义的方式进行引入。

- 不进行页面按需加载引入方式：

```javascript
import home from '../../common/home.vue';
```

- 进行页面按需加载的引入方式：

```javascript
const home = r => require.ensure([], () => r(require('../../common.vue')));
```

## 十、vuex 是什么 怎么使用 哪种功能场景使用它

vue 框架中状态管理。在 main.js 引入 store，注入。新建了一个目录 store， ... export。
场景有： 单面应用中，组件之间的状态。音乐播放、登录状态、加入购物车。

```javascript
// 新建 store.js
import vue from 'vue'
import vuex from 'vuex'

vue.use(vuex)

export default new vuex.store({
  // ...code
})

// main.js
import store from './store'
...
```

## 十一、vue-router 有哪几种导航钩子

三种

- 全局导航钩子

  - router.beforeEach(to, from, next),
  - router.beforeResolve(to, from, next),
  - router.afterEach(to, from, next)

- 组件内钩子

  - beforeRouteEnter
  - beforeRouteUpdate
  - beforeRouteLeave

- 单独路由独享组件

  - beforeEnter

## 十二、自定义指令（v-check, v-focus）的方法有哪些？它有哪些钩子函数？还有哪些钩子函数参数

- 全局定义指令： 在 vue 对象的 directive 方法里面有两个参数，一个是指令名称，另一个是函数。
- 组件内定义指令：directives
- 钩子函数：bind(绑定事件出发)、inserted(节点插入时候触发)、update(组件内相关更新)
- 钩子函数参数：el、binding

## 十三、说出至少 4 种 vue 当中的指令和它的用法

v-if(判断是否隐藏)、v-for(把数据遍历出来)、v-bind(绑定属性)、v-model(实现双向绑定)

##
