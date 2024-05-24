## CSS属性书写顺序（重点）
在书写CSS时，按照一定的顺序来组织属性可以提高代码的可读性和可维护性。以下是一些流行的最佳实践，用于规范CSS属性的书写顺序：

### 1. 按类别分类
将CSS属性分为不同的类别，并按类别的顺序排列。常见的类别顺序如下：
1. **布局属性** (如 `display`, `position`, `top`, `right`, `bottom`, `left`, `float`, `clear`, `z-index`)
2. **盒模型属性** (如 `width`, `height`, `margin`, `padding`, `border`, `box-sizing`)
3. **排版属性** (如 `font`, `line-height`, `text-align`, `text-decoration`, `color`)
4. **背景属性** (如 `background`, `background-color`, `background-image`, `background-position`, `background-repeat`)
5. **其他属性** (如 `opacity`, `transition`, `transform`, `animation`)

### 2. 具体的CSS属性顺序示例
这里提供一个具体的CSS属性书写顺序示例，基于上面的类别分类：
1. **布局属性**
    ```css
    display
    position
    top
    right
    bottom
    left
    z-index
    float
    clear
    ```
2. **盒模型属性**
    ```css
    width
    height
    margin
    padding
    border
    border-radius
    box-sizing
    ```
3. **排版属性**
    ```css
    font
    font-family
    font-size
    font-weight
    line-height
    text-align
    text-decoration
    color
    ```
4. **背景属性**
    ```css
    background
    background-color
    background-image
    background-position
    background-repeat
    ```
5. **其他属性**
    ```css
    opacity
    transition
    transform
    animation
    ```

### 3. 示例代码
以下是一个示例，展示如何按照上述顺序书写CSS属性：
```css
.element {
    /* 布局属性 */
    display: block;
    position: relative;
    top: 10px;
    right: 20px;
    z-index: 1;
    float: left;

    /* 盒模型属性 */
    width: 100px;
    height: 200px;
    margin: 10px;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;

    /* 排版属性 */
    font-family: Arial, sans-serif;
    font-size: 16px;
    font-weight: bold;
    line-height: 1.5;
    text-align: center;
    text-decoration: none;
    color: #333;

    /* 背景属性 */
    background: url('image.png') no-repeat center center;
    background-color: #f5f5f5;

    /* 其他属性 */
    opacity: 0.9;
    transition: all 0.3s ease;
    transform: translateY(-10px);
    animation: fadeIn 1s;
}
```

### 4. 工具和框架
许多开发者工具和CSS框架也有推荐的属性书写顺序，例如：
- **CSSComb**: 一个CSS代码风格格式化工具，允许定制属性顺序。
- **Prettier**: 一个代码格式化工具，可以通过插件配置CSS属性顺序。
- **Bootstrap**: 提供了一定的属性书写顺序，可以参考其源代码风格。

## 定位(position) 
**场景1**： 某个元素可以自由的在一个盒子内移动位置，并且压住其他盒子.
**场景2**：当我们滚动窗口的时候，盒子是固定屏幕某个位置的。
定位可以让盒子自由的在某个盒子内移动位置或者固定屏幕中某个位置，并且可以压住其他盒子。

### 定位组成
**定位 = 定位模式 + 边偏移**
**定位模式** 用于指定一个元素在文档中的定位方式。**边偏移**则决定了该元素的最终位置。

#### 边偏移（方位名词）
**边偏移** 就是定位的盒子移动到最终位置。有 top、bottom、left 和 right 4 个属性。

|边偏移属性|示例|描述|
|---|---|---|
|`top`|`top: 80px`|**顶端**偏移量，定义元素相对于其父元素**上边线的距离**。|
|`bottom`|`bottom: 80px`|**底部**偏移量，定义元素相对于其父元素**下边线的距离**。|
|`left`|`left: 80px`|**左侧**偏移量，定义元素相对于其父元素**左边线的距离**。|
|`right`|`right: 80px`|**右侧**偏移量，定义元素相对于其父元素**右边线的距离**|

定位的盒子有了边偏移才有价值。 一般情况下，凡是有定位地方必定有边偏移。

#### 定位模式 (position)
在 CSS 中，通过 `position` 属性定义元素的**定位模式**，语法如下：
```css
选择器 { 
    position: 属性值; 
}
```
定位模式是有不同分类的，在不同情况下，我们用到不同的定位模式。
定位模式决定元素的定位方式 ，它通过 CSS 的 position 属性来设置，其值可以分为四个：

| 值         |     语义     |
| ---------- | :----------: |
| `static`   | **静态**定位 |
| `relative` | **相对**定位 |
| `absolute` | **绝对**定位 |
| `fixed`    | **固定**定位 |

![](assets/Pasted%20image%2020240523103406.png)

### 定位模式介绍
#### 静态定位(static) - 了解
**静态定位**是元素的**默认**定位方式，**无定位的意思**。

#### 相对定位(relative) - 重要
- **相对定位**是元素在移动位置的时候，是相对于它自己**原来的位置**来说的（移动位置的时候参照点是自己原来的位置）
- **原来**在标准流的**位置**继续占有，后面的盒子仍然以标准流的方式对待它。
	因此，**相对定位并没有脱标**。它最典型的应用是给绝对定位当爹的。。。
![](assets/Pasted%20image%2020240523104545.png)

#### 绝对定位(absolute) - 重要
**绝对定位**是元素在移动位置的时候，是相对于它**祖先元素**来说的
1. **完全脱标** —— 完全不占位置；
2. **父元素要有定位**，元素将依据最近的已经定位（绝对、固定或相对定位）的父元素（祖先）进行定位。
3. **父元素没有定位**，则以**浏览器**为准定位（Document 文档）。

##### 定位口诀 —— 子绝父相

#### 固定定位(fixed) - 重要
**固定定位**是元素**固定于浏览器可视区的位置**。主要使用场景： 可以在浏览器页面滚动时元素的位置不会改变。
固定定位也是**脱标**的

#### 粘性定位(sticky) - 了解
**粘性定位**可以被认为是相对定位和固定定位的混合
1. 以浏览器的可视窗口为参照点移动元素（固定定位特点）
2. 粘性定位占有原先的位置（相对定位特点）
3. 必须添加 top 、left、right、bottom **其中一个**才有效

#### 定位总结

|**定位模式**|**是否脱标**|**移动位置**|**是否常用**|
|---|---|---|---|
|static 静态定位|否|不能使用边偏移|很少|
|**relative 相对定位**|**否 (占有位置)**|**相对于自身位置移动**|**基本单独使用**|
|**absolute绝对定位**|**是（不占有位置）**|**带有定位的父级**|**要和定位父级元素搭配使用**|
|**fixed 固定定位**|**是（不占有位置）**|**浏览器可视区**|**单独使用，不需要父级**|
|sticky 粘性定位|否 (占有位置)|浏览器可视区|当前阶段少|

- 注意：
1. **边偏移**需要和**定位模式**联合使用，**单独使用无效**；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。

## 定位(position)的应用
### 1. 固定定位小技巧： 固定在版心左侧位置
[固定定位小技巧.html](assets/06-固定定位小技巧.html)
小算法：
1. 让固定定位的盒子 left: 50%. 走到浏览器可视区（也可以看做版心） 的一半位置。
2. 让固定定位的盒子 margin-left: 版心宽度的一半距离。 多走 版心宽度的一半位置
就可以让固定定位的盒子**贴着版心右侧对齐**了。
![](assets/Pasted%20image%2020240523145600.png)
![](assets/Pasted%20image%2020240523145608.png)

### 2. 堆叠顺序（z-index）
- 在使用**定位**布局时，可能会**出现盒子重叠的情况**。此时，可以使用 **z-index** 来控制盒子的前后次序 (z轴)
- 语法：
  ```css
  选择器 { 
  	z-index: 1; 
  }
  ```
- `z-index` 的特性如下：
  1. **属性值**：**正整数**、**负整数**或 **0**，默认值是 0，数值越大，盒子越靠上；	
  2. 如果**属性值相同**，则按照书写顺序，**后来居上**；
  3. 数字后面**不能加单位**。

  **注意**：`z-index` 只能应用于**相对定位**、**绝对定位**和**固定定位**的元素，其他**标准流**、**浮动**和**静态定位**无效。

- 应用 `z-index` 层叠等级属性可以**调整盒子的堆叠顺序**。如下图所示：
![](assets/Pasted%20image%2020240523150046.png)


## 定位(position)的拓展
### 1 绝对定位的盒子居中
**注意**：加了**绝对定位/固定定位的盒子**不能通过设置 `margin: auto` 设置**水平居中**。
但是可以通过以下计算方法实现水平和垂直居中，可以按照下图的方法：
![](assets/Pasted%20image%2020240523150456.png)
1. `left: 50%;`：让**盒子的左侧**移动到**父级元素的水平中心位置**；
2. `margin-left: -100px;`：让盒子**向左**移动**自身宽度的一半**。

### 2 定位特殊特性
一个行内的盒子，如果加了**浮动**、**固定定位**和**绝对定位**，不用转换，就可以给这个盒子直接设置宽度和高度等。

### 3 脱标的盒子不会触发外边距塌陷
给盒子改为了浮动或者定位，就不会有垂直**外边距合并的问题**了。

### 4 绝对定位（固定定位）会完全压住盒子
浮动元素，只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字（图片）
但是绝对定位（固定定位） 会压住下面标准流所有的内容。
浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的。 文字会围绕浮动元素

## 元素的显示与隐藏
### 1. display 显示（重点）
- display 设置或检索对象是否及如何显示。
  ```css
  display: none 隐藏对象
  display: block 除了转换为块级元素之外，同时还有显示元素的意思。
  ```
- 特点： display 隐藏元素后，**不再占**有原来的位置。
![](assets/Pasted%20image%2020240523151250.png)

### 2. visibility 可见性 （了解）
- visibility 属性用于指定一个元素应可见还是隐藏。
  ```css
  visibility: visible; 　元素可视
  visibility: hidden; 　  元素隐藏
  ```
- 特点：**visibility 隐藏元素后，继续占有原来的位置**。
- 如果隐藏元素想要原来位置， 就用 visibility：hidden
- 如果隐藏元素不想要原来位置， 就用 display：none  (用处更多 重点）
![](assets/Pasted%20image%2020240523151257.png)

### 3. overflow 溢出（重点）
- overflow 属性指定了如果内容溢出一个元素的框（超过其指定高度及宽度） 时，会发生什么。

| 属性值      | 描述                                       |
| ----------- | ------------------------------------------ |
| **visible** | 不剪切内容也不添加滚动条                   |
| **hidden**  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| **scroll**  | 不管超出内容否，总是显示滚动条             |
| **auto**    | 超出自动显示滚动条，不超出不显示滚动条     |

-  一般情况下，我们都不想让溢出的内容显示出来，因为溢出的部分会影响布局。
-  但是如果有定位的盒子， 请慎用overflow:hidden  因为它会隐藏多余的部分。

### 显示与隐藏总结

| 属性                           | 区别                   | 用途                                                         |
| ------------------------------ | ---------------------- | ------------------------------------------------------------ |
| **display 显示     （重点）**  | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
| **visibility 可见性 （了解）** | 隐藏对象，保留位置     | 使用较少                                                     |
| **overflow 溢出（重点）**      | 只是隐藏超出大小的部分 | 1. 可以清除浮动  2. 保证盒子里面的内容不会超出该盒子范围     |


## 综合案例：土豆网鼠标经过显示遮罩
[仿土豆网显示隐藏遮罩案例.html](assets/17-仿土豆网显示隐藏遮罩案例.html)

## 精灵图（重点）
**核心原理**：
将网页中的一些小背景图像整合到一张大图中 ，这样服务器只需要一次请求就可以了。

### 精灵图（sprites）的使用
1. 精灵技术主要针对于背景图片使用。就是把多个小背景图片整合到一张大图片中。
2. 这个大图片也称为 sprites 精灵图 或者 雪碧图
3. 移动背景图片位置， 此时可以使用 background-position 。
4. 移动的距离就是这个目标图片的 x 和 y 坐标。注意网页中的坐标有所不同
5. 因为一般情况下都是往上往左移动，所以数值是负值。
6. 使用精灵图的时候需要精确测量，每个小背景图片的大小和位置。

#### 代码参考
[精灵图使用.html](assets/01-精灵图使用.html)
[拼出自己名字.html](assets/02-拼出自己名字.html)

## 字体图标
### 字体图标的下载
- **icomoon** **字库** [http://icomoon.io](http://icomoon.io/) 推荐指数 **★★★★★**
	IcoMoon 成立于 2011 年，推出了第一个自定义图标字体生成器，它允许用户选择所需要的图标，使它们成一字型。该字库内容种类繁多，非常全面，唯一的遗憾是国外服务器，打开网速较慢。
- **阿里** **iconfont** **字库** [http://www.iconfont.cn/](http://www.iconfont.cn/) 推荐指数 **★★★★★**
	这个是阿里妈妈 M2UX 的一个 iconfont 字体图标字库，包含了淘宝图标库和阿里妈妈图标库。可以使用 AI制作图标上传生成。 重点是，免费！

### 字体图标的引入
不同浏览器所支持的字体格式是不一样的，字体图标之所以兼容，就是因为包含了主流浏览器支持的字体文件。
```css
 @font-face {
   font-family: 'icomoon';
   src:  url('fonts/icomoon.eot?7kkyc2');
   src:  url('fonts/icomoon.eot?7kkyc2#iefix') format('embedded-opentype'),
     url('fonts/icomoon.ttf?7kkyc2') format('truetype'),
     url('fonts/icomoon.woff?7kkyc2') format('woff'),
     url('fonts/icomoon.svg?7kkyc2#icomoon') format('svg');
   font-weight: normal;
   font-style: normal;
 }
```

html 标签内添加小图标。
![](assets/Pasted%20image%2020240523163404.png)
给标签定义字体
```css
 span {  
   font-family: "icomoon";  
 }
```
注意：务必保证 这个字体和上面@font-face里面的字体保持一致


## CSS 三角
网页中常见一些三角形，使用 CSS 直接画出来就可以，不必做成图片或者字体图标。
一张图， 你就知道 CSS 三角是怎么来的了, 做法如下：
![](assets/Pasted%20image%2020240523162900.png)
```css
 div {
 	width: 0; 
    height: 0;
    border: 50px solid transparent;
	border-color: red green blue black;
	line-height:0;
    font-size: 0;
 }
```
1. 我们用css 边框可以模拟三角效果
2. 宽度高度为0
3. 我们4个边框都要写， 只保留需要的边框颜色，其余的不能省略，都改为 transparent 透明就好了
4. 为了照顾兼容性 低版本的浏览器，加上 font-size: 0;  line-height: 0;

## CSS 用户界面样式
### 1 鼠标样式 cursor
```css
 li {
 	cursor: pointer; 
 }
```
设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。
![](assets/Pasted%20image%2020240523163606.png)

### 2 轮廓线 outline
给表单添加 outline: 0;   或者  outline: none; 样式之后，就可以去掉默认的蓝色边框
```css
 input {
 	outline: none; 
 }
```

### 3 防止拖拽文本域 resize
```css
 textarea{ 
 	resize: none;
 }
```


## vertical-align 属性应用
CSS 的 **vertical-align** 属性使用场景： 经常用于设置图片或者表单(行内块元素）和文字垂直对齐。
官方解释： 用于设置一个元素的**垂直对齐方式**，但是它只针对于行内元素或者行内块元素有效。
语法：
```css
vertical-align : baseline | top | middle | bottom 
```
![](assets/Pasted%20image%2020240523163804.png)
![](assets/Pasted%20image%2020240523163916.png)

### 图片、表单和文字对齐
图片、表单都属于行内块元素，默认的 vertical-align 是基线对齐。
![](assets/Pasted%20image%2020240523163951.png)
此时可以给图片、表单这些行内块元素的 **vertical-align 属性设置为 middle** 就可以让文字和图片垂直居中对齐了。

## 溢出的文字省略号显示
### 1 单行文本溢出显示省略号
单行文本溢出显示省略号--必须满足三个条件：
```css
  /*1. 先强制一行内显示文本*/
   white-space: nowrap;  （ 默认 normal 自动换行）
   
  /*2. 超出的部分隐藏*/
   overflow: hidden;
   
  /*3. 文字用省略号替代超出的部分*/
   text-overflow: ellipsis;
```

### 2 多行文本溢出显示省略号（了解）
多行文本溢出显示省略号，**有较大兼容性问题**，适合于webKit浏览器或移动端（移动端大部分是webkit内核）
```css
/*1. 超出的部分隐藏 */
overflow: hidden;

/*2. 文字用省略号替代超出的部分 */
text-overflow: ellipsis;

/* 3. 弹性伸缩盒子模型显示 */
display: -webkit-box;

/* 4. 限制在一个块元素显示的文本的行数 */
-webkit-line-clamp: 2;

/* 5. 设置或检索伸缩盒对象的子元素的排列方式 */
-webkit-box-orient: vertical;
```


## CSS 初始化
不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对HTML文本呈现的差异，照顾浏览器的兼容，我们需要对CSS 初始化
[京东css初始化.css](assets/14-京东css初始化.css)


## HTML5新特性

### 语义化标签 （★★）
以前布局，我们基本用 div 来做。div 对于搜索引擎来说，是没有语义的
```html
<div class=“header”> </div>
<div class=“nav”> </div>
<div class=“content”> </div>
<div class=“footer”> </div>
```
发展到了HTML5后，新增了一些语义化标签，这样的话更加有利于浏览器的搜索引擎搜索，也方便了网站的seo（Search Engine Optimization，搜索引擎优化），下面就是新增的一些语义化标签
- `<header>` 头部标签
- `<nav>` 导航标签
- `<article>` 内容标签
- `<section>` 定义文档某个区域
- `<aside>` 侧边栏标签
- `<footer>` 尾部标签

### 多媒体标签
```html
 <video src="media/mi.mp4"></video>
 <audio src="media/music.mp3"></audio>
```
#### video 常用属性
![](assets/Pasted%20image%2020240523170150.png)
- 注意： 在google浏览器上面，默认禁止了自动播放，如果想要自动播放的效果，需要设置 muted属性
#### audio 常用属性
![](assets/Pasted%20image%2020240523170235.png)

### 新增的表单元素 （★★）
**常见输入类型**
```
text password radio checkbox button file hidden submit reset image
```
**新的输入类型**
![](assets/Pasted%20image%2020240523170455.png)

## CSS3新特性
### 属性选择器（★★）
属性选择器，按照字面意思，都是根据标签中的属性来选择元素
![](assets/Pasted%20image%2020240523170637.png)

### 结构伪类选择器
结构伪类选择器主要根据文档结构来选择器元素， 常用于根据父级选择器里面的子元素
![](assets/Pasted%20image%2020240523170731.png)

#### E:nth-child(n)（★★★）
匹配到父元素的第n个元素
- 匹配到父元素的第2个子元素  
  `ul li:nth-child(2){}`
- 匹配到父元素的序号为奇数的子元素
  `ul li:nth-child(odd){}`    **odd** 是关键字  奇数的意思（3个字母 ）
- 匹配到父元素的序号为偶数的子元素
  `ul li:nth-child(even){}`   **even**（4个字母 ）
- **匹配到父元素的前3个子元素**
  `ul li:nth-child(-n+3){}`    
  选择器中的  **n** 是怎么变化的呢？
  因为 n是从 0 ，1，2，3.. 一直递增
  所以 -n+3 就变成了   
  - n=0 时   -0+3=3
  - n=1时    -1+3=2
  - n=2时    -2+3=1
  - n=3时    -3+3=0 
  - ...

**一些常用的公式： 公式不是死的，在这里列举出来让大家能够找寻到这个模式，能够理解代码，这样才能写出满足自己功能需求的代码**
![](assets/Pasted%20image%2020240523171209.png)

#### E:nth-child 与 E:nth-of-type 的区别
![](assets/Pasted%20image%2020240523171554.png)

### 伪元素选择器（★★★）
伪元素选择器可以帮助我们利用CSS创建新标签元素，而不需要HTML标签，从而简化HTML结构
![](assets/Pasted%20image%2020240523171641.png)
```html
<style>
    div {
        width: 200px;
        height: 200px;
        background-color: pink;
    }
    /* div::before 权重是2 */
    div::before {
        /* 这个content是必须要写的 */
        content: '我';
    }
    div::after {
        content: '小猪佩奇';
    }
</style>
<body>
    <div>
        是
    </div>
</body>
```

注意：
- before 和 after 创建一个元素，但是属于行内元素
- 新创建的这个元素在文档树中是找不到的，所以我们称为伪元素
- before 和 after 必须有 content 属性

#### 应用场景一： 字体图标
在实际工作中，字体图标基本上都是用伪元素来实现的，好处在于我们不需要在结构中额外去定义字体图标的标签，通过content属性来设置字体图标的 编码
**步骤：**
- 结构中定义div盒子
- 在style中先申明字体 @font-face
- 在style中定义after伪元素 div::after{...}
- 在after伪元素中 设置content属性，属性的值就是字体编码
- 在after伪元素中 设置font-family的属性
- 利用定位的方式，让伪元素定位到相应的位置；记住定位口诀：子绝父相
```css
div::after {
	position: absolute;
	top: 10px;
	right: 10px;
	font-family: 'icomoon';
	/* content: ''; */
	content: '\e91e';
	color: red;
	font-size: 18px;
}
```

#### 应用场景二： 遮罩效果
```css
.tudou::before {
	content: '';
	/* 隐藏遮罩层 */
	display: none;
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, .4) url(images/arr.png) no-repeat center;
}

/* 当我们鼠标经过了 土豆这个盒子，就让里面before遮罩层显示出来 */
.tudou:hover::before {
	/* 而是显示元素 */
	display: block;
}
```


### 盒子模型（★★★）
CSS3 中可以通过 box-sizing 来指定盒模型，有2个值：即可指定为 content-box、border-box，这样我们计算盒子大小的方式就发生了改变
可以分成两种情况：
- box-sizing: content-box 盒子大小为 width + padding + border （以前默认的）
- box-sizing: border-box 盒子大小为 width

如果盒子模型我们改为了box-sizing: border-box ， 那padding和border就不会撑大盒子了（前提padding和border不会超过width宽度）

### 图标变模糊 -- CSS3滤镜filter
```css
filter:   函数(); -->  例如： filter: blur(5px);  -->  blur模糊处理  数值越大越模糊
```
[图片模糊处理filter.html](assets/14-图片模糊处理filter.html)

### 计算盒子宽度 -- calc 函数
calc() 此CSS函数让你在声明CSS属性值时执行一些计算
```css
width: calc(100% - 80px);
```
括号里面可以使用 + - * / 来进行计算


## 广义H5
广义的 HTML5 是 HTML5 本身 + CSS3 + JavaScript 。

## 品优购项目规划
### 网站制作流程
![](assets/Pasted%20image%2020240523174156.png)
### 品优购项目搭建工作
![](assets/Pasted%20image%2020240523180458.png)

### 网站 favicon 图标（★★★）
```html
 <link rel="shortcut icon" href="favicon.ico"  type="image/x-icon"/> 
```

### TDK三大标签SEO优化（★★）
##### Title（网站标题）
##### description（网站描述）
`<meta name="description" content="京东JD.COM-专业的综合网上购物商城,销售家电、数码通讯、电脑、家居百货、服装服饰、母婴、图书、食品等数万个品牌优质商品.便捷、诚信的服务，为您提供愉悦的网上购物体验!" />`
##### keywords （关键字）
`<meta name= " keywords" content="网上购物,网上商城,手机,笔记本,电脑,MP3,CD,VCD,DV,相机,数码,配件,手表,存储卡,京东" />`


