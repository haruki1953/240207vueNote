# 项目起步

## 初始化项目并使用git管理
### 创建项目并精细化配置
![](assets/Pasted%20image%2020240518090434.png)
不太一样，没有用prettier

### src目录调整
![](assets/Pasted%20image%2020240518090658.png)
比以前多了，composables组合函数文件夹、directives全局指令文件夹、styles全局样式文件夹

### git 管理项目
1. git init
2. git add . 
3. git commit -m “init”


## 配置别名路径联想提示
在编写代码的过程中，一旦 输入 @/ , VSCode会立刻 联想出src下的所有子目录和文件, 统一文件路径访问不容易出错
![](assets/Pasted%20image%2020240518091408.png)
应该都是自动配置好了

> 以上只为辅助提示，实际的路径转换在vite.config.js
![](assets/Pasted%20image%2020240518091930.png)

## elementPlus引入、主题定制
通用型组件（ElementPlus），业务定制化组件（手写）
和这个是一样的 [day12-day13-大事件管理系统【六、引入 Element Plus 组件库】](../day12-day13-大事件管理系统.md#六、引入%20Element%20Plus%20组件库)

![](assets/Pasted%20image%2020240518094512.png)
- 安装sass npm i sass -D
- 准备定制文件 styles/element/index.scss
- 对elementPlus样式进行覆盖，通知Element采用scss语言 -> 导入定制scss文件覆

### 准备定制化的样式文件
styles/element/index.scss
```scss
/* 只需要重写你需要的即可 */  
@forward 'element-plus/theme-chalk/src/common/var.scss' with (  
  $colors: (  
    'primary': (  
      // 主色  
      'base': #27ba9b,  
    ),  
    'success': (  
      // 成功色  
      'base': #1dc779,  
    ),  
    'warning': (  
      // 警告色  
      'base': #ffb302,  
    ),  
    'danger': (  
      // 危险色  
      'base': #e26237,  
    ),  
    'error': (  
      // 错误色  
      'base': #cf4444,  
    ),  
  )  
)
```

### 自动导入配置
> 这里自动导入需要深入到elementPlus的组件中，按照官方的配置文档来
>
> 1. 自动导入定制化样式文件进行样式覆盖
> 2. 按需定制主题配置 （需要安装 unplugin-element-plus）

```javascript
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
// 导入对应包
import ElementPlus from 'unplugin-element-plus/vite'
export default defineConfig({
  plugins: [
    vue(),
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    // 1. 配置elementPlus采用sass样式配色系统
    Components({
      resolvers: [ElementPlusResolver({ importStyle: "sass" })],
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  css: {
    preprocessorOptions: {
      scss: {
        // 2. 自动导入定制化样式文件进行样式覆盖
        additionalData: `
          @use "@/styles/element/index.scss" as *;
        `,
      }
    }
  }
})
```

## axios基础配置
![](assets/Pasted%20image%2020240518095823.png)
大致一样，稍有区别l，没有做过多的错误判断
utils/http.js
```js
// axios基础的封装
import axios from 'axios'
import { ElMessage } from 'element-plus'
import { useUserStore } from '@/stores/userStore'
const httpInstance = axios.create({
  baseURL: 'http://pcapi-xiaotuxian-front-devtest.itheima.net',
  timeout: 5000
})

// 拦截器

// axios请求拦截器
httpInstance.interceptors.request.use(config => {
  // 1. 从pinia获取token数据
  const userStore = useUserStore()
  // 2. 按照后端的要求拼接token数据
  const token = userStore.userInfo.token
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
}, e => Promise.reject(e))

// axios响应式拦截器
httpInstance.interceptors.response.use(res => res.data, e => {
  // 统一错误提示
  ElMessage({
    type: 'warning',
    message: e.response.data.message
  })
  return Promise.reject(e)
})


export default httpInstance
```

## 路由整体设计
大致一样

## 静态资源引入和Error Lens安装
![](assets/Pasted%20image%2020240518103430.png)
[styles/common.scss](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/styles/common.scss)
main.js引入 `import '@/styles/common.scss'`

主样式文件以前是在assets文件夹

**error lens 安装**
![](assets/Pasted%20image%2020240518104113.png)

## scss文件自动导入、scss变量
在项目里一些组件共享的色值会以scss变量的方式统一放到一个名为 var.scss 的文件中，正常组件中使用，需要先导入scss文件，再使用内部的变量，比较繁琐，自动导入可以免去手动导入的步骤，直接使用内部的变量
![](assets/Pasted%20image%2020240518105517.png)
.新增一个 styles/var.scss 文件，存入色值变量
通过 vite.config.js 配置自动导入文件
```js
  css: {
    preprocessorOptions: {
      scss: {
        // 2. 自动导入定制化样式文件进行样式覆盖
        additionalData: `
          @use "@/styles/element/index.scss" as *;
          @use "@/styles/var.scss" as *;
        `,
      }
    }
  }
```


# Layout
## 静态模版结构搭建 
![](assets/Pasted%20image%2020240518110256.png)
![](assets/Pasted%20image%2020240518110240.png)

## 字体图标引入
阿里的字体图标库支持多种引入方式，小兔鲜项目里采用的是 font-class引用 的方式
index.html
```html
<link rel="stylesheet" href="//at.alicdn.com/t/font_2143783_iq6z4ey5vu.css">
```

## 一级导航渲染

使用后端接口渲染渲染一级路由导航
1. 根据接口文档封装接口函数
2. 发送请求获取数据列表
3. v-for渲染页面
![](assets/Pasted%20image%2020240518111521.png)

接口文档 https://apifox.com/apidoc/shared-fa9274ac-362e-4905-806b-6135df6aa90e/api-24945669

封装api后，暂时直接在页面onMounted调用了

## 吸顶导航交互实现
![](assets/Pasted%20image%2020240518113441.png)
[views/Layout/components/LayoutFixed.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Layout/components/LayoutFixed.vue)

使用show类名控制显示隐藏
```scss
.app-header-sticky {
  width: 100%;
  height: 80px;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 999;
  background-color: #fff;
  border-bottom: 1px solid #e4e4e4;
  // 此处为关键样式!!!
  // 状态一：往上平移自身高度 + 完全透明
  transform: translateY(-100%);
  opacity: 0;

  // 状态二：移除平移 + 完全不透明
  &.show {
    transition: all 0.3s linear;
    transform: none;
    opacity: 1;
  }
  // ...
}
```

### SCSS父选择器引用符号`&`
在 SCSS 中，`&` 是一个父选择器引用符号，它代表嵌套规则中当前选择器的父选择器。允许你在嵌套的上下文中引用外层选择器，并在复杂的选择器结构中避免重复编写相同的选择器。[【散装笔记】SCSS父选择器引用符号`&`](../笔记/散装笔记.md#SCSS父选择器引用符号`&`)

### VueUse
https://vueuse.org/
[VueUse](https://vueuse.org/) 是一个基于 Vue Composition API 的工具库，包含一组实用的 Composition API 钩子函数。这些钩子函数旨在简化常见的开发任务，例如状态管理、事件监听、传感器集成等。
`useScroll` 是一个由 `@vueuse/core` 提供的钩子，使用 `useScroll` 可以轻松地获取滚动位置的实时数据，而无需手动添加和管理事件监听器。
```js
import { useScroll } from '@vueuse/core'
const { y } = useScroll(window)
```
[【散装笔记】VueUse](../笔记/散装笔记.md#VueUse)
https://vueuse.org/core/useScroll/#usescroll

## Pinia优化重复请求
俩个导航中的列表是完全一致的，但是要发送俩次网络请求，存在浪费。通过Pinia集中管理数据，再把数据给组件使用
和自己e5share项目前端的思路一样
![](assets/Pasted%20image%2020240518122037.png)
在layout触发请求，组件中使用 [stores/categoryStore.js](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/stores/categoryStore.js) 
他的模块还没有统一管理


# Home
## 整体结构搭建和分类实现
![](assets/Pasted%20image%2020240518123744.png)
按照结构新增五个组件
- HomeCategory
- HomeBanner
- HomeNew
- HomeHot
- HomeProduct

## HomeCategory
左侧分类，使用Pinia中的数据渲染
[views/Home/components/HomeCategory.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Home/components/HomeCategory.vue)

## banner轮播图实现
![](assets/Pasted%20image%2020240518124935.png)
el-carousel

## 面板组件封装
[views/Home/components/HomePanel.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Home/components/HomePanel.vue)
组件封装，解决：1. 复用问题 2. 业务维护问题
![](assets/Pasted%20image%2020240518152744.png)

核心思路：把可复用的结构只写一次，把可能发生变化的部分抽象成组件参数（props / 插槽）

实现步骤 
1. 不做任何抽象，准备静态模版 
2. 抽象可变的部分 
	- 主标题和副标题是纯文本，可以抽象成prop传入 
	- 主体内容是复杂的模版，抽象成插槽传入

## 新鲜好物和人气推荐实现

## 图片懒加载指令实现
![](assets/Pasted%20image%2020240518161239.png)
![](assets/Pasted%20image%2020240518161329.png)
![](assets/Pasted%20image%2020240518161939.png)

### Vue3自定义指令语法
在 Vue 3 中，你可以使用 `app.directive` 方法来注册自定义指令。自定义指令可以包含多个生命周期钩子函数，如 `created`、`beforeMount`、`mounted`、`beforeUpdate`、`updated`、`beforeUnmount` 和 `unmounted`。
[【散装笔记】Vue3自定义指令语法](../笔记/散装笔记.md#Vue3自定义指令语法)

### useIntersectionObserver
`useIntersectionObserver` 是 VueUse 提供的一个钩子，用于监听 DOM 元素的可见性变化。它是对浏览器原生 `IntersectionObserver` API 的封装，简化了在 Vue 3 应用中使用该功能的过程。这个钩子非常适合实现懒加载、无限滚动、元素进入视口时触发动画等功能。
[【散装笔记】useIntersectionObserver](../笔记/散装笔记.md#useIntersectionObserver)

### 懒加载指令优化
**逻辑书写位置不合理 & 重复监听问题**
直接写到入口文件中不合理，入口文件通常只做一些初始化的事情，不应该包含太多的逻辑代码，可以通过插件的方法把懒加载指令封装为插件，main.js入口文件只需要负责注册插件即可
![](assets/Pasted%20image%2020240518170158.png)

useIntersectionObserver对于元素的监听是一直存在的，除非手动停止监听，存在内存浪费。stop方法，在监听的图片第一次完成加载之后就停止监听

[【散装笔记】vue3组合式api插件写法](../笔记/散装笔记.md#vue3组合式api插件写法)
directives/index.js
```js
// 定义懒加载插件
import { useIntersectionObserver } from '@vueuse/core'

export const lazyPlugin = {
  install (app) {
    // 懒加载指令逻辑
    app.directive('img-lazy', {
      mounted (el, binding) {
        // el: 指令绑定的那个元素 img
        // binding: binding.value  指令等于号后面绑定的表达式的值  图片url
        // console.log(el, binding.value)
        // stop方法，在监听的图片第一次完成加载之后就停止监听
        const { stop } = useIntersectionObserver(
          el,
          ([{ isIntersecting }]) => {
            console.log(isIntersecting)
            if (isIntersecting) {
              // 进入视口区域
              el.src = binding.value
              stop()
            }
          },
        )
      }
    })
  }
}
```


## Product产品列表实现
![](assets/Pasted%20image%2020240518171640.png)

## GoodsItem组件封装
![](assets/Pasted%20image%2020240518172227.png)
![](assets/Pasted%20image%2020240518172249.png)

