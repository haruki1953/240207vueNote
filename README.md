- 前端最新Vue2+Vue3基础入门到实战项目全套教程 
- 视频： https://www.bilibili.com/video/BV1HV4y1a7n4/ 
- 资料： https://pan.baidu.com/s/1G3VGhQ-m43w1IOYs6YRxgw&pwd=6633 


[【散装知识】](笔记/散装知识.md#)  
[day01-快速上手+插值表达式+指令上](day01-快速上手+插值表达式+指令上.md)  
[day02-指令下+计算属性+侦听器](day02-指令下+计算属性+侦听器.md)  
[day03-生命周期+工程化开发(组件入门)](day03-生命周期+工程化开发(组件入门).md)  
[day04-组件通信&进阶用法](day04-组件通信&进阶用法.md)  
[day05-自定义指令&插槽&路由入门](day05-自定义指令&插槽&路由入门.md)  
[day06-路由进阶](day06-路由进阶.md)  
[day07-Vuex的基本使用](day07-Vuex的基本使用.md)  
[day08-day10-智慧商城项目](day08-day10-智慧商城项目.md)  
[day11-Vue3入门](day11-Vue3入门.md)  
[day12-day13-大事件管理系统](day12-day13-大事件管理系统.md)  


### 240128今日vue学习
指令修饰符、计算属性、侦听器  
[【散装知识】reduce方法：对数组中的元素进行某种累积或归约](笔记/散装知识.md#reduce方法：对数组中的元素进行某种累积或归约)  
[【散装知识】async异步函数](笔记/散装知识.md#async异步函数)  
[【散装知识】箭头函数没有自己的this](笔记/散装知识.md#箭头函数没有自己的this)  


### 240130今日vue学习
vue生命周期、生命周期函数  
[【散装知识】map创建一个新数组](笔记/散装知识.md#map创建一个新数组)  


### 240131今日vue学习
脚手架Vue CLI、组件化开发入门  


### 240201今日vue学习
今天主要学了vue的组件通信  


### 240202今日vue学习
一、v-model原理，其实就是value和@input事件的简写  
二、.sync修饰符实现 子组件 与 父组件数据 的 双向绑定  
三、利用 ref 和 $refs，在组件内获取 dom 元素 或 组件实例  
四、Vue 是异步更新DOM  
    this.$nextTick(函数体)：等 DOM更新后，才会触发执行此方法里的函数体  
    **注意：**$nextTick 内的函数体 一定是箭头函数，这样才能让函数内部的this指向Vue实例  


### 240203今日vue学习
一、自定义指令，通常封装一些dom操作，在标签被创建或更新时执行  
二、插槽，让组件内部的一些结构支持自定义   
三、路由入门，实现单页面应用程序，发现autobangumi也是单页面应用程序  

### 240204今日vue学习
一、路由进阶：导航、重定向、二级路由  
二、组件缓存，缓存首页页面以保持浏览进度  
三、ESlint 代码规范  


### 240205今日vue学习
今天只简单看了看vuex插件，可以实现复杂的组件通信  
[【散装知识】字符串前加正号可以转数字](笔记/散装知识.md#字符串前加正号可以转数字)  


### 240207今日vue学习
一、vant 组件库，有很多很方便的组件，感觉安心了很多  
二、axios 封装，通过响应拦截器，统一处理接口的错误提示  
三、vuex + storage 持久化存储登录权证信息  
[【散装知识】vscode同时编辑多行](笔记/散装知识.md#vscode同时编辑多行)  
[【散装知识】TortoiseGit修改上次提交，以及推送冲突问题](笔记/散装知识.md#TortoiseGit修改上次提交，以及推送冲突问题)  


### 240208今日vue学习
一、唤起弹层 ：ActionSheet 动作面板  
二、判断 token 添加登录提示 ：Dialog 弹出框  
三、页面回跳 ：$router.replace，push会添加回退记录，而replace不会  
四、请求拦截器统一携带 token，便于请求需要授权的接口  


### 240209今日vue学习
- **mixins混入** ： 登录确认框复用
- **vuex 跨模块调用 mutation** ：{ root: true }
- **项目打包** ：yarn build
- **配置publicPath** ：设置静态资源链接为相对路径
- **路由懒加载** ：分割组件的js文件，提高首页加载速度

比较清晰的了解了vue构建的前端项目。以前想改善alist的加载速度，但当时完全觉得像是一个黑箱（而且还是用docker部署的），现在有点头绪了，可以把静态资源放在国内的服务器，并把首页里的链接改一改应该就可以了


### 240210今日vue学习
vue3入门


### 240211今日vue学习
一、Pinia 是 Vue 的最新 状态管理工具 ，是 Vuex 的 替代品  
二、ESLint 配置代码风格，以及husky 代码检查工作流  
三、了解vue3组合式api中的路由  


### 240212今日vue学习
1. 使用 Element Plus 组件库
2. Pinia 仓库统一管理：由 stores/index.js 统一导出
3. 请求/响应拦截器中，访问pinia仓库获取token，ElMessage提示错误
4. 登录注册页面 el-form 表单校验以及提交

[【散装知识】export与export_default](笔记/散装知识.md#export与export_default)  


### 240213今日vue学习
1. 大事件案例首页布局，el-menu 菜单组件
2. vue3的全局前置守卫、router.beforeEach
3. 文章分类页面，vue3自制组件封装
4. el-table 表格组件，el-dialog 弹层组件


### 240214今日vue学习
大事件项目文章管理页面搭建
1. 选择框组件封装，Vue3中v-model是:modelValue 和 @update:modelValue 的简写
2. 分页组件 el-pagination
3. 抽屉 el-drawer。封装复用，可以通过暴露一个方法来接收参数
4. 上传文件/图片 el-upload，URL.createObjectURL(...) 创建本地预览的地址
5. 富文本编辑器 vue-quill


### 240215今日vue学习
Ai辅助开发，个人中心项目实战 - 基本资料、更换头像、重置密码


