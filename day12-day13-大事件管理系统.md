# day12-day13-大事件管理系统
[day12~day13-大事件管理系统-new.pdf](assets/day12~day13-大事件管理系统-new.pdf)
[MD笔记-大事件管理系统.pdf](assets/MD笔记-大事件管理系统.pdf)

在线演示：[https://fe-bigevent-web.itheima.net/login](https://fe-bigevent-web.itheima.net/login)

接口文档: [https://apifox.com/apidoc/shared-26c67aee-0233-4d23-aab7-08448fdf95ff/api-93850835](https://apifox.com/apidoc/shared-26c67aee-0233-4d23-aab7-08448fdf95ff/api-93850835)

**接口根路径：** [http://big-event-vue-api-t.itheima.net](http://big-event-vue-api-t.itheima.net/)

本项目的技术栈 本项目技术栈基于 [ES6](http://es6.ruanyifeng.com/)、[vue3](https://cn.vuejs.org/index.html)、[pinia](https://pinia.web3doc.top/)、[vue-router](https://router.vuejs.org/) 、vite 、axios 和 [element-plus](https://element-plus.org/)

![](assets/Pasted%20image%2020240211160643.png)
![](assets/Pasted%20image%2020240211160910.png)

## 一、pnpm 包管理器 - 创建项目
一些优势：比同类工具快2倍左右、节省磁盘空间... https://www.pnpm.cn/  
安装方式：npm install -g pnpm  
创建项目：pnpm create vue  
![](assets/Pasted%20image%2020240211161326.png)
![](assets/Pasted%20image%2020240211162238.png)

## 二、ESLint & prettier 配置代码风格
![](assets/Pasted%20image%2020240211162649.png)

**环境同步：**
1. **安装插件 ESlint，开启保存自动修复**
2. **禁用vscode插件 Prettier，并关闭保存自动格式化**
```jsx
// ESlint插件 + Vscode配置 实现自动格式化修复
"editor.codeActionsOnSave": {
    "source.fixAll": true
},
"editor.formatOnSave": false,
```

**配置文件 .eslintrc.cjs**  
1. prettier 风格配置 [https://prettier.io](https://prettier.io/docs/en/options.html )
   1. 单引号
   2. 不使用分号
   3. 每行宽度至多80字符
   4. 不加对象|数组最后逗号
   5. 换行符号不限制（win mac 不一致）

2. vue组件名称多单词组成（忽略index.vue）

3. props解构（关闭）

```jsx
  rules: {
    'prettier/prettier': [
      'warn',
      {
        singleQuote: true, // 单引号
        semi: false, // 无分号
        printWidth: 80, // 每行宽度至多80字符
        trailingComma: 'none', // 不加对象|数组最后逗号
        endOfLine: 'auto' // 换行符号不限制（win mac 不一致）
      }
    ],
    'vue/multi-word-component-names': [
      'warn',
      {
        ignores: ['index'] // vue组件名称多单词组成（忽略index.vue）
      }
    ],
    'vue/no-setup-props-destructure': ['off'], // 关闭 props 解构的校验
    // 💡 添加未定义变量错误提示，create-vue@3.6.3 关闭，这里加上是为了支持下一个章节演示。
    'no-undef': 'error'
  }
```


## 三、基于 husky 的代码检查工作流
husky 是一个 git hooks 工具 ( git的钩子工具，可以在特定时机执行特定的命令 )，提交前做代码检查  
**husky 配置**  
1. git初始化 git init
2. 初始化 husky 工具配置  https://typicode.github.io/husky/
```sh
pnpm dlx husky-init && pnpm install
```

3. 修改 .husky/pre-commit 文件
```sh
# npm test 删掉
pnpm lint
```

**问题：** 默认进行的是全量检查，耗时问题，历史问题。  

### 暂存区 eslint 校验
![](assets/Pasted%20image%2020240211172618.png)
只会对新添加的代码校验  
**lint-staged 配置**  
1. 安装
```jsx
pnpm i lint-staged -D
```
2. 配置 `package.json`
```jsx
{
  // ... 省略 ...
  "lint-staged": {
    "*.{js,ts,vue}": [
      "eslint --fix"
    ]
  }
}

{
  "scripts": {
    // ... 省略 ...
    "lint-staged": "lint-staged"
  }
}
```
3. 修改 .husky/pre-commit 文件
```jsx
pnpm lint-staged
```

## 四、调整项目目录
默认生成的目录结构不满足我们的开发需求，所以这里需要做一些自定义改动。主要是两个工作：
- 删除初始化的默认文件
- 修改剩余代码内容
- 新增调整我们需要的目录结构
- 拷贝初始化资源文件，安装预处理器插件

1. 删除文件
2. 修改内容
`src/router/index.js`
```jsx
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: []
})

export default router
```
`src/App.vue`
```jsx
<script setup></script>

<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<style scoped></style>
```
`src/main.js`
```jsx
import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(createPinia())
app.use(router)
app.mount('#app')
```

3. 新增需要目录 api utils
![](assets/Pasted%20image%2020240211194531.png)

4. 将项目需要的全局样式 和 图片文件，复制到 assets 文件夹中,  并将全局样式在main.js中引入
```jsx
import '@/assets/main.scss'
```
- 安装 sass 依赖
```jsx
pnpm add sass -D
```

## 五、vue-router4 路由代码解析
[router/index.js](project/day12-Vue3-big-event-admin/src/router/index.js)  
![](assets/Pasted%20image%2020240211194949.png)
![](assets/Pasted%20image%2020240211195031.png)
基础代码解析
```jsx
import { createRouter, createWebHistory } from 'vue-router'

// createRouter 创建路由实例，===> new VueRouter()
// 1. history模式: createWebHistory()   http://xxx/user
// 2. hash模式: createWebHashHistory()  http://xxx/#/user

// vite 的配置 import.meta.env.BASE_URL 是路由的基准地址，默认是 ’/‘
// https://vitejs.dev/guide/build.html#public-base-path

// 如果将来你部署的域名路径是：http://xxx/my-path/user
// vite.config.ts  添加配置  base: my-path，路由这就会加上 my-path 前缀了

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: []
})

export default router
```
import.meta.env.BASE_URL 是Vite 环境变量：[https://cn.vitejs.dev/guide/env-and-mode.html](https://cn.vitejs.dev/guide/env-and-mode.html)  

### 获取路由对象 - 跳转页面
[App.vue](project/day12-Vue3-big-event-admin/src/App.vue)  
```jsx
// 在 Vue3 CompositionAPI 中
// 1. 获取路由对象 router  useRouter
//    const router = useRouter()
// 2. 获取路由参数 route   useRoute
//    const route = useRoute()
import { useRoute, useRouter } from 'vue-router'
import { useUserStore, useCountStore } from '@/stores'
const router = useRouter()
const route = useRoute()

const goList = () => {
  router.push('/list')
  console.log(router, route)
}

<el-button @click="$router.push('/home')">跳首页</el-button>
<el-button @click="goList">跳列表页</el-button>
```



















