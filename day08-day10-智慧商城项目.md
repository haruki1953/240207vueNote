# day08-day10-智慧商城项目
[day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)
![day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)
[MD笔记-智慧商城项目.pdf](assets/MD笔记-智慧商城项目.pdf)
[hm-shopping.zip](assets/hm-shopping.zip)

- **创建项目** ：基于 VueCli 自定义创建项目架子 
- **调整初始化目录** ：将目录调整成符合企业规范的目录 
- **vant 组件库** ：认识第三方 Vue组件库 vant-ui 
- **项目中的 vw 适配** ：基于 postcss 插件 实现项目 vw 适配 
- **路由设计配置** ：分析项目页面，设计路由，配置一级路由 
	阅读vant组件库文档，实现底部导航 tabbar 
- **登录页静态布局**  
- **request模块 - axios 封装** ：将 axios 请求方法，封装到  request 模块 
- **图形验证码功能完成** ：基于请求回来的 base64 图片，实现图形验证码功能 
- **api 接口模块 -封装图片验证码接口** ：将请求封装成方法，统一存放到 api 模块，与页面分离 
- **Toast 轻提示** 
- **短信验证倒计时** 
- **登录功能** ：封装api登录接口，实现登录功能 
- **响应拦截器统一处理错误提示** ：通过响应拦截器，统一处理接口的错误提示 
- **登录权证信息存储** ：vuex 构建 user 模块存储登录权证 (token & userId) 
- **storage存储模块 - vuex 持久化处理** ：封装 storage 存储模块，利用本地存储，进行 vuex 持久化处理

## 一、调整初始化目录
1. 删除 多余的文件 
2. 修改 路由配置 和 App.vue 
3. 新增 两个目录 api / utils 
	① api 接口模块：发送ajax请求的接口模块 
	② utils 工具模块：自己封装的一些工具方法模块

## 二、vant 组件库
目标：认识第三方 Vue组件库 vant-ui 
组件库：第三方 封装 好了很多很多的 组件，整合到一起就是一个组件库。 
https://vant-contrib.gitee.io/vant/v2/#/zh-CN/
**安装**
`npm i vant@latest-v2 -S`

**其他 Vue 组件库** 
① PC端： element-ui (element-plus) ant-design-vue 
② 移动端：vant-ui Mint UI (饿了么) Cube UI (滴滴)


### vant 全部导入 和 按需导入
全部导入(方便)
按需导入(性能)【推荐】

#### 全部导入
安装：yarn add vant@latest-v2
注册：
```js
import Vant from 'vant'; 
import 'vant/lib/index.css'; 

// 插件安装初始化：内部会将所有的vant所有组件进行导入注册
Vue.use(Vant);
```
使用测试： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/button
```html
<van-button type="primary">主要按钮</van-button>
<van-button type="info">信息按钮</van-button>
```

#### 按需导入
自动按需引入组件 (推荐)
安装：yarn add vant@latest-v2
安装插件：npm i babel-plugin-import -D
babel.config.js 中配置
```js
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
}
```
main.js 按需导入注册
```js
import Vue from 'vue'; 
import { Button } from 'vant'; 

Vue.use(Button);
```
使用测试：
```html
<van-button type="primary">主要按钮</van-button>
<van-button type="info">信息按钮</van-button>
```
将  `main.js 按需导入注册`  提取到 utils/vant-ui.js 中，main.js 导入
```js
// 导入按需导入的配置文件 
import '@/utils/vant-ui'
```

### 项目中的 vw 适配
目标：基于 postcss 插件 实现项目 vw 适配
Vant 默认使用 `px` 作为样式单位，如果需要使用 `viewport` 单位 (vw, vh, vmin, vmax)，推荐使用 [postcss-px-to-viewport](https://github.com/evrone/postcss-px-to-viewport) 进行转换。
[postcss-px-to-viewport](https://github.com/evrone/postcss-px-to-viewport) 是一款 PostCSS 插件，用于将 px 单位转化为 vw/vh 单位。
官方说明： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/advanced-usage

安装插件：yarn add postcss-px-to-viewport@1.1.1 -D
项目根目录， 新建postcss的配置文件`postcss.config.js`
```js
// postcss.config.js
module.exports = {
  plugins: {
    'postcss-px-to-viewport': {
      // vw适配的标准屏的宽度 iphoneX
      // 设计图 750，调成1倍 => 适配375标准屏幕
      // 设计图 640，调成1倍 => 适配320标准屏幕
      viewportWidth: 375,
    },
  },
};
```

## 三、路由设计配置
目标：分析项目页面，设计路由，配置一级路由
但凡是单个页面，独立展示的，都是一级路由
![](assets/Pasted%20image%2020240207103142.png)
每个一级路由新建为一个文件夹，文件夹里创建index.vue，导入时只写到文件夹即可
```js
const router = new VueRouter({
  routes: [
    { path: '/login', component: Login },
    {
      path: '/',
      component: Layout,
      redirect: '/home',
      children: [
        { path: '/home', component: Home },
        { path: '/category', component: Category },
        { path: '/cart', component: Cart },
        { path: '/user', component: User }
      ]
    },
    { path: '/search', component: Search },
    { path: '/searchlist', component: SearchList },
    // 动态路由传参，确认将来是哪个商品，路由参数中携带 id
    { path: '/prodetail/:id', component: ProDetail },
    { path: '/pay', component: Pay },
    { path: '/myorder', component: MyOrder }
  ]
})
```

### 根据vant组件库文档，实现底部导航 tabbar
![](assets/Pasted%20image%2020240207121520.png)
底部导航 tabbar： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/tabbar
图标： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/icon

### 基于底部导航，完成二级路由配置
配置二级路由 (规则 & 组件)
配置导航链接
配置路由出口
@/views/layout/index.vue
```html
<template>
  <div>
    <router-view></router-view>
    <van-tabbar route active-color="#ee0a24" inactive-color="#000">
      <van-tabbar-item to="/home" icon="wap-home-o">首页</van-tabbar-item>
      <van-tabbar-item to="/category" icon="apps-o">分类页</van-tabbar-item>
      <van-tabbar-item to="/cart" icon="shopping-cart-o">购物车</van-tabbar-item>
      <van-tabbar-item to="/user" icon="user-o">我的</van-tabbar-item>
    </van-tabbar>
  </div>
</template>
```
van-tabbar 加上 route属性 即可支持路由模式（vue-router），自动高亮


## 四、登录页静态布局
![](assets/Pasted%20image%2020240207125838.png)
头部导航条：van-nav-bar
https://vant-contrib.gitee.io/vant/v2/#/zh-CN/nav-bar
```html
<van-nav-bar title="会员登录" left-arrow @click-left="$router.go(-1)" />
```

`styles/common.less` 设置导航条返回箭头颜色，**通用样式覆盖**
```less
// 添加导航的通用样式
.van-nav-bar {
  .van-nav-bar__arrow {
    color: #333;
  }
}
```


## 五、request模块 - axios 封装
使用 axios 来请求后端接口, 一般都会对 axios 进行 一些配置 (比如: 配置基础地址，请求响应拦截器等) 
所以项目开发中, 都会对 axios 进行基本的二次封装, 单独封装到一个 request 模块中, 便于维护使用
接口文档： https://apifox.com/apidoc/shared-12ab6b18-adc2-444c-ad11-0e60f5693f66/doc-2221080
基地址： http://cba.itlike.com/public/index.php?s=/api/
![](assets/Pasted%20image%2020240207132651.png)

### axios 封装
utils/request.js
```js
/* 封装axios用于发送请求 */
import axios from 'axios'

// 创建 axios 实例，将来对创建出来的实例，进行自定义配置
// 好处：不会污染原始的 axios 实例
const request = axios.create({
  // 基地址
  baseURL: 'http://cba.itlike.com/public/index.php?s=/api/',
  timeout: 5000
})

// 自定义配置 - 请求/响应 拦截器
// 添加请求拦截器
request.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})

// 添加响应拦截器
request.interceptors.response.use(function (response) {
  // 2xx 范围内的状态码都会触发该函数。
  // 对响应数据做点什么 (默认axios会多包装一层data，需要响应拦截器中处理一下)
  return response.data
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error)
})

export default request
```

获取图形验证码，请求测试
```js
import request from '@/utils/request'
export default {
  name: 'LoginPage',
  async created () {
    const res = await request.get('/captcha/image')
    console.log(res)
  }
}
```

## 六、图形验证码功能完成
![](assets/Pasted%20image%2020240207141830.png)
```js
async created () {
  this.getPicCode()
},
data () {
  return {
    picUrl: '',
    picKey: ''
  }
},
methods: {
  // 获取图形验证码
  async getPicCode () {
    const { data: { base64, key } } = await request.get('/captcha/image')
    this.picUrl = base64
    this.picKey = key
  }
}
```


### api 接口模块 -封装图片验证码接口
![](assets/Pasted%20image%2020240207144226.png)
新建 `api/login.js` 提供获取图形验证码 Api 函数
```js
import request from '@/utils/request'

// 获取图形验证码
export const getPicCode = () => {
  return request.get('/captcha/image')
}
```
`login/index.vue`页面中调用测试
```js
import { getPicCode } from '@/api/login'

async getPicCode () {
  const { data: { base64, key } } = await getPicCode()
  this.picUrl = base64
  this.picKey = key
},
```

## 七、Toast 轻提示
![](assets/Pasted%20image%2020240207152001.png)
https://vant-contrib.gitee.io/vant/v2/#/zh-CN/toast

## 八、短信验证倒计时
![](assets/Pasted%20image%2020240207154949.png)
离开页面清除定时器

**封装接口** `api/login.js`
```js
// 获取短信验证码
export const getMsgCode = (captchaCode, captchaKey, mobile) => {
  return request.post('/captcha/sendSmsCaptcha', {
    form: {
      captchaCode,
      captchaKey,
      mobile
    }
  })
}
```


## 九、登录功能
![](assets/Pasted%20image%2020240207171319.png)


## 十、响应拦截器统一处理错误提示
![](assets/Pasted%20image%2020240207173637.png)
如果响应的status非200，最好抛出一个promise错误，await只会等待成功的promise，抛出错误之后便不会执行await之后的代码
`await getMsgCode(this.picCode, this.picKey, this.mobile)`


## 十一、登录权证信息存储 vuex + storage
![](assets/Pasted%20image%2020240207175430.png)
目标：vuex 构建 user 模块存储登录权证 (token & userId)
1. token 存入 vuex 的好处，易获取，响应式 
2. vuex 需要分模块 => user 模块

**新建 vuex user 模块 store/modules/user.js**
```js
export default {
  namespaced: true,
  state () {
    return {
      // 个人权证相关
      userInfo: {
        token: '',
        userId: ''
      },
    }
  },
  mutations: {
    // 所有mutations的第一个参数，都是state
    setUserInfo (state, obj) {
      state.userInfo = obj
    },
  },
  actions: {}
}
```
挂载到 vuex 上 store/index.js
```js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    user,
  }
})
```
页面中 commit 调用
```js
// 登录按钮（校验 & 提交）
async login () {
  if (!this.validFn()) {
    return
  }
  ...
  const res = await codeLogin(this.mobile, this.msgCode)
  this.$store.commit('user/setUserInfo', res.data)
  this.$router.push('/')
  this.$toast('登录成功')
}
```

### storage存储模块 - vuex 持久化处理
![](assets/Pasted%20image%2020240207200246.png)
1. 新建 `utils/storage.js` 封装方法
```js
const INFO_KEY = 'hm_shopping_info'

// 获取个人信息
export const getInfo = () => {
  const result = localStorage.getItem(INFO_KEY)
  return result ? JSON.parse(result) : {
    token: '',
    userId: ''
  }
}

// 设置个人信息
export const setInfo = (info) => {
  localStorage.setItem(INFO_KEY, JSON.stringify(info))
}

// 移除个人信息
export const removeInfo = () => {
  localStorage.removeItem(INFO_KEY)
}
```
2. vuex user 模块持久化处理 store/modules/user.js
```js
import { getInfo, setInfo } from '@/utils/storage'
export default {
  namespaced: true,
  state () {
    return {
      userInfo: getInfo()
    }
  },
  mutations: {
    setUserInfo (state, obj) {
      state.userInfo = obj
      setInfo(obj)
    }
  },
  actions: {}
}
```