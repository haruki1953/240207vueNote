# day05-自定义指令&插槽&路由入门
## 一、自定义指令
- 内置指令：**v-html、v-if、v-bind、v-on**... 这都是Vue给咱们内置的一些指令，可以直接使用
- 自定义指令：同时Vue也支持让开发者，自己注册一些指令。这些指令被称为**自定义指令**
	通常**封装一些DOM操作**，扩展额外的功能

### 自定义指令语法
- 全局注册
```js
//在main.js中
Vue.directive('指令名', {
  "inserted" (el) {  // inserted 会在 指令所在的元素，被插入到页面中时触发
    // 可以对 el 标签，扩展额外功能
    el.focus()
  }
})
```
- 局部注册
```html
<script>
export default {
  //在Vue组件的配置项中
  directives: {
    "指令名": {
      inserted () {
        // 可以对 el 标签，扩展额外功能
        el.focus()
      }
    }
  }
}
</script>
```
- 使用指令
  注意：在使用指令的时候，一定要**先注册**，**再使用**，否则会报错
  使用指令语法： v-指令名。如：`<input type="text" v-focus/>`  
  **注册**指令时**不用**加**v-前缀**，但**使用时**一定要**加v-前缀**

**inserted：** 被绑定元素插入父节点时调用的钩子函数
**el：** 使用指令的那个DOM元素


### 指令的值
![](assets/Pasted%20image%2020240203120109.png)

1、在绑定指令时，可以通过“等号”的形式为指令 绑定 具体的参数值
`<div v-color="color">我是内容</div>`
2、通过 binding.value 可以拿到指令值，**指令值修改会 触发 update 函数**
```js
directives: {
  color: {
    // 1. inserted 提供的是元素被添加到页面中时的逻辑
    inserted (el, binding) {
      // console.log(el, binding.value);
      // binding.value 就是指令的值
      el.style.color = binding.value
    },
    // 2. update 指令的值修改的时候触发，提供值变化后，dom更新的逻辑
    update (el, binding) {
      console.log('指令的值修改了');
      el.style.color = binding.value
    }
  }
}
```

① v-指令名 = "指令值" ，通过 等号可以绑定指令的值
② 通过 binding.value 可以拿到指令的值
③ 通过 update 钩子，可以监听指令值的变化，进行dom更新操作


### v-loading指令的封装
**场景：** 实际开发过程中，发送请求需要时间，在请求的数据未回来时，页面会处于空白状态=> 用户体验不好
**需求：** 封装一个 v-loading 指令，实现加载中的效果

**分析**
1. 本质 loading效果就是一个蒙层，盖在了盒子上
2. 数据请求中，开启loading状态，添加蒙层
3. 数据请求完毕，关闭loading状态，移除蒙层

**实现**
1. 准备一个 loading类，通过伪元素定位，设置宽高，实现蒙层
2. 开启关闭 loading状态（添加移除蒙层），本质只需要添加移除类即可
3. 结合自定义指令的语法进行封装复用
```css
.loading:before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: #fff url("./loading.gif") no-repeat center;
}
```
```js
// 安装axios =>  yarn add axios
import axios from 'axios'

// 接口地址：http://hmajax.itheima.net/api/news
// 请求方式：get
export default {
  data () {
    return {
      list: [],
      isLoading: true,
      isLoading2: true
    }
  },
  async created () {
    // 1. 发送请求获取数据
    const res = await axios.get('http://hmajax.itheima.net/api/news')
    
    setTimeout(() => {
      // 2. 更新到 list 中，用于页面渲染 v-for
      this.list = res.data.data
      this.isLoading = false
    }, 2000)
  },
  directives: {
    loading: {
      inserted (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      },
      update (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      }
    }
  }
}
```
inserted 钩子中，binding.value 判断指令的值，设置默认状态
update 钩子中，binding.value 判断指令的值，更新类名状态


## 二、插槽
### 默认插槽
**作用：** 让组件内部的一些 结构 支持 自定义 
**需求：** 将需要多次显示的对话框, 封装成一个组件 
**问题：** 组件的内容部分，不希望写死，希望能使用的时候自定义。怎么办？
	![](assets/Pasted%20image%2020240203125237.png) 
#### 插槽的基本语法
1. 组件内需要定制的结构部分，改用`<slot></slot>`占位
2. 使用组件时, `<MyDialog></MyDialog>`标签内部, 传入结构替换slot
3. 给插槽传入内容时，可以传入**纯文本、html标签、组件**
	![](assets/Pasted%20image%2020240203125149.png) 


### 后备内容（默认值）
> 通过插槽完成了内容的定制，传什么显示什么, 但是如果不传，则是空白 
> 能否给插槽设置 默认显示内容 呢？

**插槽后备内容：** 封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）。
	![](assets/Pasted%20image%2020240203125928.png) 


### 具名插槽
需求：一个组件内有多处结构，需要外部传入标签，进行定制
**语法：** 多个slot使用name属性区分名字，template配合v-slot:名字来分发对应标签
	![](assets/Pasted%20image%2020240203130540.png) 
 一旦插槽起了名字，就是具名插槽，只支持定向分发
**v-slot的简写**，**v-slot —> #**
	![](assets/Pasted%20image%2020240203130639.png) 


### 作用域插槽
> 插槽只有两种，作用域插槽不属于插槽的一种分类，只是插槽的一个传参语法

定义slot 插槽的同时, 是可以**传值**的。给 **插槽** 上可以 **绑定数据**，将来 **使用组件时可以用**

场景：
	![](assets/Pasted%20image%2020240203132336.png) 

基本使用步骤：
	![](assets/Pasted%20image%2020240203132950.png) 

1. 给 slot 标签, 以 添加属性的方式传值
```html
<div v-for="(item, index) in data" :key="item.id">
  <slot :id="item.id" msg="测试文本"></slot>
</div>
```

2. 所有添加的属性, 都会被收集到一个对象中
	`{ id: 3, msg: '测试文本' }`

3. 在template中, 通过 ` #插槽名= "obj" ` 接收，默认插槽名为 default
```html
<MyTable :list="list">
  <template #default="obj">
    <button @click="del(obj.id)">删除</button>
  </template>
</MyTable>
```


## 综合案例：商品列表
[08-商品案例-组件封装.zip](assets/08-商品案例-组件封装.zip)
需求
	![](assets/Pasted%20image%2020240203143208.png) 

1. **my-tag 标签组件封装**
(1) 双击显示输入框，输入框获取焦点
(2) 失去焦点，隐藏输入框
(3) 回显标签信息
(4) 内容修改，回车 → 修改标签信息

2. **my-table 表格组件封装**
(1) 动态传递表格数据渲染
(2) 表头支持用户自定义
(3) 主体支持用户自定义


## 三、路由入门

### 单页应用程序
![](assets/Pasted%20image%2020240203185932.png)
![](assets/Pasted%20image%2020240203190100.png)


### 路由概念
单页面应用程序，之所以开发效率高，性能好，用户体验好
最大的原因就是：**页面按需更新**
要按需更新，首先就需要明确：**访问路径**和 **组件**的对应关系！**路由**

**Vue中的路由**：**路径和组件**的**映射**关系
	![](assets/Pasted%20image%2020240203191030.png) 

### VueRouter 的基本使用(5 + 2)
作用：修改地址栏路径时，切换显示匹配的组件 
说明：Vue 官方的一个路由插件，是一个第三方包 
官网：https://v3.router.vuejs.org/zh/

注意：
	Vue2 VueRouter3.x Vuex3.x
	Vue3 VueRouter4.x Vuex4.x

**固定5个固定的步骤**（不用死背，熟能生巧）
	![](assets/Pasted%20image%2020240203192103.png) 
```js
// 路由的使用步骤 5 + 2
// 5个基础步骤
// 1. 下载 v3.6.5
// yarn add vue-router@3.6.5
// 2. 引入
// 3. 安装注册 Vue.use(Vue插件)
// 4. 创建路由对象
// 5. 注入到new Vue中，建立关联

import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件初始化

const router = new VueRouter()

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

当我们配置完以上5步之后 就可以看到浏览器地址栏中的路由 变成了 /#/的形式。表示项目的路由已经被Vue-Router管理了

**2 个核心步骤**
	![](assets/Pasted%20image%2020240203192705.png) 

1. 创建需要的组件 (views目录)，配置路由规则
main.js
```js
import Find from './views/Find.vue'
import My from './views/My.vue'
import Friend from './views/Friend.vue'
const router = new VueRouter({
  routes: [
    { path: '/find', component: Find },
    { path: '/my', component: My },
    { path: '/friend', component: Friend },
  ]
})
```
2. 配置导航，配置路由出口(路径匹配的组件显示的位置)
App.vue
```html
<div class="footer_wrap">
  <a href="#/find">发现音乐</a>
  <a href="#/my">我的音乐</a>
  <a href="#/friend">朋友</a>
</div>
<div class="top">
  <router-view></router-view>
</div>
```


### 组件目录存放问题
注意： **.vue文件** 本质无区别
#### 组件分类
.vue文件分为2类，都是 **.vue文件（本质无区别）**
- 页面组件 （配置路由规则时使用的组件）
- 复用组件（多个组件中都使用到的组件）
	![](assets/Pasted%20image%2020240203200736.png) 

#### 存放目录
分类开来的目的就是为了 **更易维护**
1. src/views文件夹
    页面组件 - 页面展示 - 配合路由用
2. src/components文件夹
    复用组件 - 展示数据 - 常用于复用