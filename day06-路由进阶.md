# day06-路由进阶

## 一、路由模块封装
问题：：所有的路由配置都在main.js中合适吗？
目标：将路由模块抽离出来。 好处**拆分模块，利于维护**
	![](assets/Pasted%20image%2020240203201457.png)

路径简写：
**脚手架环境下** @指代src目录，可以用于快速引入组件
```js
import Find from '@/views/Find'
import My from '@/views/My'
import Friend from '@/views/Friend'
```


## 二、声明式导航
### 导航高亮
实现导航高亮效果
	![](assets/Pasted%20image%2020240204082237.png) 
vue-router 提供了一个全局组件 router-link (取代 a 标签) 
① 能跳转，配置 to 属性指定路径(必须) 。本质还是 a 标签 ，to 无需 # 
② 能高亮，默认就会提供高亮类名，可以直接设置高亮样式

**通过router-link自带的两个样式进行高亮**
使用router-link跳转后，我们发现。当前点击的链接默认加了两个class的值 `router-link-exact-active`和`router-link-active`
我们可以给任意一个class属性添加高亮样式即可实现功能
```css
.footer_wrap a.router-link-active {
  background-color: purple;
}
```


### 精确匹配&模糊匹配
**声明式导航 - 两个类名**
	![](assets/Pasted%20image%2020240204083708.png) 
① router-link-active 模糊匹配 (用的多) 
	to="/my" 可以匹配 /my /my/a /my/b .... 
② router-link-exact-active 精确匹配 
	to="/my" 仅可以匹配 /my

### 自定义高亮类名
我们可以在创建路由对象时，额外配置两个配置项即可。 `linkActiveClass`和`linkExactActiveClass`
	![](assets/Pasted%20image%2020240204084522.png) 
```js
// 创建了一个路由对象
const router = new VueRouter({
  routes: [...], 
  linkActiveClass: 'active', // 配置模糊匹配的类名
  linkExactActiveClass: 'exact-active' // 配置精确匹配的类名
})
```


### 跳转传参
目标：在跳转路由时, 进行传值
1. 查询参数传参 
2. 动态路由传参 

#### 查询参数传参
![](assets/Pasted%20image%2020240204090500.png) 
① 语法格式如下 
	to="/path?参数名=值"
② 对应页面组件接收传递过来的值
	$route.query.参数名
```html
<template>
  <div class="search">
    <p>搜索关键字: {{ $route.query.key }} </p>
    <p>搜索结果: </p>
  </div>
</template>

<script>
export default {
  name: 'MyFriend',
  created () {
    // 在created中，获取路由参数
    // this.$route.query.参数名 获取
    console.log(this.$route.query.key);
  }
}
</script>
```

#### 动态路由传参
![](assets/Pasted%20image%2020240204092309.png)
① 配置动态路由 
② 配置导航链接
	to="/path/参数值" 
③ 对应页面组件接收传递过来的值
	$route.params.参数名

动态路由参数可选符
	![](assets/Pasted%20image%2020240204094229.png) 

#### 查询参数传参 VS 动态路由传参
1. 查询参数传参 (比较适合传**多个参数**)
    1. 跳转：to="/path?参数名=值&参数名2=值"
    2. 获取：$route.query.参数名

2. 动态路由传参 (**优雅简洁**，传单个参数比较方便)
    1. 配置动态路由：path: "/path/:参数名"
    2. 跳转：to="/path/参数值"
    3. 获取：$route.params.参数名
    注意：动态路由也可以传多个参数，但一般只传一个


## 三、路由重定向

### Vue路由 - 重定向
![](assets/Pasted%20image%2020240204094736.png)
语法： { path: 匹配路径, redirect: 重定向到的路径 },

### Vue路由 - 404
![](assets/Pasted%20image%2020240204094907.png)

### Vue路由 - 模式设置
![](assets/Pasted%20image%2020240204094959.png)
- hash路由(默认) 例如: http://localhost:8080/#/home 
- history路由(常用) 例如: http://localhost:8080/home (后台需配置访问规则)


## 四、编程式导航
### 基本跳转
编程式导航：用JS代码来进行跳转，点击按钮跳转
两种语法：
- path 路径跳转 （简易方便）
- name 命名路由跳转 (适合 path 路径长的场景)

**path路径跳转语法**
```js
//简单写法
this.$router.push('路由路径')

//完整写法
this.$router.push({
  path: '路由路径'
})
```

**name命名路由跳转**
- 路由规则，必须配置name配置项
```js
{ name: '路由名', path: '/path/xxx', component: XXX },
```
- 通过name来进行跳转
```js
this.$router.push({
  name: '路由名'
})
```


### 编程式导航 - 路由传参
**① path 路径跳转传参 (query传参)**
```js
//简单写法
this.$router.push('/路径?参数名1=参数值1&参数2=参数值2')
//完整写法
this.$router.push({
  path: '/路径',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
```

**① path 路径跳转传参 (动态路由传参)**
```js
//简单写法
this.$router.push('/路径/参数值')
//完整写法
this.$router.push({
  path: '/路径/参数值'
})
```
**注意：** path不能配合params使用

**② name 命名路由跳转传参 (query传参)**
```js
this.$router.push({
  name: '路由名字',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
```

**② name 命名路由跳转传参 (动态路由传参)**
```js
this.$router.push({
  name: '路由名字',
  params: {
    参数名: '参数值',
  }
})
```


## 综合案例：面经基础版
[综合案例：面经基础版.zip](assets/综合案例：面经基础版.zip)
案例效果：
	![](assets/Pasted%20image%2020240204114123.png) 

### 一级路由配置
对router/index.js文件 进行一级路由配置
```js
import Layout from '@/views/Layout.vue'
import ArticleDetail from '@/views/ArticleDetail.vue'


const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Layout
    },
    {
      path: '/detail',
      component: ArticleDetail
    }
  ]
})
```

### 二级路由配置
二级路由也叫嵌套路由，当然也可以嵌套三级、四级...
语法：在一级路由下，配置children属性即可
```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Layout,
      redirect: '/article',
      children:[
        {
          path:'/article',
          component:Article
        },
        {
          path:'/collect',
          component:Collect
        },
        {
          path:'/like',
          component:Like
        },
        {
          path:'/user',
          component:User
        }
      ]
    },
  ]
})
```
**注意：** 配置了嵌套路由，一定配置对应的路由出口，否则不会渲染出对应的组件
```html
<template>
  <div class="h5-wrapper">
    <div class="content">
      <router-view></router-view>
    </div>
  ....
  </div>
</template>
```

### 二级导航高亮
- 将a标签替换成 `<router-link></router-link>` 组件，配置to属性，不用加 #
- 结合高亮类名实现高亮效果 (推荐模糊匹配：router-link-active)

Layout.vue
```html
    <nav class="tabbar">
      <router-link to="/article">面经</router-link>
      <router-link to="/collect">收藏</router-link>
      <router-link to="/like">喜欢</router-link>
      <router-link to="/user">我的</router-link>
    </nav>

<style>
   a.router-link-active {
      color: orange;
    }
</style>
```

### 首页请求渲染 & 跳转传参
Article.vue
```html
<template>
  <div class="article-page">
    <div
      v-for="item in articles"
      :key="item.id"
      @click="$router.push(`/detail/${item.id}`)"
      class="article-item">
      <div class="head">
        <img :src="item.creatorAvatar" alt="" />
        <div class="con">
          <p class="title">{{ item.stem }}</p>
          <p class="other">{{ item.creatorName }} | {{ item.createdAt }}</p>
        </div>
      </div>
      <div class="body">
        {{ item.content }}
      </div>
      <div class="foot">点赞 {{ item.likeCount }} | 浏览 {{ item.views }}</div>
    </div>
  </div>
</template>

<script>
// 首页请求渲染
// 1. 安装 axios   yarn add axios
// 2. 看接口文档，确认请求方式，请求地址，请求参数
//    请求地址: https://mock.boxuegu.com/mock/3083/articles
//    请求方式: get
// 3. created中发请求，获取数据，存起来
// 4. 页面动态渲染

// 跳转详情页传参 → 渲染
// 1. 查询参数传参 (更适合多个参数)
//    ?参数=参数值    =>  this.$route.query.参数名
// 2. 动态路由传参 (单个参数更优雅方便)
//    改造路由 => /路径/参数  =>  this.$route.params.参数名
//    细节优化：
//    (1) 访问 / 重定向到 /article   (redirect)
//    (2) 返回上一页 $router.back()

import axios from 'axios'
export default {
  name: 'ArticlePage',
  data () {
    return {
      articles: []
    }
  },
  async created () {
    const res = await axios.get('https://mock.boxuegu.com/mock/3083/articles')
    this.articles = res.data.result.rows
    // console.log(this.articles);
  }
}
</script>
```

### 详情页渲染
ArticleDetail.vue
```html
<template>
  <div class="article-detail-page" v-if="article.id">
    <nav class="nav"> <span @click="$router.back()" class="back">&lt;</span> 面经详情</nav>
    <header class="header">
      <h1>{{ article.stem }}</h1>
      <p>{{ article.createdAt }} | {{ article.views }} 浏览量 | {{ article.likeCount }} 点赞数</p>
      <p>
        <img :src="article.creatorAvatar" alt=""> 
        <span>{{ article.creatorName }}</span> 
      </p>
    </header>
    <main class="body">  
      {{ article.content }}
    </main>
  </div>
</template>

<script>
// 请求地址: https://mock.boxuegu.com/mock/3083/articles/:id
// 请求方式: get
import axios from 'axios'
export default {
  name: 'ArticleDetailPage',
  data () {
    return {
      article: {}
    }
  },
  async created () {
    const id = this.$route.params.id
    const { data } = await axios.get(`https://mock.boxuegu.com/mock/3083/articles/${id}`)
    this.article = data.result
    // console.log(this.article);
  }
};
</script>
```

`@click="$router.back()"` 回到上一页

### 组件缓存 keep-alive
问题：
	![](assets/Pasted%20image%2020240204143200.png) 
**keep-alive**
	![](assets/Pasted%20image%2020240204143512.png) 

#### keep-alive的三个属性
① include ： 组件名数组，只有匹配的组件会被缓存 
② exclude ： 组件名数组，任何匹配的组件都不会被缓存 
③ max ： 最多可以缓存多少组件实例
App.vue
```html
<template>
  <div class="h5-wrapper">
    <keep-alive :include="['LayoutPage']">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>
```

#### 额外的两个生命周期钩子
**keep-alive的使用会触发两个生命周期函数**，被缓存的组件会多两个生命周期钩子：
**activated** 当组件被激活（使用）的时候触发 → 进入这个页面的时候触发
**deactivated** 当组件不被使用的时候触发 → 离开这个页面的时候触发

组件**缓存后**就**不会执行**组件的**created, mounted, destroyed** 等钩子了
所以其提供了**actived 和deactived**钩子，帮我们实现业务需求。

### 总结
1、项目案例实现的基本步骤分哪两大步? 
	① 配路由 ② 实现页面功能 
	
2、嵌套路由的关键配置项是什么？ 
	children 
	
3、路由传参两种方式？ 
	① 查询参数传参，`$route.query.参数名` (适合多个参数) 
	② 动态路由传参，`$route.params.参数名` (更简洁直观) 
	
4、缓存组件可以用哪个内置组件？ 
	keep-alive 
	三个属性： include exclude max 
	两个钩子： activated deactivated


## 五、VueCli 自定义创建项目
![](assets/Pasted%20image%2020240204175831.png)

1.安装脚手架 (已安装)
```
npm i @vue/cli -g
```

2.创建项目
```
vue create hm-exp-mobile
```

+ 选项
```js
Vue CLI v5.0.8
? Please pick a preset:
  Default ([Vue 3] babel, eslint)
  Default ([Vue 2] babel, eslint)
> Manually select features     选自定义
```

- 手动选择功能
![](assets/Pasted%20image%2020240204161203.png)

+ 选择vue的版本
```jsx
  3.x
> 2.x
```

+ 是否使用history模式
![](assets/Pasted%20image%2020240204161244.png)

+ 选择css预处理
![](assets/Pasted%20image%2020240204161303.png)

+ 选择eslint的风格 （eslint 代码规范的检验工具，检验代码是否符合规范）
+ 比如：const age = 18;   =>  报错！多加了分号！后面有工具，一保存，全部格式化成最规范的样子
![](assets/Pasted%20image%2020240204161314.png)

+ 选择校验的时机 （直接回车）
![](assets/Pasted%20image%2020240204161328.png)

+ 选择配置文件的生成方式 （直接回车）
![](assets/Pasted%20image%2020240204161400.png)

- 是否保存预设，下次直接使用？  =>   不保存，输入 N
![](assets/Pasted%20image%2020240204161410.png)

+ 等待安装，项目初始化完成
![](assets/Pasted%20image%2020240204161419.png)

+ 启动项目
```sh
# yarn serve
npm run serve
```


## 六、ESlint 代码规范
**正规的团队需要统一的编码风格**
	![](assets/Pasted%20image%2020240204180656.png) 
ESLint:是一个代码检查工具，用来检查你的代码是否符合指定的规则(你和你的团队可以自行约定一套规则)。在创建项目时，我们使用的是 [JavaScript Standard Style](https://standardjs.com/readme-zhcn.html) 代码风格的规则。
下面是这份规则中的一小部分：
- _字符串使用单引号_ – 需要转义的地方除外
- _无分号_ – [这](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)[没什么不好。](http://inimino.org/~inimino/blog/javascript_semicolons)[不骗你！](https://www.youtube.com/watch?v=gsfbh17Ax9I)
- _关键字后加空格_ `if (condition) { ... }`
- _函数名后加空格_ `function name (arg) { ... }`
- 坚持使用全等 `===` 摒弃 `==`，但在需要检查 `null || undefined` 时可以使用 `obj == null`
- ......

**代码规范错误** [ESLint 规则表](https://zh-hans.eslint.org/docs/latest/rules/)
	![](assets/Pasted%20image%2020240204180441.png) 
	![](assets/Pasted%20image%2020240204181419.png) 

### 通过eslint插件来实现自动修正
![](assets/Pasted%20image%2020240204181752.png)
1. eslint会自动高亮错误显示
2. 通过配置，eslint会自动帮助我们修复错误

**配置**
```js
// 当保存的时候，eslint自动帮我们修复错误
"editor.codeActionsOnSave": {
    "source.fixAll": true
},
// 保存代码，不自动格式化
"editor.formatOnSave": false
```
- 注意：eslint的配置文件必须在根目录下，这个插件才能才能生效。打开项目必须以根目录打开，一次打开一个项目
- 注意：使用了eslint校验之后，把vscode带的那些格式化工具全禁用了 Beatify

**settings.json 参考**
```js
{
    "window.zoomLevel": 2,
    "workbench.iconTheme": "vscode-icons",
    "editor.tabSize": 2,
    "emmet.triggerExpansionOnTab": true,
    // 当保存的时候，eslint自动帮我们修复错误
    "editor.codeActionsOnSave": {
        "source.fixAll": true
    },
    // 保存代码，不自动格式化
    "editor.formatOnSave": false
}
```