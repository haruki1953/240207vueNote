# day11-Vue3入门
[day11-Vue3入门.pdf](assets/day11-Vue3入门.pdf)  
[MD笔记-Vue3入门.pdf](assets/MD笔记-Vue3入门.pdf)  

https://cn.vuejs.org/

vue3的优势  
	![](assets/Pasted%20image%2020240210100638.png) 

## 一、Vue2 选项式 API vs Vue3 组合式API
![](assets/Pasted%20image%2020240210100818.png)
![](assets/Pasted%20image%2020240210101505.png)
1. 代码量变少
2. 分散式维护变成集中式维护

## 二、create-vue搭建Vue3项目
create-vue是Vue官方新的脚手架工具，底层切换到了 vite （下一代构建工具），为开发提供极速响应
	![](assets/Pasted%20image%2020240210101904.png) 

![](assets/Pasted%20image%2020240210111408.png)
> 前置条件 - 已安装16.0或更高版本的Node.js，node -v

`npm init vue@latest`  
这一指令将会安装并执行 create-vue
安装依赖：`npm install`
运行项目：`npm run dev` 或 `yarn dev`


## 三、熟悉项目目录和关键文件
![](assets/Pasted%20image%2020240210113335.png) 
[vite.config.js](project/day11-vue3-demo/vite.config.js)  
[package.json](project/day11-vue3-demo/package.json)  
[src/main.js](project/day11-vue3-demo/src/main.js)  
[src/App.vue](project/day11-vue3-demo/src/App.vue)  
[index.html](project/day11-vue3-demo/index.html)  

vetur是vue2的vscode插件，禁用。vue3的插件是volar
	![](assets/Pasted%20image%2020240210114828.png) 


## 四、组合式API
### setup选项
[01-setup.vue](project/day11-vue3-demo/src/01-setup.vue)  
setup选项的写法和执行时机
	![](assets/Pasted%20image%2020240210123141.png) 
**1、执行时机，比beforeCreate还要早**  
**2、setup函数中，获取不到this (this是undefined)，vue3将不用this**  

3、在setup函数中写的数据和方法需要在末尾以对象的方式return，才能给模版使用
	![](assets/Pasted%20image%2020240210124544.png) 
问题：每次都要return，好麻烦？

**4、通过 setup 语法糖简化代码**  
script标签添加 setup标记，不需要再写导出语句，默认会添加导出语句
	![](assets/Pasted%20image%2020240210125403.png) 

### reactive和ref函数
[02-reactive和ref.vue](project/day11-vue3-demo/src/02-reactive和ref.vue)  
默认的数据并不是响应式的，如果需要是响应式的，需要通过reactive或ref来进行处理
#### reactive()
作用：接受对象类型数据的参数传入并返回一个响应式的对象
```html
<script setup>
 // 导入
 import { reactive } from 'vue'
 // 执行函数 传入参数 变量接收
 const state = reactive({
   msg:'this is msg'
 })
 
 const setSate = ()=>{
   // 修改数据更新视图
   state.msg = 'this is new msg'
 }
</script>

<template>
  {{ state.msg }}
  <button @click="setState">change msg</button>
</template>
```
1. 从 vue 包中导入 reactive 函数
2. 在 `<script setup>` 中执行 reactive 函数并传入类型为对象的初始值，并使用变量接收返回值

#### ref()
作用：接收简单类型或者对象类型的数据传入并返回一个响应式的对象
```html
<script setup>
 // 导入
 import { ref } from 'vue'
 // 执行函数 传入参数 变量接收
 const count = ref(0)
 const setCount = ()=>{
   // 脚本中修改数据更新视图必须加上.value（模板中不用）
   count.value++
 }
</script>

<template>
  <button @click="setCount">{{count}}</button>
</template>
```
接收简单类型 或 复杂类型，返回一个响应式的对象  
本质：是在原有传入数据的基础上，外层包了一层对象，包成了复杂类型  
底层，包成复杂类型之后，再借助 reactive 实现的响应式  
注意点：  
1. 脚本中访问数据，需要通过 .value
2. 在template中，.value不需要加 

#### reactive 对比 ref
推荐：以后声明数据，统一用 ref => 统一了编码规范  
1. 都是用来生成响应式数据
2. 不同点
    1. reactive不能处理简单类型的数据
    2. ref参数类型支持更好，但是必须通过.value做访问修改
    3. ref函数内部的实现依赖于reactive函数
3. 在实际工作中的推荐
    1. 推荐使用ref函数，减少记忆负担


### computed
[03-computed.vue](project/day11-vue3-demo/src/03-computed.vue)  
计算属性基本思想和Vue2的完全一致，组合式API下的计算属性只是修改了写法
```html
<script setup>
// 导入
import {ref, computed } from 'vue'
// 原始数据
const count = ref(0)
// 计算属性
const doubleCount = computed(()=>count.value * 2)

// 原始数据
const list = ref([1,2,3,4,5,6,7,8])
// 基于list派生一个计算属性，从list中过滤出 > 2
const computedList = computed(() => {
  return list.value.filter(item => item > 2)
})
</script>
```
1. 导入computed函数 
2. 执行函数 在回调参数中return基于响应式数据做计算的值，用变量接收

https://cn.vuejs.org/api/reactivity-core.html#computed
```js
// 创建一个可写的计算属性 ref
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1
  }
})

plusOne.value = 1
console.log(count.value) // 0
```

最佳实践：
1. 计算属性中不应该有“副作用”
	比如异步请求/修改dom  
2. 避免直接修改计算属性的值
	计算属性应该是只读的，特殊情况可以配置get set


### watch
[04-watch.vue](project/day11-vue3-demo/src/04-watch.vue)  
作用: 侦听一个或者多个数据的变化，数据变化时执行回调函数   
两个额外参数：1. immediate（立即执行） 2. deep（深度侦听）  
#### 基础使用 - 侦听单个数据
1. 导入watch函数 
2. 执行watch函数传入要侦听的响应式数据(ref对象)和回调函数
```html
<script setup>
  // 1. 导入watch
  import { ref, watch } from 'vue'
  const count = ref(0)
  // 2. 调用watch 侦听变化
  watch(count, (newValue, oldValue)=>{
    console.log(`count发生了变化，老值为${oldValue},新值为${newValue}`)
  })
</script>
```
#### 基础使用 - 侦听多个数据
说明：同时侦听多个响应式数据的变化，不管哪个数据变化都需要执行回调
```html
<script setup>
  // 1. 导入watch
  import { ref, watch } from 'vue'
  const count = ref(0)
  const name = ref('cp')
  // 2. 调用watch 侦听变化
  watch([count, name], (newArr, oldArr) => {
  console.log(newArr, oldArr)
  })
</script>
```

#### immediate
说明：在侦听器创建时立即触发回调, 响应式数据变化之后继续执行回调
```html
<script setup>
  // 1. 导入watch
  import { ref, watch } from 'vue'
  const count = ref(0)
  // 2. 调用watch 侦听变化
  watch(count, (newValue, oldValue)=>{
    console.log(`count发生了变化，老值为${oldValue},新值为${newValue}`)
  },{
    immediate: true
  })
</script>
```

#### deep
通过watch监听的ref对象默认是浅层侦听的，直接修改嵌套的对象属性不会触发回调执行，需要开启deep选项
```html
<script setup>
  // 1. 导入watch
  import { ref, watch } from 'vue'
  const state = ref({ count: 0 })
  // 2. 监听对象state
  watch(state, ()=>{
    console.log('数据变化了')
  })
  const changeStateByCount = ()=>{
    // 直接修改不会引发回调执行
    state.value.count++
  }
</script>

<script setup>
  // 1. 导入watch
  import { ref, watch } from 'vue'
  const state = ref({ count: 0 })
  // 2. 监听对象state 并开启deep
  watch(state, ()=>{
    console.log('数据变化了')
  },{deep:true})
  const changeStateByCount = ()=>{
    // 此时修改可以触发回调
    state.value.count++
  }
</script>
```

#### 精确侦听对象的某个属性
需求：在不开启deep的前提下，侦听age的变化，只有age变化时才执行回调
![](assets/Pasted%20image%2020240210154649.png)

```js
// 5. 对于对象中的单个属性，进行监视
watch(() => userInfo.value.age, (newValue, oldValue) => {
  console.log(newValue, oldValue)
})
```

#### 总结
1. 作为watch函数的第一个参数，ref对象需要添加.value吗？
    不需要，第一个参数就是传ref 对象
2. watch只能侦听单个数据吗？ 
    单个 或者 多个 
3. 不开启deep，直接监视 复杂类型，修改属性能触发回调吗？
    不能，默认是浅层侦听 
4. 不开启deep，精确侦听对象的某个属性？
    可以把第一个参数写成函数的写法，返回要监听的具体属性


### 生命周期函数
[05-生命周期函数.vue](project/day11-vue3-demo/src/05-生命周期函数.vue)  
![](assets/Pasted%20image%2020240210160052.png)

1. 导入生命周期函数
2. 执行生命周期函数，传入回调
```html
<scirpt setup>
import { onMounted } from 'vue'
onMounted(()=>{
  // 自定义逻辑
})
</script>
```

生命周期函数执行多次的时候，会按照顺序依次执行
```html
<scirpt setup>
import { onMounted } from 'vue'
onMounted(()=>{
  // 自定义逻辑
})

onMounted(()=>{
  // 自定义逻辑
})
</script>
```

**总结**
1. 组合式API中生命周期函数的格式是什么？
    on + 生命周期名字 
2. 组合式API中可以使用onCreated吗？
    没有这个钩子函数，直接写到setup中
3. 组合式API中组件卸载完毕时执行哪个函数？
    onUnmounted


### 父子通信

**组合式API下的父传子**  
[06-父传子.vue](project/day11-vue3-demo/src/06-父传子.vue)  
[components/son-com.vue](project/day11-vue3-demo/src/components/son-com.vue)  
1. 父组件中给子组件绑定属性 
2. 子组件内部通过props选项接收
![](assets/Pasted%20image%2020240210184654.png)
```js
// 注意：由于写了 setup，所以无法直接配置 props 选项
// 所以：此处需要借助于 “编译器宏” 函数接收子组件传递的数据
const props = defineProps({
  car: String,
  money: Number
})
console.log(props.car)
console.log(props.money)
```

**组合式API下的子传父**   
[07-子传父.vue](project/day11-vue3-demo/src/07-子传父.vue)  
[components/son-com.vue](project/day11-vue3-demo/src/components/son-com.vue)  
1. 父组件中给子组件标签通过@绑定事件 
2. 子组件内部通过 emit 方法触发事件
![](assets/Pasted%20image%2020240210193646.png)
```js
// 通过defineEmits编译器宏生成emit方法（vue3中不使用this）
// 需要显示声明事件名称
const emit = defineEmits(['changeMoney'])

const buy = () => {
  // emit 触发事件
  emit('changeMoney', 5)
}
```

**总结**  
**父传子** 
1. 父传子的过程中通过什么方式接收props？
    defineProps( { 属性名：类型} ) 
2. setup语法糖中如何使用父组件传过来的数据？
    const props = defineProps( { 属性名：类型} ) 
    props.xxx 

**子传父** 
1. 子传父的过程中通过什么方式得到emit方法？
    `defineEmits( [‘事件名称’] ) 
2. 怎么触发事件 
    emit('自定义事件名' , 参数)


### 模版引用
模板引用的概念：通过ref标识获取真实的dom对象或者组件实例对象  
[【ref和$refs】day04-组件通信&进阶用法](day04-组件通信&进阶用法.md#ref和$refs)  
#### 如何使用（以获取dom为例 组件同理）
[08-模板引用.vue](project/day11-vue3-demo/src/08-模板引用.vue)  
1. 调用ref函数生成一个ref对象
2. 通过ref标识绑定ref对象到标签
3. 通过ref对象.value即可访问到绑定的元素(必须渲染完成后，才能拿到)
```html
<script setup>
import TestCom from '@/components/test-com.vue'
import { onMounted, ref } from 'vue'

// 模板引用(可以获取dom，也可以获取组件)
// 1. 调用ref函数，生成一个ref对象
// 2. 通过ref标识，进行绑定
// 3. 通过ref对象.value即可访问到绑定的元素(必须渲染完成后，才能拿到)
const inp = ref(null)

// 生命周期钩子 onMounted
onMounted(() => {
  // console.log(inp.value)
  // inp.value.focus()
})
const clickFn = () => {
  inp.value.focus()
}

// --------------------------------------
const testRef = ref(null)
const getCom = () => {
  console.log(testRef.value.count)
  testRef.value.sayHi()
}
</script>

<template>
  <div>
    <input ref="inp" type="text">
    <button @click="clickFn">点击让输入框聚焦</button>
  </div>
  <TestCom ref="testRef"></TestCom>
  <button @click="getCom">获取组件</button>
</template>
```

#### defineExpose()
[components/test-com.vue](project/day11-vue3-demo/src/components/test-com.vue)
默认情况下在 `<script setup>` 语法糖下组件内部的属性和方法是不开放给父组件访问的，可以通过defineExpose编译宏指定哪些属性和方法容许访问   
![](assets/Pasted%20image%2020240210204942.png)

#### 总结
1. 获取模板引用的时机是什么？
    组件挂载完毕 
2. defineExpose编译宏的作用是什么？
    显式暴露组件内部的属性和方法


### provide和inject
[09-provide和inject.vue](project/day11-vue3-demo/src/09-provide和inject.vue)  
作用和场景：顶层组件向任意的底层组件传递数据和方法，实现跨层组件通信  
1. 顶层组件通过 `provide` 函数提供数据
2. 底层组件通过 `inject` 函数提供数据
![](assets/Pasted%20image%2020240210205557.png)

**跨层传递响应式数据**  
在调用provide函数时，第二个参数设置为ref对象
![](assets/Pasted%20image%2020240210211504.png)

**跨层传递方法**
顶层组件可以向底层组件传递方法，底层组件调用方法修改顶层组件的数据
![](assets/Pasted%20image%2020240210211612.png)

## 五、Vue3.3新特性
### defineOptions

背景说明：  
有 `<script setup>` 之前，如果要定义 props, emits 可以轻而易举地添加一个与 setup 平级的属性。但是用了 `<script setup>` 后，就没法这么干了 setup 属性已经没有了，自然无法添加与其平级的属性。  
为了解决这一问题，引入了 defineProps 与 defineEmits 这两个宏。但这只解决了 props 与emits 这两个属性。如果我们要定义组件的 name 或其他自定义的属性，还是得回到最原始的用法——再添加一个普通的`<script>`标签。这样就会存在两个 `<script>` 标签。让人无法接受。  

所以在 Vue 3.3 中新引入了 defineOptions 宏。顾名思义，主要是用来定义 Options API 的选项。可以用defineOptions 定义任意的选项， props, emits, expose, slots 除外（因为这些可以使用defineXXX来做到）  
![](assets/Pasted%20image%2020240211093644.png)

### defineModel
（试验性质）
[App.vue](project/day11-vue3-demo/src/App.vue)  
[components/my-input.vue](project/day11-vue3-demo/src/components/my-input.vue)  
[components/my-input备份.vue](project/day11-vue3-demo/src/components/my-input备份.vue)  
在Vue3中，自定义组件上使用v-model, 相当于传递一个modelValue属性，同时触发 update:modelValue 事件
![](assets/Pasted%20image%2020240211094210.png)
我们需要先定义 props，再定义 emits 。其中有许多重复的代码。如果需要修改此值，还需要手动调用 emit 函数。  

于是乎 defineModel 诞生了。  
![](assets/Pasted%20image%2020240211094754.png)
生效需要配置 vite.config.js  
```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue({
      script: {
        // 开启defineModel
        defineModel: true
      }
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```


## 六、Pinia入门
[day12-Pinia入门.pdf](assets/day12-Pinia入门.pdf)  
[MD笔记-Pinia.pdf](assets/MD笔记-Pinia.pdf)  
Pinia 是 Vue 的最新 状态管理工具 ，是 Vuex 的 替代品  
https://pinia.vuejs.org/zh/  
![](assets/Pasted%20image%2020240211110217.png)

### 手动添加Pinia到Vue项目
在实际开发项目的时候，关于Pinia的配置，可以在项目创建时自动添加  
现在我们初次学习，从零开始：  
1. 使用 Vite 创建一个空的 Vue3 项目 
    `npm create vue@latest` 
2. 按照官方文档 安装 pinia 到项目中
https://pinia.vuejs.org/zh/getting-started.html  
[main.js](project/day12-vue3-pinia-demo/src/main.js)  

### Pinia基础使用 - 计数器案例
1. 定义store 
    https://pinia.vuejs.org/zh/core-concepts/
2. 组件使用store
![](assets/Pasted%20image%2020240211113659.png)
[store/counter.js](project/day12-vue3-pinia-demo/src/store/counter.js)  

### getters实现
Pinia中的 getters 直接使用 computed函数 进行模拟, 组件中需要使用需要把 getters return出去
![](assets/Pasted%20image%2020240211124845.png)

### action异步实现
编写方式：异步action函数的写法和组件中获取异步数据的写法完全一致  
[store/channel.js](project/day12-vue3-pinia-demo/src/store/channel.js)  

### storeToRefs工具函数
直接解构会导致响应式丢失 https://pinia.vuejs.org/zh/core-concepts/#using-the-store  
使用storeToRefs函数可以辅助保持数据（state + getter）的响应式解构  
`import { storeToRefs } from 'pinia'`  
![](assets/Pasted%20image%2020240211132841.png)
方法可以直接解构

### Pinia的调试
Vue官方的 dev-tools 调试工具 对 Pinia直接支持，可以直接进行调试
![](assets/Pasted%20image%2020240211133950.png)

### Pinia持久化插件
官方文档： https://prazdevs.github.io/pinia-plugin-persistedstate/zh/

1. 安装插件 pinia-plugin-persistedstate 
`npm i pinia-plugin-persistedstate` 

2. main.js 使用 
```js
import persist from 'pinia-plugin-persistedstate' 
...
app.use(createPinia().use(persist)) 
```

3. store仓库中，persist: true 开启
配置 store/counter.js
```js
import { defineStore } from 'pinia'
import { computed, ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  ...
  return {
    count,
    doubleCount,
    increment
  }
}, {
  persist: true
})
```
[store/counter.js](project/day12-vue3-pinia-demo/src/store/counter.js)  

**更多配置**
https://prazdevs.github.io/pinia-plugin-persistedstate/zh/guide/config.html
```js
// persist: true // 开启当前模块的持久化
persist: {
  key: 'hm-counter', // 修改本地存储的唯一标识
  paths: ['count'] // 存储的是哪些数据
}
```


### 总结
1. Pinia是用来做什么的？
	新一代的状态管理工具，替代vuex
2. Pinia中还需要mutation吗？
	不需要，action 既支持同步也支持异步
3. Pinia如何实现getter？
	computed计算属性函数
4. Pinia产生的Store如何解构赋值数据保持响应式？
	storeToRefs
5. Pinia 如何快速实现持久化？
	pinia-plugin-persistedstate



