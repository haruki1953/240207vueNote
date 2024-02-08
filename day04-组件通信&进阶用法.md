# day04-组件通信&进阶用法

## 一、组件的三大组成部分（结构/样式/逻辑）
### scoped解决样式冲突
写在组件中的样式会 **全局生效** → 因此很容易造成多个组件之间的样式冲突问题。
1. **全局样式**: 默认组件中的样式会作用到全局，任何一个组件中都会受到此样式的影响
2. **局部样式**: 可以给组件加上**scoped** 属性,可以**让样式只作用于当前组件**

```html
<template>
  <div class="base-one">
    BaseOne
  </div>
</template>

<script>
export default {

}
</script>
<style scoped>
</style>
```
组件都应该有独立的样式，推荐加scoped

**scoped原理**
1. 当前组件内标签都被添加**data-v-hash值** 的属性
2. css选择器都被添加 `[data-v-hash值]` 的属性选择器

最终效果: **必须是当前组件的元素**, 才会有这个自定义属性, 才会被这个样式作用到
	![](assets/Pasted%20image%2020240201114126.png) 

### data必须是一个函数
一个组件的 **data** 选项必须**是一个函数**。目的是为了：保证每个组件实例，维护**独立**的一份**数据**对象。
每次创建新的组件实例，都会新**执行一次data 函数**，得到一个新对象。
	![](assets/Pasted%20image%2020240201114553.png) 


## 二、组件通信
组件通信，就是指**组件与组件**之间的**数据传递**
- 组件的数据是独立的，无法直接访问其他组件的数据。
- 想使用其他组件的数据，就需要组件通信

**组件关系分类**
	![](assets/Pasted%20image%2020240201121710.png) 
1. 父子关系
2. 非父子关系

**通信解决方案**
	![](assets/Pasted%20image%2020240201121750.png) 

### 父子通信
流程
	![](assets/Pasted%20image%2020240201122032.png) 
1. 父组件通过 **props** 将数据传递给子组件
2. 子组件利用 **$emit** 通知父组件修改更新

#### 父组件通过**props**将数据传递给子组件
父向子传值步骤
	![](assets/Pasted%20image%2020240201123909.png) 
1. 给子组件以添加属性的方式传值
2. 子组件内部通过props接收
3. 模板中直接使用 props接收的值

#### 子组件利用 **$emit** 通知父组件，进行修改更新
子向父传值步骤
	![](assets/Pasted%20image%2020240201124741.png) 
1. $emit触发事件，给父组件发送消息通知
2. 父组件监听$emit触发的事件
3. 提供处理函数，在函数的性参中获取传过来的参数

### props 详解
**Props 定义：** 组件上注册的一些 自定义属性
**Props 作用：** 向子组件传递数据
**特点** 
1. 可以 传递 **任意数量** 的prop
2. 可以 传递 **任意类型** 的prop
![](assets/Pasted%20image%2020240201125753.png) 

#### props 校验
**作用：** 为组件的 prop 指定**验证要求**，不符合要求，控制台就会有**错误提示** → 帮助开发者，快速发现错误
**语法：**
	 ![](assets/Pasted%20image%2020240201130139.png) 
	 ![](assets/Pasted%20image%2020240201132145.png) 
- **类型校验**
- 非空校验
- 默认值
- 自定义校验

```js
export default {
  // 1.基础写法（类型校验）
  // props: {
  //   w: Number,
  // },

  // 2.完整写法（类型、非空、默认值、自定义校验）
  props: {
    w: {
      type: Number,
      required: true,
      default: 0,
      validator(val) {
        // console.log(val)
        if (val >= 100 || val <= 0) {
          console.error('传入的范围必须是0-100之间')
          return false
        } else {
          return true
        }
      },
    },
  },
}
```
**注意：**
1. default和required一般不同时写（因为当时必填项时，肯定是有值的）
2. default后面如果是简单类型的值，可以直接写默认。如果是复杂类型的值，则需要以函数的形式return一个默认值


#### props和data，单向数据流
**共同点：** 都可以给组件提供数据
**区别：**
- data 的数据是**自己**的 → 随便改
- prop 的数据是**外部**的 → 不能直接改，要遵循 **单向数据流**

**单向数据流：**
父级props 的数据更新，会向下流动，影响子组件。这个数据流动是单向的
	![](assets/Pasted%20image%2020240201122032.png) 

### 非父子通信
#### event bus 事件总线
非父子组件之间，进行简易消息传递。
1.创建一个都能访问的事件总线 （就是在一个js文件里创建空Vue实例），EventBus.js
   ```js
   import Vue from 'vue'
   const Bus = new Vue()
   export default Bus
   ```
2.A组件（接受方），监听Bus的 $on事件
   ```js
   import Bus from '../utils/EventBus'
   created () {
     Bus.$on('sendMsg', (msg) => {
       this.msg = msg
     })
   }
   ```
3.B组件（发送方），触发Bus的$emit事件
   ```js
   import Bus from '../utils/EventBus'
   Bus.$emit('sendMsg', '这是一个消息')
   ```
![](assets/Pasted%20image%2020240201171201.png) 


#### provide&inject
跨层级共享数据
	![](assets/Pasted%20image%2020240201172438.png) 

**父组件 provide提供数据**
```js
export default {
  provide() {
    return {
      // 简单类型 是非响应式的
      color: this.color,
      // 复杂类型 是响应式的
      userInfo: this.userInfo,
    }
  },
  data() {
    return {
      color: 'pink',
      userInfo: {
        name: 'zs',
        age: 18,
      },
    }
  }
}
```

**子/孙组件 inject获取数据**
```js
export default {
  inject: ['color','userInfo'],
  created () {
    console.log(this.color, this.userInfo)
  }
}
```

**注意**
- provide提供的简单类型的数据不是响应式的，复杂类型数据是响应式。（推荐提供复杂类型数据）
- 子/孙组件通过inject获取的数据，不能在自身组件内修改


## 综合案例：小黑记事本（组件版）
[09-小黑记事本.zip](assets/09-小黑记事本.zip)

### 组件拆分
**需求说明：**
- 拆分基础组件
- 渲染待办任务
- 添加任务
- 删除任务
- 底部合计 和 清空功能
- 持久化存储

可以把小黑记事本原有的结构拆成三部分内容：头部（TodoHeader）、列表(TodoMain)、底部(TodoFooter)
	![](assets/Pasted%20image%2020240201142659.png) 

### 列表渲染
思路分析：
1. 提供数据：提供在公共的父组件 App.vue
2. 通过父传子，将数据传递给TodoMain
3. 利用v-for进行渲染

### 添加功能
思路分析：
1. 收集表单数据 v-model
2. 监听时间 （回车+点击 都要进行添加）
3. 子传父，将任务名称传递给父组件App.vue
4. 父组件接受到数据后 进行添加 **unshift**(自己的数据自己负责)

### 删除功能
思路分析：
1. 监听时间（监听删除的点击）携带id
2. 子传父，将删除的id传递给父组件App.vue
3. 进行删除 **filter** (自己的数据自己负责)

### 底部功能及持久化存储
思路分析：
1. 底部合计：父组件传递list到底部组件 —>展示合计
2. 清空功能：监听事件 —> **子组件**通知父组件 —>父组件清空
3. 持久化存储：watch监听数据变化，持久化到本地

## 三、进阶语法
### v-model原理
v-model本质上是一个语法糖。例如应用在输入框上，就是value属性 和 input事件 的合写
```html
<template>
  <div id="app" >
    <input v-model="msg" type="text">

    <input :value="msg" @input="msg = $event.target.value" type="text">
  </div>
</template>
```
**$event** 用于在模板中，获取事件的形参

不同的表单元素， v-model在底层的处理机制是不一样的。比如给checkbox使用v-model
底层处理的是 checked属性和change事件。
**不过咱们只需要掌握应用在文本框上的原理即可**

#### 表单类组件封装
实现子组件和父组件数据的双向绑定 （实现App.vue中的selectId和子组件选中的数据进行双向绑定）
	![](assets/Pasted%20image%2020240202082707.png) 

#### v-model简化代码
v-model其实就是 :value和@input事件的简写
- 子组件：props通过value接收数据，事件触发 input
- 父组件：v-model直接绑定数据
	![](assets/Pasted%20image%2020240202083304.png) 

### .sync修饰符
**作用：** 可以实现 **子组件** 与 **父组件数据** 的 **双向绑定**，简化代码
简单理解：**子组件可以修改父组件传过来的props值**
**特点：** prop属性名，可以自定义，非固定为 value 
**场景：** 封装弹框类的基础组件， visible属性 true显示 false隐藏 
**本质：** 就是 :属性名 和 @update:属性名 合写
![](assets/Pasted%20image%2020240202090942.png)

### ref和$refs
利用ref 和 $refs 可以用于 获取 dom 元素 或 组件实例
![](assets/Pasted%20image%2020240202092958.png)
![](assets/Pasted%20image%2020240202094903.png)


### 异步更新 & $nextTick
**Vue 是异步更新DOM**
![](assets/Pasted%20image%2020240202100846.png)
```html
<template>
  <div class="app">
    <div v-if="isShowEdit">
      <input type="text" v-model="editValue" ref="inp" />
      <button>确认</button>
    </div>
    <div v-else>
      <span>{{ title }}</span>
      <button @click="editFn">编辑</button>
    </div>
  </div>
</template>
```

$nextTick：**等 DOM更新后**,才会触发执行此方法里的函数体
![](assets/Pasted%20image%2020240202100928.png)

**注意：**$nextTick 内的函数体 一定是**箭头函数**，这样才能让函数内部的this指向Vue实例
> 箭头函数没有自己的this，在箭头函数里使用的this是在定义时被闭包的