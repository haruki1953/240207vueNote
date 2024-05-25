Web API 是浏览器提供的一套操作浏览器功能和页面元素的 API ( BOM 和 DOM )。 
现阶段我们主要针对于浏览器讲解常用的 API , 主要针对浏览器做交互效果。 
比如我们想要浏览器弹出一个警示框， 直接使用 alert(‘弹出’) 
MDN 详细 API : https://developer.mozilla.org/zh-CN/docs/Web/API 

# DOM
文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标记语言（HTML 或者XML）的标准编程接口。 
W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。

## DOM 树
![](assets/Pasted%20image%2020240525163052.png)

## 获取元素
```js
// 根据 ID 获取
document.getElementById('id');

// 根据标签名获取
document.getElementsByTagName('标签名');
// 还可以获取某个元素(父元素)内部所有指定标签名的子元素.
element.getElementsByTagName('标签名');
// 1. 因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历。 2. 得到元素对象是动态的 3. 如果获取不到元素,则返回为空的伪数组(因为获取不到对象)

document.getElementsByClassName('类名');// 根据类名返回元素对象集合

document.querySelector('选择器'); // 根据指定选择器返回第一个元素对象
document.querySelectorAll('选择器'); // 根据指定选择器返回
// querySelector 和 querySelectorAll里面的选择器需要加符号,比如:document.querySelector('#nav');

doucumnet.body // 返回body元素对象
document.documentElement // 返回html元素对象
```
使用 console.dir() 可以打获取的元素对象

## 事件基础
```js
var btn = document.getElementById('btn');
btn.onclick = function() {
 alert('你好吗');
};
```
![](assets/Pasted%20image%2020240525163830.png)

## 操作元素
操作元素是 DOM 核心内容
![](assets/Pasted%20image%2020240525164644.png)

### 改变元素内容
```js
// 改变元素内容
element.innerText // 从起始位置到终止位置的内容, 但它去除 html 标签， 同时空格和换行也会去掉
element.innerHTML // 起始位置到终止位置的全部内容，包括 html 标签，同时保留空格和换行
```
### 常用元素的属性操作
1. innerText、innerHTML 改变元素内容 
2. src、href 
3. id、alt、title

### 表单元素属性
利用 DOM 可以操作如下表单元素的属性：
type、value、checked、selected、disabled
```js
// 1. 获取元素
var btn = document.querySelector('button');
var input = document.querySelector('input');
// 2. 注册事件 处理程序
btn.onclick = function() {
	// input.innerHTML = '点击了';  这个是 普通盒子 比如 div 标签里面的内容
	// 表单里面的值 文字内容是通过 value 来修改的
	input.value = '被点击了';
	// 如果想要某个表单被禁用 不能再点击 disabled  我们想要这个按钮 button禁用
	// btn.disabled = true;
	this.disabled = true;
	// this 指向的是事件函数的调用者 btn
}
```

### 样式属性操作
1. element.style 行内样式操作 
2. element.className 类名样式操作
className 会直接更改元素的类名，会覆盖原先的类名。

### 自定义属性的操作
- element.属性 获取内置属性值（元素本身自带的属性） 
- element.getAttribute(‘属性’); 主要获得自定义的属性 

- element.属性 = ‘值’ 设置内置属性值。 
- element.setAttribute('属性', '值'); 设置自定义的属性

- element.removeAttribute('属性'); 移除属性

### H5自定义属性
自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中
H5规定自定义属性data-开头做为属性名并且赋值。
#### 获取H5自定义属性
1. 兼容性获取 `element.getAttribute('data-index');` 
2. H5新增 `element.dataset.index` 或者 `element.dataset['index']` ie 11才开始支持

## 节点操作
获取元素通常使用两种方式：
1. 利用 DOM 提供的方法获取元素
2. 利用节点层级关系获取元素

```js
// 父级节点,最近的一个父节点,没有父节点则返回 null
node.parentNode
// 子节点,返回包含指定节点的子节点的集合，该集合为即时更新的集合。
parentNode.childNodes （标准）
// 注意：返回值里面包含了所有的子节点，包括元素节点，文本节点等。 如果只想要获得里面的元素节点，则需要专门处理。 所以我们一般不提倡使用childNodes
parentNode.children （非标准）
// parentNode.children 是一个只读属性，返回所有的子元素节点。它只返回子元素节点，其余节点不返 回 （这个是我们重点掌握的）。 虽然children 是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用
parentNode.firstChild // 返回第一个子节点，找不到则返回null。是包含所有的节点。
parentNode.lastChild // 返回最后一个子节点
parentNode.firstElementChild // 返回第一个子元素节点，
parentNode.lastElementChild // 返回最后一个子元素节点
```

### 创建节点
```js
document.createElement('tagName')
```
### 添加节点
```js
node.appendChild(child) // 将一个节点添加到指定父节点的子节点列表末尾。类似于 CSS 里面的 after 伪元素
node.insertBefore(child, 指定元素) // 将一个节点添加到父节点的指定子节点前面
```
### 删除节点
node.removeChild(child) 方法从 DOM 中删除一个子节点，返回删除的节

### 复制节点(克隆节点)
node.cloneNode() 方法返回调用该方法的节点的一个副本。
注意： 
1. 如果括号参数为空或者为 false ，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点。 
2. 如果括号参数为 true ，则是深度拷贝，会复制节点本身以及里面所有的子节点。

### 三种动态创建元素区别
![](assets/Pasted%20image%2020240525170622.png)

# 事件高级
![](资料/JavaScript%20APIs/ppt/02-事件高级.pdf)

# BOM
![](资料/JavaScript%20APIs/ppt/03-BOM%20浏览器对象模型.pdf)

setTimeout() 定时器
setInterval() 定时器

## location 对象
![](assets/Pasted%20image%2020240525175831.png)

location 对象的属性
![](assets/Pasted%20image%2020240525175913.png)

location 对象的方法
![](assets/Pasted%20image%2020240525180009.png)


## navigator 对象
navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客 户机发送服务器的 user-agent 头部的值。 
下面前端代码可以判断用户那个终端打开页面，实现跳转
```js
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|
Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS
|Symbian|Windows Phone)/i))) {
	window.location.href = ""; //手机
} else {
	window.location.href = ""; //电脑
}
```

## history 对象
window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中） 访问过的 URL。
![](assets/Pasted%20image%2020240525182740.png)


# PC 端网页特效
![](资料/JavaScript%20APIs/ppt/04-PC端网页特效.pdf)


# 移动端网页特效
![](资料/JavaScript%20APIs/ppt/05-移动端网页特效.pdf)


# 本地存储
![](资料/JavaScript%20APIs/ppt/06-本地存储.pdf)


