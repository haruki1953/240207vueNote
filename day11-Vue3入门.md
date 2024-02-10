# day11-Vue3入门
vue3的优势
	![](assets/Pasted%20image%2020240210100638.png) 

## 一、Vue2 选项式 API vs Vue3 组合式API
![](assets/Pasted%20image%2020240210100818.png)
![](assets/Pasted%20image%2020240210101505.png)
1. 代码量变少
2. 分散式维护变成集中式维护

## 二、create-vue搭建Vue3项目
create-vue是Vue官方新的脚手架工具，底层切换到了 vite （下一代构建工具），为开发提供极速响应
	![](assets/Pasted%20image%2020240210101904.png) 

![](assets/Pasted%20image%2020240210111408.png)
> 前置条件 - 已安装16.0或更高版本的Node.js，node -v

`npm init vue@latest`  
这一指令将会安装并执行 create-vue
安装依赖：`npm install`
运行项目：`npm run dev` 或 `yarn dev`


## 三、熟悉项目目录和关键文件
![](assets/Pasted%20image%2020240210113335.png) 
[vite.config.js](project/day11-vue3-demo/vite.config.js)  
[package.json](project/day11-vue3-demo/package.json)  
[src/main.js](project/day11-vue3-demo/src/main.js)  
[src/App.vue](project/day11-vue3-demo/src/App.vue)  
[index.html](project/day11-vue3-demo/index.html)  

vetur是vue2的vscode插件，禁用。vue3的插件是volar
	![](assets/Pasted%20image%2020240210114828.png) 





