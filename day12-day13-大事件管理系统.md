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


## 六、引入 Element Plus 组件库
**官方文档：** https://element-plus.org/zh-CN/
- 安装
```sh
$ pnpm add element-plus
```

**自动按需：**
1. 安装插件
```sh
pnpm add -D unplugin-vue-components unplugin-auto-import
```

2. 然后把下列代码插入到你的 `Vite` 或 `Webpack` 的配置文件中
```jsx
...
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    ...
    AutoImport({
      resolvers: [ElementPlusResolver()]
    }),
    Components({
      resolvers: [ElementPlusResolver()]
    })
  ]
})

```
[vite.config.js](project/day12-Vue3-big-event-admin/vite.config.js)  

3. 直接使用
```jsx
<template>
  <div>
    <el-button type="primary">Primary</el-button>
    <el-button type="success">Success</el-button>
    <el-button type="info">Info</el-button>
    <el-button type="warning">Warning</el-button>
    <el-button type="danger">Danger</el-button>
    ...
  </div>
</template>
```
![](assets/Pasted%20image%2020240212091028.png)
[App.vue](project/day12-Vue3-big-event-admin/src/App.vue)  
**彩蛋：** 默认 components 下的文件也会被自动注册~


## 七、Pinia 构建仓库 和 持久化
[stores/index.js](project/day12-Vue3-big-event-admin/src/stores/index.js)  
[stores/modules/user.js](project/day12-Vue3-big-event-admin/src/stores/modules/user.js)  
![](assets/Pasted%20image%2020240212092439.png)
官方文档： https://prazdevs.github.io/pinia-plugin-persistedstate/zh/

1. 安装插件 pinia-plugin-persistedstate
```sh
pnpm add pinia-plugin-persistedstate -D
```

2. 使用 main.js
```jsx
import persist from 'pinia-plugin-persistedstate'
...
app.use(createPinia().use(persist))
```

3. 配置 stores/user.js
```jsx
import { defineStore } from 'pinia'
import { ref } from 'vue'

// 用户模块
export const useUserStore = defineStore(
  'big-user',
  () => {
    const token = ref('') // 定义 token
    const setToken = (t) => (token.value = t) // 设置 token

    return { token, setToken }
  },
  {
    persist: true // 持久化
  }
)
```


## 八、Pinia 仓库统一管理
[stores/index.js](project/day12-Vue3-big-event-admin/src/stores/index.js)  
[stores/modules/user.js](project/day12-Vue3-big-event-admin/src/stores/modules/user.js)  

**pinia 独立维护**
- 现在：初始化代码在 main.js 中，仓库代码在 stores 中，代码分散职能不单一
- 优化：由 stores 统一维护，在 stores/index.js 中完成 pinia 初始化，交付 main.js 使用

**仓库 统一导出**
- 现在：使用一个仓库 `import { useUserStore } from ./stores/user.js` 不同仓库路径不一致
- 优化：由 stores/index.js 统一导出，导入路径统一 `./stores`，而且仓库维护在 stores/modules 中
[stores/index.js](project/day12-Vue3-big-event-admin/src/stores/index.js)  
```js
import { createPinia } from 'pinia'
import persist from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(persist)

export default pinia
export * from './modules/user'
export * from './modules/counter'

// import { useUserStore } from './modules/user'
// export { useUserStore }
// import { useCountStore } from './modules/counter'
// export { useCountStore }
```


## 九、数据交互 - 请求工具设计
[utils/request.js](project/day12-Vue3-big-event-admin/src/utils/request.js)  
### 创建 axios 实例
使用 axios 来请求后端接口, 一般都会对 axios 进行一些配置 (比如: 配置基础地址等)  
一般项目开发中, 都会对 axios 进行基本的二次封装, 单独封装到一个模块中, 便于使用

1. 安装 axios
```
pnpm add axios
```

2. 新建 `utils/request.js` 封装 axios 模块
    利用 axios.create 创建一个自定义的 axios 来使用  
    http://www.axios-js.com/zh-cn/docs/#axios-create-config  
```js
import axios from 'axios'

const baseURL = 'http://big-event-vue-api-t.itheima.net'

const instance = axios.create({
  // TODO 1. 基础地址，超时时间
})

instance.interceptors.request.use(
  (config) => {
    // TODO 2. 携带token
    return config
  },
  (err) => Promise.reject(err)
)

instance.interceptors.response.use(
  (res) => {
    // TODO 3. 处理业务失败
    // TODO 4. 摘取核心响应数据
    return res
  },
  (err) => {
    // TODO 5. 处理401错误
    return Promise.reject(err)
  }
)

export default instance
```

### 完成 axios 基本配置
#### 请求拦截器中携带token - 访问pinia仓库
#### 响应拦截器中提示错误 - ElMessage
https://element-plus.org/zh-CN/component/message.html
```js
import axios from 'axios'
import { useUserStore } from '@/stores'
import { ElMessage } from 'element-plus'
import router from '@/router'
const baseURL = 'http://big-event-vue-api-t.itheima.net'

const instance = axios.create({
  // TODO 1. 基础地址，超时时间
  baseURL,
  timeout: 10000
})

// 请求拦截器
instance.interceptors.request.use(
  (config) => {
    // TODO 2. 携带token
    const useStore = useUserStore()
    if (useStore.token) {
      config.headers.Authorization = useStore.token
    }
    return config
  },
  (err) => Promise.reject(err)
)

// 响应拦截器
instance.interceptors.response.use(
  (res) => {
    // TODO 4. 摘取核心响应数据
    if (res.data.code === 0) {
      return res
    }
    // TODO 3. 处理业务失败
    // 处理业务失败, 给错误提示，抛出错误
    ElMessage.error(res.data.message || '服务异常')
    return Promise.reject(res.data)
  },
  (err) => {
    // TODO 5. 处理401错误
    // 错误的特殊情况 => 401 权限不足 或 token 过期 => 拦截到登录
    if (err.response?.status === 401) {
      router.push('/login')
    }

    // 错误的默认情况 => 只要给提示
    ElMessage.error(err.response.data.message || '服务异常')
    return Promise.reject(err)
  }
)

export default instance
export { baseURL }
```


## 十、整体路由设计
![](assets/Pasted%20image%2020240212133317.png)

|path|文件|功能|组件名|路由级别|
|---|---|---|---|---|
|/login|views/login/LoginPage.vue|登录&注册|LoginPage|一级路由|
|/|views/layout/LayoutContainer.vue|布局架子|LayoutContainer|一级路由|
|├─ /article/manage|views/article/ArticleManage.vue|文章管理|ArticleManage|二级路由|
|├─ /article/channel|views/article/ArticleChannel.vue|频道管理|ArticleChannel|二级路由|
|├─ /user/profile|views/user/UserProfile.vue|个人详情|UserProfile|二级路由|
|├─ /user/avatar|views/user/UserAvatar.vue|更换头像|UserAvatar|二级路由|
|├─ /user/password|views/user/UserPassword.vue|重置密码|UserPassword|二级路由|

[router/index.js](project/day12-Vue3-big-event-admin/src/router/index.js)


## 十一、登录注册页面 【element-plus 表单 & 表单校验】
![](assets/Pasted%20image%2020240212135344.png)
[views/login/LoginPage.vue](project/day13-Vue3-big-event-admin/src/views/login/LoginPage.vue)

### 注册登录 静态结构 & 基本切换
![](assets/Pasted%20image%2020240211160910.png)
安装 element-plus 图标库 `pnpm i @element-plus/icons-vue`
表单： https://element-plus.org/zh-CN/component/form.html
```
1. 结构相关
  el-row表示一行，一行分成24份 
   el-col表示列  
   (1) :span="12"  代表在一行中，占12份 (50%)
   (2) :span="6"   表示在一行中，占6份  (25%)
   (3) :offset="3" 代表在一行中，左侧margin份数

   el-form 整个表单组件
   el-form-item 表单的一行 （一个表单域）
   el-input 表单元素（输入框）
   
2. 校验相关
   (1) el-form => :model="ruleForm"      绑定的整个form的数据对象 { xxx, xxx, xxx }
   (2) el-form => :rules="rules"         绑定的整个rules规则对象  { xxx, xxx, xxx }
   (3) 表单元素 => v-model="ruleForm.xxx" 给表单元素，绑定form的子属性
   (4) el-form-item => prop配置生效的是哪个校验规则 (和rules中的字段要对应)
```

### 注册功能
[views/login/LoginPage.vue](project/day13-Vue3-big-event-admin/src/views/login/LoginPage.vue)  
#### 实现注册校验
【需求】注册页面基本校验
1. 用户名非空，长度校验5-10位
2. 密码非空，长度校验6-15位
3. 再次输入密码，非空，长度校验6-15位

【进阶】再次输入密码需要自定义校验规则，和密码框值一致（可选）

1. model 属性绑定 form 数据对象
```jsx
const formModel = ref({
  username: '',
  password: '',
  repassword: ''
})

<el-form :model="formModel" >
```

2. v-model 绑定 form 数据对象的子属性
```jsx
<el-input
  v-model="formModel.username"
  :prefix-icon="User"
  placeholder="请输入用户名"
></el-input>
... 
(其他两个也要绑定)
```

3. rules 配置校验规则
```jsx
<el-form :rules="rules" >

// 整个表单的校验规则
// 1. 非空校验 required: true      message消息提示，  trigger触发校验的时机 blur change
// 2. 长度校验 min:xx, max: xx
// 3. 正则校验 pattern: 正则规则    \S 非空字符
// 4. 自定义校验 => 自己写逻辑校验 (校验函数)
//    validator: (rule, value, callback)
//    (1) rule  当前校验规则相关的信息
//    (2) value 所校验的表单元素目前的表单值
//    (3) callback 无论成功还是失败，都需要 callback 回调
//        - callback() 校验成功
//        - callback(new Error(错误信息)) 校验失败
const rules = {
  username: [
    { required: true, message: '请输入用户名', trigger: 'blur' },
    { min: 5, max: 10, message: '用户名必须是5-10位的字符', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    {
      pattern: /^\S{6,15}$/,
      message: '密码必须是6-15位的非空字符',
      trigger: 'blur'
    }
  ],
  repassword: [
    { required: true, message: '请再次输入密码', trigger: 'blur' },
    {
      pattern: /^\S{6,15}$/,
      message: '密码必须是6-15的非空字符',
      trigger: 'blur'
    },
    {
      validator: (rule, value, callback) => {
        if (value !== formModel.value.password) {
          callback(new Error('两次输入密码不一致!'))
        } else {
          callback()
        }
      },
      trigger: 'blur'
    }
  ]
}
```

4. prop 绑定校验规则
```jsx
<el-form-item prop="username">
  <el-input
    v-model="formModel.username"
    :prefix-icon="User"
    placeholder="请输入用户名"
  ></el-input>
</el-form-item>
... 
(其他两个也要绑定prop)
```

#### 注册前的预校验
https://element-plus.org/zh-CN/component/form.html#form-exposes
需求：点击注册按钮，注册之前，需要先校验

1. 通过 ref 获取到 表单组件
```jsx
const form = ref()

<el-form ref="form">
```

2. 注册之前进行校验
```jsx
<el-button
  @click="register"
  class="button"
  type="primary"
  auto-insert-space
>
  注册
</el-button>

const register = async () => {
  // 注册成功之前，先进行校验，校验成功 → 请求，校验失败 → 自动提示
  await form.value.validate()
  console.log('开始注册请求')
}
```


#### 封装 api 实现注册功能
[api/user.js](project/day13-Vue3-big-event-admin/src/api/user.js)  
需求：封装注册api，进行注册，注册成功切换到登录

1. 新建 api/user.js 封装
```jsx
import request from '@/utils/request'

export const userRegisterService = ({ username, password, repassword }) =>
  request.post('/api/reg', { username, password, repassword })
```

2. 页面中调用
```jsx
const register = async () => {
  await form.value.validate()
  await userRegisterService(formModel.value)
  ElMessage.success('注册成功')
  // 切换到登录
  isRegister.value = false
}
```
ElMessage已被自动按需导入

3. eslintrc 中声明全局变量名,  解决 ElMessage 报错问题
```jsx
module.exports = {
  ...
  globals: {
    ElMessage: 'readonly',
    ElMessageBox: 'readonly',
    ElLoading: 'readonly'
  }
}
```


### 登录功能
[views/login/LoginPage.vue](project/day13-Vue3-big-event-admin/src/views/login/LoginPage.vue)  
#### 实现登录校验
【需求说明】给输入框添加表单校验
1. 用户名不能为空，用户名必须是5-10位的字符，失去焦点 和 修改内容时触发校验
2. 密码不能为空，密码必须是6-15位的字符，失去焦点 和 修改内容时触发校验

操作步骤：

1. model 属性绑定 form 数据对象，直接绑定之前提供好的数据对象即可
```jsx
<el-form :model="formModel" >
```

2. rules 配置校验规则，共用注册的规则即可
```jsx
<el-form :rules="rules" >
```

3. v-model 绑定 form 数据对象的子属性
```jsx
<el-input
  v-model="formModel.username"
  :prefix-icon="User"
  placeholder="请输入用户名"
></el-input>

<el-input
  v-model="formModel.password"
  name="password"
  :prefix-icon="Lock"
  type="password"
  placeholder="请输入密码"
></el-input>
```

4. prop 绑定校验规则
```jsx
<el-form-item prop="username">
  <el-input
    v-model="formModel.username"
    :prefix-icon="User"
    placeholder="请输入用户名"
  ></el-input>
</el-form-item>
... 
```

5. 切换的时候重置
```jsx
watch(isRegister, () => {
  formModel.value = {
    username: '',
    password: '',
    repassword: ''
  }
})
```

#### 登录前的预校验 & 登录成功

【需求说明1】登录之前的预校验
- 登录请求之前，需要对用户的输入内容，进行校验
- 校验通过才发送请求

【需求说明2】**登录功能**
1. 封装登录API，点击按钮发送登录请求
2. 登录成功存储token，存入pinia 和 持久化本地storage
3. 跳转到首页，给提示

实现步骤：

1.  注册事件，进行登录前的预校验 (获取到组件调用方法)
```jsx
<el-form ref="form">
    
const login = async () => {
  await form.value.validate()
  console.log('开始登录')
}
```

2. 封装接口 API
```jsx
export const userLoginService = ({ username, password }) =>
  request.post('api/login', { username, password })
```

3. 调用方法将 token 存入 pinia 并 自动持久化本地
```jsx
const userStore = useUserStore()
const router = useRouter()
 
const login = async () => {
  await form.value.validate()
  const res = await userLoginService(formModel.value)
  userStore.setToken(res.data.token)
  ElMessage.success('登录成功')
  router.push('/')
}
```









