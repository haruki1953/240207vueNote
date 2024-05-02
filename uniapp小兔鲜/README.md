资料 https://pan.baidu.com/s/1uhQQvqs9ESRvhEvgzgSUWQ&pwd=9988

视频 https://www.bilibili.com/video/BV1Bp4y1379L/

接口文档 https://www.apifox.cn/apidoc/shared-0e6ee326-d646-41bd-9214-29dbf47648fa/

### 240501
浏览uniapp小兔鲜，先掌握其[01-项目起步](uniapp小程序-配套资料/02-笔记/rabbit-shop/01-项目起步.md)中的TS项目经验
day01 [uni-app笔记](uniapp小程序-配套资料/02-笔记/uni-app/index.md)
- HBuilderX 创建 uni-app 项目
- pages.json 和 tabBar 案例
- 案例：滑动轮播图、点击大图预览
	- uni-app前端开发框架，对微信小程序常用api进行了跨段兼容封装
- 命令行创建 uni-app 项目（vue3 + ts）
	- 大概清楚这个的运行逻辑了，其实基本上还是vue项目，而uni-app（算是插件吧）又将其编译为了微信小程序的代码（保存在 dist/dev/ 目录下），以此实现在微信开发者工具中的预览。
	  所以其中的ts项目经验完全可以借鉴
- 用 VS Code 开发 uni-app 项目
	- 看到其中配置了模板中组件属性的类型检查与提示，`Ctrl+I` 自动提示，试了一下自己的移动端前端ts项目，也同样是有类型检查与自动提示 `Ctrl+I`（现在才知道🤡）

### 240502
[01-项目起步](uniapp小程序-配套资料/02-笔记/rabbit-shop/01-项目起步.md)
- 拉取小兔鲜儿项目模板代码
- 基础架构 – 引入 uni-ui 组件库【构建界面】
	- 自动导入、配置ts类型（和自己弄的还是有区别）
	  需要注意的是，它是在tsconfig.json中的compilerOptions.types中添加类型声明，而不是include（或许都可以，既然当前没问题就走一步看一步吧）
- 基础架构 – 小程序端 Pinia 持久化【状态管理】
	- 小程序端的持久化和网页端是有区别的，只设置 persist: true 是不行的，可以自行配置保存方法
	  [【01-项目起步】小程序端 Pinia 持久化](uniapp小程序-配套资料/02-笔记/rabbit-shop/01-项目起步.md#多端兼容)
- 基础架构 – uni.request 请求封装
	- 不是axios，是uni专用的请求工具，而且是通过类似钩子函数的方式配置的基地址、token等数据。响应拦截器、异步之类的也得自己实现（好麻烦）

又看了看项目，已经基本理解了ts项目中类型声明的套路了，总结在这里了 [在 Vue3 中使用 TypeScript](../TypeScript/在%20Vue3%20中使用%20TypeScript.md)
