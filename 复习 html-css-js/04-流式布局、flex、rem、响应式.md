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




