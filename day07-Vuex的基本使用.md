# day07-Vuex的基本使用
## 一、vuex概述
vuex 是什么，应用场景，优势
	![](assets/Pasted%20image%2020240205120336.png) 
Vuex 是一个 Vue 的 状态管理工具
Vuex 是一个插件，可以帮我们管理 Vue 通用的数据 (多组件共享的数据)。例如：购物车数据 个人信息数据

官方原文：
- 不是所有的场景都适用于vuex，只有在必要的时候才使用vuex
- 使用了vuex之后，会附加更多的框架中的概念进来，增加了项目的复杂度 （数据的操作更便捷，数据的流动更清晰）
Vuex就像《近视眼镜》, 你自然会知道什么时候需要用它~


## 二、构建 vuex【多组件数据共享】环境
需求
	![](assets/Pasted%20image%2020240205120931.png) 

### 创建一个空仓库
![](assets/Pasted%20image%2020240205122032.png) 

创建仓库 `store/index.js`
```js
// 导入 vue
import Vue from 'vue'
// 导入 vuex
import Vuex from 'vuex'
// vuex也是vue的插件, 需要use一下, 进行插件的安装初始化
Vue.use(Vuex)

// 创建仓库 store
const store = new Vuex.Store()

// 导出仓库
export default store
```

main.js 中导入挂载到 Vue 实例上
```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```
此刻起, 就成功创建了一个 **空仓库!!**

测试打印Vuex
App.vue
```js
created(){
  console.log(this.$store)
}
```


## 三、核心概念 - state 状态
**目标**
明确如何给仓库 提供 数据，如何 使用 仓库的数据

#### 提供数据
State提供唯一的公共数据源，所有共享的数据都要统一放到Store中的State中存储。
打开项目中的store.js文件，在state对象中可以添加我们要共享的数据。
```js
// 创建仓库 store
const store = new Vuex.Store({
  // state 状态, 即数据, 类似于vue组件中的data,
  // 区别：
  // 1.data 是组件自己的数据, 
  // 2.state 中的数据整个vue项目的组件都能访问到
  state: {
    count: 101
  }
})
```

#### 访问Vuex中的数据
问题: 如何在组件中获取count?
1. 通过$store直接访问 —> {{ $store.state.count }}
2. 通过辅助函数mapState 映射计算属性 —> {{ count }}

**通过$store访问的语法**
```
获取 store：
 1.Vue模板中获取 this.$store
 2.js文件中获取 import 导入 store

模板中：     {{ $store.state.xxx }}
组件逻辑中：  this.$store.state.xxx
JS模块中：   store.state.xxx
```

可以将state属性定义在计算属性中 [https://vuex.vuejs.org/zh/guide/state.html](https://vuex.vuejs.org/zh/guide/state.html)
```js
// <h1>state的数据 - {{ count }}</h1>

// 把state中数据，定义在组件内的计算属性中
  computed: {
    count () {
      return this.$store.state.count
    }
  }
```

#### 通过mapState辅助函数 (简化)
mapState是辅助函数，帮助我们把store中的数据 **自动** 映射到 组件的计算属性中, 它属于一种方便的用法
	![](assets/Pasted%20image%2020240205124951.png) 
`mapState(['count']) ` 
上面代码的最终得到的是 **类似于**
`count () { return this.$store.state.count }`

```js
import { mapState } from 'vuex'
export default {
  computed: {
    ...mapState(['count, title'])
  }
}
```

## 四、核心概念 - mutations
vuex 同样遵循单向数据流，**组件中不能直接修改仓库的数据**（可以修改但是不符合规范）
	![](assets/Pasted%20image%2020240205130256.png) 
开发中建议开启严格模式strict: true （监测消耗性能），上线后不需要开启

#### mutations 修改 state数据
**state数据的修改只能通过 mutations**
	![](assets/Pasted%20image%2020240205131502.png) 
```js
// 定义 mutations 对象，对象中存放修改 state 的方法
const store = new Vuex.Store({
  state: {
    count: 0
  },
  // 定义mutations
  mutations: {
    // 方法里参数 第一个参数是当前store的state属性
    // payload 载荷 运输参数 调用mutaiions的时候 可以传递参数 传递载荷
    addCount (state) {
      state.count += 1
    }
  }
})

// 组件中提交调用 mutations
this.$store.commit('addCount')
```

#### mutations 传参语法
提交 mutation 是可以传递参数的 `this.$store.commit( 'xxx', 参数 )`
	![](assets/Pasted%20image%2020240205133806.png) 

#### Vuex中的值和组件中的input双向绑定
![](assets/Pasted%20image%2020240205145918.png)

App.vue
```js
// <input :value="count" @input="handleInput" type="text">

export default {
  methods: {
    handleInput (e) {
      // 1. 实时获取输入框的值
      const num = +e.target.value
      // 2. 提交mutation，调用mutation函数
      this.$store.commit('changeCount', num)
    }
  }
}
```

store/index.js
```js
mutations: { 
   changeCount (state, newCount) {
      state.count = newCount
   }
},
```

#### 辅助函数 - mapMutations
mapMutations 和 mapState很像，它是把位于mutations中的方法提取了出来，映射到组件methods中
	![](assets/Pasted%20image%2020240206112445.png) 

## 五、核心概念 - actions
![](assets/Pasted%20image%2020240206113451.png)
mutations 必须是同步的 (便于监测数据变化，记录调试)
```js
// 提供 action 方法
// 注意：不能直接操作 state，操作 state，还是需要 commit mutation
actions: {
  // context 上下文 (此处未分模块，可以当成store仓库)
  // context.commit('mutation名字', 额外参数)
  changeCountAction (context, num) {
    // 这里是setTimeout模拟异步，以后大部分场景是发请求
    setTimeout(() => {
      context.commit('changeCount', num)
    }, 1000)
  }
},

// 页面中 dispatch 调用
this.$store.dispatch('changeCountAction',200)
```

#### 辅助函数 - mapActions
![](assets/Pasted%20image%2020240206115147.png)
```js
methods: {
  // mapMutations 和 mapActions 都是映射方法
  // 全局级别的映射
  ...mapMutations(['subCount', 'changeTitle']),
  ...mapActions(['changeCountAction']),
}
```


## 六、核心概念 - getters
![](assets/Pasted%20image%2020240206121256.png)
```js
// getters 类似于计算属性
getters: {
  // 注意点：
  // 1. 形参第一个参数，就是state
  // 2. 必须有返回值，返回值就是getters的值
  filterList (state) {
    return state.list.filter(item => item > 5)
  }
},

// mapState 和 mapGetters 都是映射属性
computed: {
    ...mapState(['count', 'user', 'setting']),
    ...mapGetters(['filterList']),
},
```


## 七、核心概念 - 模块 module (进阶语法)
![](assets/Pasted%20image%2020240206123145.png) 

### **模块定义** - 准备 state
![](assets/Pasted%20image%2020240206123424.png)
store/modules/user.js
```js
const state = {
  userInfo: {
    name: 'zs',
    age: 18
  }
}
const mutations = {}
const actions = {}
const getters = {}
export default {
  state,
  mutations,
  actions,
  getters
}
```
在`store/index.js`文件中的modules配置项中，注册模块
```js
import user from './modules/user'
import setting from './modules/setting'

const store = new Vuex.Store({
    modules:{
        user,
        setting
    }
})
```

### 获取模块内的state数据
![](assets/Pasted%20image%2020240206130300.png)
1. 直接通过模块名访问 $store.state.模块名.xxx
2. 通过 mapState 映射：
    1. 默认根级别的映射 `mapState([ 'xxx' ])`
    2. 子模块的映射 ：`mapState('模块名', ['xxx'])` - 需要开启命名空间 **`namespaced:true`**

```html
<!-- 直接通过模块名访问 $store.state.模块名.xxx -->
<div>{{ $store.state.user.userInfo.name }}</div>
<div>{{ $store.state.setting.theme }}</div>

<!-- 默认根级别的映射 mapState([ 'xxx' ]) -->
<div>{{ user.userInfo.name }}</div>
<div>{{ setting.theme }}</div>

<!-- 子模块的映射 ：mapState('模块名', ['xxx']) -->
<div>{{ userInfo.name }}</div>
<div>{{ theme }}</div>
```
```js
computed: {
  ...mapState(['count', 'user', 'setting']),
  ...mapState('user', ['userInfo']),
  ...mapState('setting', ['theme', 'desc']),
},
```

### 获取模块内的getters数据
![](assets/Pasted%20image%2020240206132239.png)
```js
const getters = {
  // 分模块后，state指代子模块的state
  UpperCaseName (state) {
    return state.userInfo.name.toUpperCase()
  }
}
```

### 获取模块内的 mutations actions 方法
![](assets/Pasted%20image%2020240206133536.png)
![](assets/Pasted%20image%2020240206135555.png)
```js
methods: {
  // 分模块的映射
  ...mapMutations('setting', ['setTheme']),
  ...mapMutations('user', ['setUser']),
  ...mapActions('user', ['setUserSecond'])
}
```

```js
const mutations = {
  setUser (state, newUserInfo) {
    state.userInfo = newUserInfo
  }
}
const actions = {
  setUserSecond (context, newUserInfo) {
    // 将异步在action中进行封装
    setTimeout(() => {
      // 调用mutation   context上下文，默认提交的就是自己模块的action和mutation
      context.commit('setUser', newUserInfo)
    }, 1000)
  }
}
```


## 综合案例 - 购物车
[vue-cart-demo.zip](assets/vue-cart-demo.zip)
![](assets/Pasted%20image%2020240206141847.png)

### 构建vuex-cart模块
![](assets/Pasted%20image%2020240206144918.png)
state写为返回一个对象的函数，是官方推荐的写法，可以保证每个模块间数据的独立性

### 基于 json-server 工具，准备后端接口服务环境
![](assets/Pasted%20image%2020240206150736.png)
https://www.npmjs.com/package/json-server
```sh
yarn global add json-server 
npm i json-server  -g
```

### 请求获取数据存入 vuex, 映射渲染
![](assets/Pasted%20image%2020240206153650.png)

### 修改数量功能完成
![](assets/Pasted%20image%2020240206155646.png)

### 底部 getters 统计
![](assets/Pasted%20image%2020240206163054.png)