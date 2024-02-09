# day08-day10-智慧商城项目
[day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)  
![day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)
[MD笔记-智慧商城项目.pdf](assets/MD笔记-智慧商城项目.pdf)  
[hm-shopping.zip](assets/hm-shopping.zip)  
[project/hm-shopping](project/hm-shopping/)  

接口文档： https://apifox.com/apidoc/shared-12ab6b18-adc2-444c-ad11-0e60f5693f66/doc-2221080  
基地址： http://cba.itlike.com/public/index.php?s=/api/  

- **vant 组件库** ：认识第三方 Vue组件库 vant-ui 
- **项目中的 vw 适配** ：基于 postcss 插件 实现项目 vw 适配 
- **request模块 - axios 封装** ：将 axios 请求方法，封装到  request 模块 
- **图形验证码功能完成** ：基于请求回来的 base64 图片，实现图形验证码功能 
- **api 接口模块 -封装图片验证码接口** ：将请求封装成方法，统一存放到 api 模块，与页面分离 
- **Toast 轻提示** 
- **短信验证倒计时** 
- **登录功能** ：封装api登录接口，实现登录功能 
- **响应拦截器统一处理错误提示** ：通过响应拦截器，统一处理接口的错误提示 
- **登录权证信息存储** ：vuex 构建 user 模块存储登录权证 (token & userId) 
- **storage存储模块 - vuex 持久化处理** ：封装 storage 存储模块，利用本地存储，进行 vuex 持久化处理
- **添加请求 loading 效果** 统一在每次请求后台时，添加 loading 效果
- **页面访问拦截** ：基于全局前置守卫，进行页面访问拦截处理
- **搜索 - 历史记录管理** ：通过storage实现持久化（可以不用vuex）
- **唤起弹层** ：ActionSheet 动作面板
- **判断 token 添加登录提示** ：Dialog 弹出框
- **页面回跳** ：$router.replace
- **封装接口进行请求** ：请求拦截器统一携带 token


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

## 三、【路由设计配置】
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


## 四、【登录页】静态布局
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

## 十二、添加请求 loading 效果
![](assets/Pasted%20image%2020240208094821.png)

1. 请求拦截器中，每次请求，打开 loading 
```js
// 添加请求拦截器
request.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  // 开启loading，禁止背景点击 (节流处理，防止多次无效触发)
  Toast.loading({
    message: '加载中...',
    forbidClick: true, // 禁止背景点击
    loadingType: 'spinner', // 配置loading图标
    duration: 0 // 不会自动消失
  })
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})
```

2. 响应拦截器中，每次响应，关闭 loading
```js
// 添加响应拦截器
request.interceptors.response.use(function (response) {
  const res = response.data
  if (res.status !== 200) {
    // 给错误提示, Toast 默认是单例模式，后面的 Toast调用了，会将前一个 Toast 效果覆盖。同时只能存在一个 Toast
    Toast(res.message)
    // 抛出一个错误的promise
    return Promise.reject(res.message)
  } else {
    // 正确情况，直接走业务核心逻辑，清除loading效果
    Toast.clear()
  }
  // 对响应数据做点什么
  return res
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error)
})
```


## 十三、页面访问拦截
![](assets/Pasted%20image%2020240208101325.png)
![](assets/Pasted%20image%2020240208101513.png)
router/index.js
```js
// 定义一个数组，专门用户存放所有需要权限访问的页面
const authUrls = ['/pay', '/myorder']

router.beforeEach((to, from, next) => {
  // console.log(to, from, next)
  // 看 to.path 是否在 authUrls 中
  if (!authUrls.includes(to.path)) {
    // 非权限页面，直接放行
    next()
    return
  }

  // 是权限页面，需要判断token
  const token = store.getters.token
  if (token) {
    next()
  } else {
    next('/login')
  }
})
```


## 十四、【首页】 - 静态结构准备 & 动态渲染
![](assets/Pasted%20image%2020240208112937.png)
首页： [views/layout/home.vue](project/hm-shopping/src/views/layout/home.vue)  
组件： [components/GoodsItem.vue](project/hm-shopping/src/components/GoodsItem.vue)  
接口封装： [api/home.js](project/hm-shopping/src/api/home.js)  

## 十五、【搜索】 - 历史记录管理
![](assets/Pasted%20image%2020240208130945.png)
[views/search/index.vue](project/hm-shopping/src/views/search/index.vue)  
Search 搜索： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/search  
通过storage实现持久化（可以不用vuex）： [utils/storage.js](project/hm-shopping/src/utils/storage.js)  

## 十六、【搜索列表】 - 静态布局 & 动态渲染
![](assets/Pasted%20image%2020240208135533.png)
[views/search/list.vue](project/hm-shopping/src/views/search/list.vue)  
[api/product.js](project/hm-shopping/src/api/product.js)

## 十七、【商品详情】- 静态布局 & 渲染
![](assets/Pasted%20image%2020240208154058.png)
[views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue)

## 十八、加入购物车
### 唤起弹层
![](assets/Pasted%20image%2020240208161826.png)
ActionSheet 动作面板： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/action-sheet  
[views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue)

### 封装数字框组件
![](assets/Pasted%20image%2020240208164751.png)
[components/CountBox.vue](project/hm-shopping/src/components/CountBox.vue)

### 判断 token 添加登录提示，登录回跳
![](assets/Pasted%20image%2020240208190551.png)
Dialog 弹出框： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/dialog  
[views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue) 添加 token 鉴权判断，跳转携带回跳地址
```js
async addCart () {
  // 判断用户是否有登录
  if (!this.$store.getters.token) {
    this.$dialog.confirm({
      title: '温馨提示',
      message: '此时需要先登录才能继续操作哦',
      confirmButtonText: '去登录',
      cancelButtonText: '再逛逛'
    })
      .then(() => {
        // 如果希望跳转到登录后能回跳回来，需要在跳转去携带参数(当前的路径地址)
        this.$router.replace({
          path: '/login',
          query: {
            backUrl: this.$route.fullPath // 完整路径包含参数
          }
        })
      })
      .catch(() => {})
    return
  }
  console.log('进行加入购物车操作')
}
```
使用 `this.$router.replace` 进行跳转，而不是 `this.$router.push`  
push会添加回退记录，而replace不会。这样用户登陆后再按返回就不会再回到登陆界面了  
修改 [views/login/index.vue](project/hm-shopping/src/views/login/index.vue) 实现回跳
```js
// 登录
async login () {
  if (!this.validFn()) {
    return
  }
  if (!/^\d{6}$/.test(this.msgCode)) {
    this.$toast('请输入正确的手机验证码')
    return
  }
  console.log('发送登录请求')
  const res = await codeLogin(this.mobile, this.msgCode)
  this.$store.commit('user/setUserInfo', res.data)
  this.$toast('登录成功')
  // 进行判断，看地址栏有无回跳地址
  // 1. 如果有   => 说明是其他页面，拦截到登录来的，需要回跳
  // 2. 如果没有 => 正常去首页
  const url = this.$route.query.backUrl || '/'
  this.$router.replace(url)
}
```


### 封装接口进行请求，请求拦截器统一携带 token
![](assets/Pasted%20image%2020240208191345.png)
[api/cart.js](project/hm-shopping/src/api/cart.js)  
[views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue)  
```js
async addCart () {
  ...
  const { data } = await addCart(this.goodsId, this.addCount, this.detail.skuList[0].goods_sku_id)
  this.cartTotal = data.cartTotal
  this.$toast('加入购物车成功')
  this.showPannel = false
},
```
**请求拦截器中，统一携带 token**
[utils/request.js](project/hm-shopping/src/utils/request.js)
```js
// 自定义配置 - 请求/响应 拦截器
// 添加请求拦截器
instance.interceptors.request.use(function (config) {
  ...
  // 只要有token，就在请求时携带，便于请求需要授权的接口
  const token = store.getters.token
  if (token) {
    config.headers['Access-Token'] = token
    config.headers.platform = 'H5'
  }
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})
```

## 十九、【购物车模块】
![](assets/Pasted%20image%2020240209082140.png)
[views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
Checkbox 复选框： https://vant-contrib.gitee.io/vant/v2/#/zh-CN/checkbox  

### 构建 vuex cart 模块，获取数据存储
新建模块 [store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
封装 API 接口 [api/cart.js](project/hm-shopping/src/api/cart.js)  
```js
// 获取购物车列表数据
export const getCartList = () => {
  return request.get('/cart/list')
}
```
封装 action 和 mutation [store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```js
mutations: {
  setCartList (state, newList) {
    state.cartList = newList
  },
},
actions: {
  async getCartAction (context) {
    // 后台返回的数据中，不包含复选框的选中状态，为了实现将来的功能
    // 需要手动维护数据，给每一项，添加一个 isChecked 状态 (标记当前商品是否选中)
    const { data } = await getCartList()
    data.list.forEach(item => {
      item.isChecked = true
    })
    context.commit('setCartList', data.list)
  }
},
```
购物车页面中 dispatch 调用 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```js
computed: {
  isLogin () {
    return this.$store.getters.token
  }
},
created () {
  if (this.isLogin) {
    this.$store.dispatch('cart/getCartAction')
  }
},
```

### 基于 数据 动态渲染 购物车列表
将数据映射到页面 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```js
import { mapState } from 'vuex'

computed: {
  ...mapState('cart', ['cartList'])
}
```
动态渲染 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```html
<!-- 购物车列表 -->
<div class="cart-list">
  <div class="cart-item" v-for="item in cartList" :key="item.goods_id">
    <van-checkbox icon-size="18" :value="item.isChecked"></van-checkbox>
    <div class="show" @click="$router.push(`/prodetail/${item.goods_id}`)">
      <img :src="item.goods.goods_image" alt="">
    </div>
    <div class="info">
      <span class="tit text-ellipsis-2">{{ item.goods.goods_name }}</span>
      <span class="bottom">
        <div class="price">¥ <span>{{ item.goods.goods_price_min }}</span></div>
        <CountBox :value="item.goods_num"></CountBox>
      </span>
    </div>
  </div>
</div>
```

### 封装 getters 实现动态统计
封装 getters：商品总数 / 选中的商品列表 / 选中的商品总数 / 选中的商品总价  
[store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```js
getters: {
  // 求所有的商品累加总数
  cartTotal (state) {
    return state.cartList.reduce((sum, item, index) => sum + item.goods_num, 0)
  },
  // 选中的商品项
  selCartList (state) {
    return state.cartList.filter(item => item.isChecked)
  },
  // Getter可以接收其他getter作为第二个参数
  // 选中的总数
  selCount (state, getters) {
    return getters.selCartList.reduce((sum, item, index) => sum + item.goods_num, 0)
  },
  // 选中的总价
  selPrice (state, getters) {
    return getters.selCartList.reduce((sum, item, index) => {
      return sum + item.goods_num * item.goods.goods_price_min
    }, 0).toFixed(2)
  }
}
```
**Getter可以接收其他getter作为第二个参数**  

页面中 mapGetters 映射使用 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
computed: {
  ...mapGetters('cart', ['cartTotal', 'selCount', 'selPrice']),
},
    
<!-- 购物车开头 -->
<div class="cart-title">
  <span class="all">共<i>{{ cartTotal || 0 }}</i>件商品</span>
  <span class="edit">
    <van-icon name="edit"  />
    编辑
  </span>
</div>


<div class="footer-fixed">
  <div  class="all-check">
    <van-checkbox  icon-size="18"></van-checkbox>
    全选
  </div>
  <div class="all-total">
    <div class="price">
      <span>合计：</span>
      <span>¥ <i class="totalPrice">{{ selPrice }}</i></span>
    </div>
    <div v-if="true" :class="{ disabled: selCount === 0 }" class="goPay">
      结算({{ selCount }})
    </div>
    <div v-else  :class="{ disabled: selCount === 0 }" class="delete">
      删除({{ selCount }})
    </div>
  </div>
</div>
```

### 全选反选功能
点击小选，修改状态  
[views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
[store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```jsx
// views/layout/cart.vue
<van-checkbox icon-size="18" :value="item.isChecked" @click="toggleCheck(item.goods_id)"></van-checkbox>
    
toggleCheck (goodsId) {
  this.$store.commit('cart/toggleCheck', goodsId)
},

// store/modules/cart.js 
mutations: {
  toggleCheck (state, goodsId) {
    const goods = state.cartList.find(item => item.goods_id === goodsId)
    goods.isChecked = !goods.isChecked
  },
}
```
全选 getters，全选按钮显示  
[store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
[views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
getters: {
  isAllChecked (state) {
    return state.cartList.every(item => item.isChecked)
  }
}


...mapGetters('cart', ['isAllChecked']),

<div class="all-check">
  <van-checkbox :value="isAllChecked" icon-size="18"></van-checkbox>
  全选
</div>
```
点击全选，重置状态  
[views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
[store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```jsx
<div @click="toggleAllCheck" class="all-check">
  <van-checkbox :value="isAllChecked" icon-size="18"></van-checkbox>
  全选
</div>

toggleAllCheck () {
  this.$store.commit('cart/toggleAllCheck', !this.isAllChecked)
},


mutations: {
  toggleAllCheck (state, flag) {
    state.cartList.forEach(item => {
      item.isChecked = flag
    })
  },
}
```

### 数字框修改数量功能
封装 api 接口 [api/cart.js](project/hm-shopping/src/api/cart.js)  
```jsx
// 更新购物车商品数量
export const changeCount = (goodsId, goodsNum, goodsSkuId) => {
  return request.post('/cart/update', {
    goodsId,
    goodsNum,
    goodsSkuId
  })
}
```
页面中注册点击事件，传递数据 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
<!-- 既希望保留原本的形参，又需要通过调用函数传参 => 箭头函数包装一层 -->
<CountBox :value="item.goods_num" @input="(value) => changeCount(value, item.goods_id, item.goods_sku_id)"></CountBox>

changeCount (value, goodsId, skuId) {
  this.$store.dispatch('cart/changeCountAction', {
    value,
    goodsId,
    skuId
  })
},
```
提供 action 发送请求 [store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```jsx
mutations: {
  changeCount (state, { goodsId, value }) {
    const obj = state.cartList.find(item => item.goods_id === goodsId)
    obj.goods_num = value
  }
},
actions: {
  async changeCountAction (context, obj) {
    const { goodsId, value, skuId } = obj
    context.commit('changeCount', {
      goodsId,
      value
    })
    await changeCount(goodsId, value, skuId)
  },
}
```

### 编辑切换状态
[views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
data 提供数据, 定义是否在编辑删除的状态
注册点击事件，修改状态
```jsx
<span class="edit" @click="isEdit = !isEdit">
  <van-icon name="edit"  />
  编辑
</span>

<div v-if="!isEdit" :class="{ disabled: selCount === 0 }" class="goPay">
    去结算（{{ selCount }}）
</div>
<div v-else :class="{ disabled: selCount === 0 }" class="delete">删除</div>


data () {
  return {
    isEdit: false
  }
},
```
监视编辑状态，动态控制复选框状态（编辑状态时购物车全不选）
```jsx
watch: {
  isEdit (value) {
    if (value) {
      this.$store.commit('cart/toggleAllCheck', false)
    } else {
      this.$store.commit('cart/toggleAllCheck', true)
    }
  }
}
```


### 删除功能
查看接口，封装 API ( 注意：此处 id 为获取回来的购物车数据的 id ) [api/cart.js](project/hm-shopping/src/api/cart.js)  
```jsx
// 删除购物车
export const delSelect = (cartIds) => {
  return request.post('/cart/clear', {
    cartIds
  })
}
```
注册删除点击事件 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
<div v-else :class="{ disabled: selCount === 0 }" @click="handleDel" class="delete">
  删除({{ selCount }})
</div>

async handleDel () {
  if (this.selCount === 0) return
  await this.$store.dispatch('cart/delSelect')
  this.isEdit = false
},
```
提供 actions [store/modules/cart.js](project/hm-shopping/src/store/modules/cart.js)  
```jsx
actions: {
  // 删除购物车数据
  async delSelect (context) {
    const selCartList = context.getters.selCartList
    const cartIds = selCartList.map(item => item.id)
    await delSelect(cartIds)
    Toast('删除成功')
    
    // 重新拉取最新的购物车数据 (重新渲染)
    context.dispatch('getCartAction')
  }
},
```

### 空购物车处理
外面包个大盒子，添加 v-if 判断 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
<div class="cart-box" v-if="isLogin && cartList.length > 0">
  <!-- 购物车开头 -->
  <div class="cart-title">
    ...
  </div>
  <!-- 购物车列表 -->
  <div class="cart-list">
    ...
  </div>
  <div class="footer-fixed">
    ...
  </div>
</div>

<div class="empty-cart" v-else>
  <img src="@/assets/empty.png" alt="">
  <div class="tips">
    您的购物车是空的, 快去逛逛吧
  </div>
  <div class="btn" @click="$router.push('/')">去逛逛</div>
</div>
```

## 二十、【订单结算台】
![](assets/Pasted%20image%2020240209153033.png)
[views/pay/index.vue](project/hm-shopping/src/views/pay/index.vue)  

### 确认订单信息 - 封装通用接口
![](assets/Pasted%20image%2020240209174518.png)
[api/order.js](project/hm-shopping/src/api/order.js)

### 购物车结算 - 购物车结算跳转传递参数
![](assets/Pasted%20image%2020240209175850.png)
跳转时，传递查询参数 [views/layout/cart.vue](project/hm-shopping/src/views/layout/cart.vue)  
```jsx
<div @click="goPay">结算({{ selCount }})</div>

goPay () {
  // 判断有没有选中商品
  if (this.selCount > 0) {
    // 有选中的 商品 才进行结算跳转
    this.$router.push({
      path: '/pay',
      query: {
        mode: 'cart',
        cartIds: this.selCartList.map(item => item.id).join(',') 
        // 'cartId,cartId,cartId'
      }
    })
  }
}
```
结算页面中接收参数, 调用接口，获取数据 [views/pay/index.vue](project/hm-shopping/src/views/pay/index.vue)  
```jsx
data () {
  return {
    order: {},
    personal: {}
  }
},
    
computed: {
  mode () {
    return this.$route.query.mode
  },
  cartIds () {
    return this.$route.query.cartIds
  }
}

async created () {
  this.getOrderList()
},

async getOrderList () {
  if (this.mode === 'cart') {
    const { data: { order, personal } } = await checkOrder(this.mode, { cartIds: this.cartIds })
    this.order = order
    this.personal = personal
  }
}
```
基于数据进行渲染 [views/pay/index.vue](project/hm-shopping/src/views/pay/index.vue)  

### 立即购买结算
![](assets/Pasted%20image%2020240209182723.png)


### 登录确认框复用 (mixins混入)
新建一个 mixin 文件 [mixins/loginConfirm.js](project/hm-shopping/src/mixins/loginConfirm.js)  
```jsx
export default {
  // 此处编写的就是 Vue组件实例的 配置项，通过一定语法，可以直接混入到组件内部
  // data methods computed 生命周期函数 ...
  // 注意点：
  // 1. 如果此处 和 组件内，提供了同名的 data 或 methods， 则组件内优先级更高
  // 2. 如果编写了生命周期函数，则mixins中的生命周期函数 和 页面的生命周期函数，
  //    会用数组管理，统一执行
  created () {
    // console.log('嘎嘎')
  },
  data () {
    return {
      title: '标题'
    }
  },
  methods: {
    sayHi () {
      // console.log('你好')
    },

    // 根据登录状态，判断是否需要显示登录确认框
    // 1. 如果未登录 => 显示确认框 返回 true
    // 2. 如果已登录 => 啥也不干   返回 false
    loginConfirm () {
      // 判断 token 是否存在
      if (!this.$store.getters.token) {
        // 弹确认框
        this.$dialog.confirm({
          title: '温馨提示',
          message: '此时需要先登录才能继续操作哦',
          confirmButtonText: '去登陆',
          cancelButtonText: '再逛逛'
        })
          .then(() => {
            this.$router.replace({
              path: '/login',
              query: {
                backUrl: this.$route.fullPath
              }
            })
          })
          .catch(() => {})
        return true
      }
      return false
    }
  }
}
```
页面中导入，混入方法 [views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue)  
```jsx
import loginConfirm from '@/mixins/loginConfirm'

export default {
  name: 'ProDetail',
  mixins: [loginConfirm],
  ...
}
```
页面中调用混入的方法 [views/prodetail/index.vue](project/hm-shopping/src/views/prodetail/index.vue)  
```jsx
async addCart () {
  if (this.loginConfirm()) {
    return
  }
  const { data } = await addCart(this.goodsId, this.addCount, this.detail.skuList[0].goods_sku_id)
  this.cartTotal = data.cartTotal
  this.$toast('加入购物车成功')
  this.showPannel = false
  console.log(this.cartTotal)
},

goBuyNow () {
  if (this.loginConfirm()) {
    return
  }
  this.$router.push({
    path: '/pay',
    query: {
      mode: 'buyNow',
      goodsId: this.goodsId,
      goodsSkuId: this.detail.skuList[0].goods_sku_id,
      goodsNum: this.addCount
    }
  })
}
```


## 二十、提交订单并支付
