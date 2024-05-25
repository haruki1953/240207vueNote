# 流式布局
## 视口
视口（viewport）就是浏览器显示页面内容的屏幕区域。 视口可以分为布局视口、视觉视口和理想视口
![](assets/Pasted%20image%2020240524143916.png)
![](assets/Pasted%20image%2020240524143927.png)
![](assets/Pasted%20image%2020240524144135.png)
### meta视口标签
```html
<meta name="viewport" content="width=device-width, user-scalable=no,
initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

| 属性 | 解释说明 |
| :--: | :--: |
| width | 宽度设置的是viewport宽度，可以设置device-width特殊值 |
| initial-scale | 初始缩放比，大于0的数字 |
| maximum-scale | 最大缩放比，大于0的数字 |
| minimum-scale | 最小缩放比，大于0的数字 |
| user-scalable | 用户是否可以缩放，yes或no（1或0） |

**标准的viewport设置**
- 视口宽度和设备保持一致
- 视口的默认缩放比例1.0
- 不允许用户自行缩放
- 最大允许的缩放比例1.0
- 最小允许的缩放比例1.0

### 二倍图
### 物理像素&物理像素比
PC端页面，1个px 等于1个物理像素，移动端1px 不是一定等于1个物理像素。
一个px的能显示的物理像素点的个数，称为物理像素比或屏幕像素比

## 移动端技术解决方案
### CSS初始化 normalize.css
移动端 CSS 初始化推荐使用 [normalize.css](资料/02-移动端流式布局/移动端流式布局资料/H5/css/normalize.css)

### CSS3 盒子模型 box-sizing
```css
/*CSS3盒子模型*/
box-sizing: border-box;
/*传统盒子模型*/
box-sizing: content-box;
```
尽量使用CSS3 盒子模型

### 特殊样式
```css
/*CSS3盒子模型*/
box-sizing: border-box;
-webkit-box-sizing: border-box;

/*点击高亮我们需要清除清除 设置为transparent 完成透明*/
-webkit-tap-highlight-color: transparent;

/*在移动端浏览器默认的外观在iOS上加上这个属性才能给按钮和输入框自定义样式*/
-webkit-appearance: none;

/*禁用长按页面时的弹出菜单*/
img,a { -webkit-touch-callout: none; }
```

## 流式布局（百分比布局）
![](assets/Pasted%20image%2020240524154718.png)
```css
section {
	width: 100%;
	max-width: 980px;
	min-width: 320px;
	margin: 0 auto;
}

section div {
	float: left;
	width: 50%;
	height: 400px;
}
```


# flex布局
## 布局原理
flex 是 flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性，任何一个容器都可以指定为 flex 布局。
- 当我们为父盒子设为 flex 布局以后，子元素的 float、clear 和 vertical-align 属性将失效。 
- 伸缩布局 = 弹性布局 = 伸缩盒布局 = 弹性盒布局 =flex布局

![](assets/Pasted%20image%2020240524155239.png)

## 常见父项属性
- flex-direction：设置主轴的方向
- justify-content：设置主轴上的子元素排列方式
- flex-wrap：设置子元素是否换行
- align-content：设置侧轴上的子元素的排列方式（多行）
- align-items：设置侧轴上的子元素排列方式（单行）
- flex-flow：复合属性，相当于同时设置了 flex-direction 和 flex-wrap

### flex-direction 设置主轴的方向
#### 主轴与侧轴
![](assets/Pasted%20image%2020240524155610.png)
#### 属性值
flex-direction 属性决定主轴的方向（即项目的排列方向） 
注意： 主轴和侧轴是会变化的，就看 flex-direction 设置谁为主轴，剩下的就是侧轴。而我们的子元素是跟着主轴来排列的

| 属性值 | 说明 |
| --- | --- |
| `row` | 默认值。主轴为水平方向，起点在左端。容器内的项目从左到右排列，形成一行。 |
| `row-reverse` | 主轴为水平方向，但是起点在右端。容器内的项目从右到左排列，形成一行。 |
| `column` | 主轴为垂直方向，起点在上沿。容器内的项目从上到下排列，形成一列。 |
| `column-reverse` | 主轴为垂直方向，但是起点在下沿。容器内的项目从下到上排列，形成一列。 |

### justify-content 设置主轴上的子元素排列方式
justify-content 属性定义了项目在主轴上的对齐方式 注意： 使用这个属性之前一定要确定好主轴是哪个

| 属性值 | 说明 |
| --- | --- |
| `flex-start` | 默认值。项目在主轴上从起点开始排列。如果主轴是x轴（水平方向），则项目从左到右排列。 |
| `flex-end` | 项目在主轴上从终点开始排列。如果主轴是x轴（水平方向），则项目从右到左排列。 |
| `center` | 项目在主轴上居中对齐。如果主轴是x轴（水平方向），则项目水平居中。 |
| `space-around` | 项目在主轴上均匀分布，每个项目两侧的间隔相等。第一个项目与容器的起点之间的间隔和最后一个项目与容器的终点之间的间隔是项目之间间隔的一半。 |
| `space-between` | 项目在主轴上均匀分布，但第一个项目与容器的起点紧贴，最后一个项目与容器的终点紧贴。只有项目之间的间隔是相等的。 |
| `space-evenly` | 项目在主轴上均匀分布，所有子元素之间的间距相等，且子元素与容器边框的间距也相等。 |

### flex-wrap 设置子元素是否换行
默认情况下，项目都排在一条线（又称”轴线”）上。flex-wrap属性定义，flex布局中默认是不换行的。
- nowrap 默认值，不换行 
- wrap 换行

### align-items 设置侧轴上的子元素排列方式（单行）
该属性是控制子项在侧轴（默认是y轴）上的排列方式 在子项为单项（单行）的时候使用
- flex-start 从上到下 
- flex-end 从下到上 
- center 挤在一起居中（垂直居中） 
- stretch 拉伸 （默认值 ）

### align-content 设置侧轴上的子元素的排列方式（多行）
设置子项在侧轴上的排列方式 并且只能用于子项出现 换行 的情况（多行），在单行下是没有效果的。
- flex-start 默认值在侧轴的头部开始排列 
- flex-end 在侧轴的尾部开始排列 
- center 在侧轴中间显示 
- space-around 子项在侧轴平分剩余空间 
- space-between 子项在侧轴先分布在两头，再平分剩余空间 
- stretch 设置子项元素高度平分父元素高度

### align-content 和 align-items 区别
![](assets/Pasted%20image%2020240525125540.png)

### flex-flow
flex-flow 属性是 flex-direction 和 flex-wrap 属性的复合属性 
`flex-flow:row wrap;`

## 子项常见属性
### flex 属性
flex 属性定义子项目分配剩余空间，用flex来表示占多少份数。
```css
.item { 
	flex: number; /* default 0 */ 
}
```

### align-self 控制子项自己在侧轴上的排列方式
align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。 默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。
```css
span:nth-child(2) {
	/* 设置自己在侧轴上的排列方式 */ 
	align-self: flex-end; 
}
```

### order 属性定义项目的排列顺序
数值越小，排列越靠前，默认为0。 注意：和 z-index 不一样。
```css
.item { 
	order: number; 
}
```

[案例：携程网移动端首页](资料/03-移动web开发-flex布局/案例/H5/index.html)


# rem适配布局
## rem 单位
rem (root em)是一个相对单位，类似于em，em是父元素字体大小。 
不同的是rem的基准是相对于html元素的字体大小。 
比如，根元素（html）设置font-size=12px; 非根元素设置width:2rem; 则换成px表示就是24px。 
rem的优势：父元素文字大小可能不一致， 但是整个页面只有一个html，可以很好来控制整个页面的元素大小。
```css
/* 根html 为 12px */
html {
 font-size: 12px;
}
/* 此时 div 的字体大小就是 24px */
div {
 font-size: 2rem;
}
```

## 媒体查询
媒体查询（Media Query）是CSS3新语法。 
- 使用 @media 查询，可以针对不同的媒体类型定义不同的样式 
- @media 可以针对不同的屏幕尺寸设置不同的样式 
- 当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面 
- 目前针对很多苹果手机、Android手机，平板等设备都用得到多媒体查询

```css
@media mediatype and|not|only (media feature) {
 CSS-Code;
}
```
- mediatype 媒体类型
- 关键字 and not only
- media feature 媒体特性 必须有小括号包含

### mediatype 查询类型
将不同的终端设备划分成不同的类型，称为媒体类型
- all 用于所有设备 
- print 用于打印机和打印预览 
- scree 用于电脑屏幕，平板电脑，智能手机等

### 关键字
关键字将媒体类型或多个媒体特性连接到一起做为媒体查询的条件。
- and：可以将多个媒体特性连接到一起，相当于“且”的意思。 
- not：排除某个媒体类型，相当于“非”的意思，可以省略。 
- only：指定某个特定的媒体类型，可以省略。

### 媒体特性
每种媒体类型都具体各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格。我们暂且了解三个。注意他们要加小括号包含
- width 定义输出设备中页面可见区域的宽度 
- min-width 定义输出设备中页面最小可见区域宽度 
- max-width 定义输出设备中页面最大可见区域宽度

```css
/* 这句话的意思就是： 在我们屏幕上 并且 最大的宽度是 800像素 设置我们想要的样式 */
/* max-width 小于等于800 */
/* 媒体查询可以根据不同的屏幕尺寸在改变不同的样式 */

@media screen and (max-width: 800px) {
	body {
		background-color: pink;
	}
}

@media screen and (max-width: 500px) {
	body {
		background-color: purple;
	}
}
```
注意顺序，css层叠性。同时满足小于等于800和500时，因为`max-width: 500px`在下所以他会生效

### 案例：根据页面宽度改变背景变色
[案例：根据页面宽度改变背景变色](资料/04-移动web开发-rem布局/案例/03-媒体查询案例修改背景颜色.html)
为了防止混乱，媒体查询我们要按照从小到大或者从大到小的顺序来写,但是我们最喜欢的还是从小到大来写，这样代码更简洁
![](assets/Pasted%20image%2020240525150150.png)

### 媒体查询+rem实现元素动态大小变
rem单位是跟着html来走的，有了rem页面元素可以设置不同大小尺寸 
媒体查询可以根据不同设备宽度来修改样式 
媒体查询+rem 就可以实现不同设备宽度，实现页面元素大小的动态变化
[媒体查询+rem实现元素动态变化.html](资料/04-移动web开发-rem布局/案例/04-媒体查询+rem实现元素动态变化.html)

### 引入资源（理解）
当样式比较繁多的时候，我们可以针对不同的媒体使用不同 stylesheets（样式表）。 
原理，就是直接在link中判断设备的尺寸，然后引用不同的css文件。
```html
语法规范
<link rel="stylesheet" media="mediatype and|not|only (media feature)" href="mystylesheet.css">

示例
<link rel="stylesheet" href="styleA.css" media="screen and (min-width: 400px)">
```

## rem适配方案
- 让一些不能等比自适应的元素，达到当设备尺寸发生改变的时候，等比例适配当前设备。
- 使用媒体查询根据不同设备按比例设置html的字体大小，然后页面元素使用rem做尺寸单位，当html字体大小变化元素尺寸也会发生变化，从而达到等比缩放的适配。


# 响应式布局
## 响应式开发
就是使用媒体查询针对不同宽度的设备进行布局和样式的设置，从而适配不同设备的目的。
- 超小屏幕（手机） < 768px 
- 小屏设备（平板） >= 768px ~ < 992px 
- 中等屏幕（桌面显示器） >= 992px ~ <1200px 
- 宽屏设备（大桌面显示器） >= 1200px
[案例：响应式导航](资料/05-响应式布局开发/响应式布局开发/02-响应式导航.html)

## Bootstrap前端开发框架
与其说是框架，感觉更类似组件库
