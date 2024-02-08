# day01-快速上手+插值表达式+指令上
Vue2官网：[https://v2.cn.vuejs.org/](https://v2.cn.vuejs.org/)
Vue (读音 /vjuː/，类似于 view) 是一套 **构建用户界面** 的 **渐进式** **框架**
> 所谓渐进式就是循序渐进，不一定非得把Vue中的所有API都学完才能开发Vue，可以学一点开发一点

## 一、创建Vue实例

**核心步骤（4步）：**
1. 准备容器
2. 引包（官网） — 开发版本/生产版本
3. 创建Vue实例 new Vue()
4. 指定配置项，渲染数据
    1. el:指定挂载点
    2. data提供数据

```html
<!-- Vue所管理的范围 -->
<div id="app">
  <!-- 这里将来会编写一些用于渲染的代码逻辑 -->
  <h1>{{ msg }}</h1>
  <a href="#">{{ count }}</a>
</div>

<!-- 引入的是开发版本包 - 包含完整的注释和警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

<script>
  // 一旦引入 VueJS核心包，在全局环境，就有了 Vue 构造函数
  const app = new Vue({
    // 通过 el 配置选择器，指定 Vue 管理的是哪个盒子
    el: '#app',
    // 通过 data 提供数据
    data: {
      msg: 'Hello 传智播客',
      count: 666
    }
  })
</script>
```

## 二、插值表达式{{}}
插值表达式是一种Vue的模板语法
我们可以用插值表达式渲染出Vue提供的数据
> 表达式：是可以被求值的代码，JS引擎会将其计算出一个结果

**插值表达式语法**：{{ 表达式 }}
```html
<h3>{{title}}<h3>
<p>{{nickName.toUpperCase()}}</p>
<p>{{age >= 18 ? '成年':'未成年'}}</p>
<p>{{obj.name}}</p>
<p>{{fn()}}</p>
```

**错误用法**
```html
1.在插值表达式中使用的数据 必须在data中进行了提供
<p>{{hobby}}</p>  //如果在data中不存在 则会报错

2.支持的是表达式，而非语句，比如：if   for ...
<p>{{if}}</p>

3.不能在标签属性中使用 {{  }} 插值 (插值表达式只能标签中间使用)
<p title="{{username}}">我是P标签</p>
```


## 三、响应式特性
简单理解就是数据变，视图对应变。

**访问 和 修改 data中的数据（响应式演示）**
data中的数据, 最终会被添加到实例上
① 访问数据： "实例.属性名"
② 修改数据： "实例.属性名"= "值"

```js
const app = new Vue({
  el: '#app',
  data: {
    // 响应式数据 → 数据变化了，视图自动更新
    msg: '你好，黑马',
    count: 100
  }
})
// data中的数据，是会被添加到实例上
// 1. 访问数据  实例.属性名
// 2. 修改数据  实例.属性名 = 新值
app.msg
app.msg = '你好，vue'
```
> 可以通过控制台更新数据

## 四、Vue开发者工具安装

1. 通过谷歌应用商店安装
2. 极简插件下载 [https://chrome.zzzmh.cn/index](https://chrome.zzzmh.cn/index)
[vuejs-devtools](https://chromewebstore.google.com/detail/nhdogjmejiglipccpnnnanhbledajbpd)
详情 - 允许访问文件网址

## 五、Vue中的常用指令

**概念：** 指令（Directives）是 Vue 提供的带有 **v- 前缀** 的 特殊 标签 **属性**。

vue 中的指令按照不同的用途可以分为如下 6 大类：
- 内容渲染指令（v-html、v-text）
- 条件渲染指令（v-show、v-if、v-else、v-else-if）
- 事件绑定指令（v-on）
- 属性绑定指令 （v-bind）
- 双向绑定指令（v-model）
- 列表渲染指令（v-for）

### 内容渲染指令
内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下2 个：

- v-text（类似innerText）
    使用语法：`<p v-text="uname">hello</p>`，意思是将 uame 值渲染到 p 标签中
    类似 innerText，使用该语法，会覆盖 p 标签原有内容
- v-html（类似 innerHTML）
    使用语法：`<p v-html="intro">hello</p>`，意思是将 intro 值渲染到 p 标签中
    类似 innerHTML，使用该语法，会覆盖 p 标签原有内容
    类似 innerHTML，使用该语法，能够将HTML标签的样式呈现出来。

代码演示：
```html
  <div id="app">
    <h2>个人信息</h2>
	// 既然指令是vue提供的特殊的html属性，所以咱们写的时候就当成属性来用即可
    <p v-text="uname">姓名：</p> 
    <p v-html="intro">简介：</p>
  </div> 

<script>
        const app = new Vue({
            el:'#app',
            data:{
                uname:'张三',
                intro:'<h2>这是一个<strong>非常优秀</strong>的boy<h2>'
            }
        })
</script>
```


### 条件渲染指令
1. v-show
    1. 作用： 控制元素显示隐藏
    2. 语法： v-show = "表达式" 表达式值为 true 显示， false 隐藏
    3. 原理： 切换 display:none 控制显示隐藏
    4. 场景：频繁切换显示隐藏的场景

2. v-if
    1. 作用： 控制元素显示隐藏（条件渲染）
    2. 语法： v-if= "表达式" 表达式值 true显示， false 隐藏
    3. 原理： 基于条件判断，是否创建 或 移除元素节点
    4. 场景： 要么显示，要么隐藏，不频繁切换的场景

```html
<!-- 
  v-show底层原理：切换 css 的 display: none 来控制显示隐藏
  v-if  底层原理：根据 判断条件 控制元素的 创建 和 移除（条件渲染）
-->

<div id="app">
  <div v-show="flag" class="box">我是v-show控制的盒子</div>
  <div v-if="flag" class="box">我是v-if控制的盒子</div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      flag: false
    }
  })
</script>
```

1. v-else 和 v-else-if
    1. 作用：辅助v-if进行判断渲染
    2. 语法：v-else v-else-if="表达式"
    3. 需要紧接着v-if使用

```html
<div id="app">
  <p v-if="gender === 1">性别：♂ 男</p>
  <p v-else>性别：♀ 女</p>
  <hr>
  <p v-if="score >= 90">成绩评定A：奖励电脑一台</p>
  <p v-else-if="score >= 70">成绩评定B：奖励周末郊游</p>
  <p v-else-if="score >= 60">成绩评定C：奖励零食礼包</p>
  <p v-else>成绩评定D：惩罚一周不能玩手机</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      gender: 2,
      score: 95
    }
  })
</script>
```


### 事件绑定指令
使用Vue时，如需为DOM注册事件，及其的简单，语法如下：
- `<button v-on:事件名="内联语句">按钮</button>`
- `<button v-on:事件名="处理函数">按钮</button>`
- `<button v-on:事件名="处理函数(实参)">按钮</button>`
- `v-on:` 简写为 **@**

**1、内联语句**
```html
<div id="app">
    <button @click="count--">-</button>
    <span>{{ count }}</span>
    <button v-on:click="count++">+</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        count: 100
      }
    })
  </script>
```

**2、事件处理函数**
注意：
- 事件处理函数应该写到一个跟data同级的配置项（methods）中
- methods中的函数内部的this都指向Vue实例

```html
<div id="app">
  <button @click="fn">切换显示隐藏</button>
  <h1 v-show="isShow">黑马程序员</h1>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app4 = new Vue({
    el: '#app',
    data: {
      isShow: true
    },
    methods: {
      fn () {
        // Vue让提供的所有methods中的函数，this都指向当前实例
        // console.log('执行了fn', app.isShow)
        // console.log(app3 === this)
        this.isShow = !this.isShow
      }
    }
  })
</script>
```

**3、给事件处理函数传参**
`<button @click="fn(参数1,参数2)"></button>`
- 如果不传递任何参数，则方法无需加小括号；methods方法中可以直接使用 e 当做事件对象
- 如果传递了参数，则实参 `$event` 表示事件对象，固定用法。

```html
<div id="app">
  <div class="box">
    <h3>小黑自动售货机</h3>
    <button @click="buy(5)">可乐5元</button>
    <button @click="buy(10)">咖啡10元</button>
    <button @click="buy(8)">牛奶8元</button>
  </div>
  <p>银行卡余额：{{ money }}元</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      money: 100
    },
    methods: {
      buy (price) {
        this.money -= price
      }
    }
  })
</script>
```


### 属性绑定指令
1. **作用：** 动态设置html的标签属性 比如：src、url、title
2. **语法**：**v-bind:** 属性名=“表达式”
3. **v-bind:** 可以简写成 => **:**

比如，有一个图片，它的 `src` 属性值，是一个图片地址。这个地址在数据 data 中存储。
则可以这样设置属性值：
- `<img v-bind:src="url" />`
- `<img :src="url" />` （v-bind可以省略）

```html
<div id="app">
  <!-- v-bind:src   =>   :src -->
  <img v-bind:src="imgUrl" v-bind:title="msg" alt="">
  <img :src="imgUrl" :title="msg" alt="">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      imgUrl: './imgs/10-02.png',
      msg: 'hello 波仔'
    }
  })
</script>
```

#### 小案例-波仔的学习之旅
需求：默认展示数组中的第一张图片，点击上一页下一页来回切换数组中的图片
实现思路：
1. 数组存储图片路径 `['url1','url2','url3'，...]`
2. 可以准备个下标index 去数组中取图片地址。
3. 通过v-bind给src绑定当前的图片地址
4. 点击上一页下一页只需要修改下标的值即可
5. 当展示第一张的时候，上一页按钮应该隐藏。展示最后一张的时候，下一页按钮应该隐藏

```html
<div id="app">
  <button v-show="index > 0" @click="index--">上一页</button>
  <div>
    <img :src="list[index]" alt="">
  </div>
  <button v-show="index < list.length - 1" @click="index++">下一页</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      index: 0,
      list: [
        './imgs/11-00.gif',
        './imgs/11-01.gif',
        './imgs/11-02.gif',
        './imgs/11-03.gif',
        './imgs/11-04.png',
        './imgs/11-05.png',
      ]
    }
  })
</script>
```

### 列表渲染指令
Vue 提供了 v-for 列表渲染指令，用来辅助开发者基于一个数组来循环渲染一个列表结构。
v-for 指令需要使用 `(item, index) in arr` 形式的特殊语法，其中：
- item 是数组中的每一项
- index 是每一项的索引，不需要可以省略
- arr 是被遍历的数组

此语法也可以遍历**对象和数字**

```html
//遍历数组
<li v-for="(item, index) in list">{{ item }} - {{ index }}</li>
<li v-for="item in list">{{ item }}</li>

//遍历对象
<div v-for="(value, key, index) in object">{{value}}</div>
value:对象中的值
key:对象中的键
index:遍历索引从0开始

//遍历数字
<p v-for="item in 10">{{item}}</p>
item从1 开始
```

#### 小案例-小黑的书架
需求：
	![](assets/1681896632672.png) 
1. 根据左侧数据渲染出右侧列表（v-for）
2. 点击删除按钮时，应该把当前行从列表中删除（获取当前行的id，利用filter进行过滤）

```html
<div id="app">
  <h3>小黑的书架</h3>
  <ul>
    <li v-for="(item, index) in booksList" :key="item.id">
      <span>{{ item.name }}</span>
      <span>{{ item.author }}</span>
      <!-- 注册点击事件 →  通过 id 进行删除数组中的 对应项 -->
      <button @click="del(item.id)">删除</button>
    </li>
  </ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      booksList: [
        { id: 1, name: '《红楼梦》', author: '曹雪芹' },
        { id: 2, name: '《西游记》', author: '吴承恩' },
        { id: 3, name: '《水浒传》', author: '施耐庵' },
        { id: 4, name: '《三国演义》', author: '罗贯中' }
      ]
    },
    methods: {
      del (id) {
        // console.log('删除', id)
        // 通过 id 进行删除数组中的 对应项 → filter(不会改变原数组)
        // filter: 根据条件，保留满足条件的对应项，得到一个新数组。
        // console.log(this.booksList.filter(item => item.id !== id))
        this.booksList = this.booksList.filter(item => item.id !== id)
      }
    }
  })
</script>
```

#### v-for中的key
**语法：** key="唯一值"
**作用：** 给列表项添加的**唯一标识**。便于Vue进行列表项的**正确排序复用**。
**为什么加key：** Vue 的默认行为会尝试原地修改元素（**就地复用**）

实例代码：
```html
<ul>
  <li v-for="(item, index) in booksList" :key="item.id">
    <span>{{ item.name }}</span>
    <span>{{ item.author }}</span>
    <button @click="del(item.id)">删除</button>
  </li>
</ul>
```
注意：
1. key 的值只能是字符串 或 数字类型
2. key 的值必须具有唯一性
3. 推荐使用 id 作为 key（唯一），不推荐使用 index 作为 key（会变化，不对应）

### 双向绑定指令
所谓双向绑定就是：
1. 数据改变后，呈现的页面结果会更新
2. 页面结果更新后，数据也会随之而变

**作用：** 给**表单元素**（input、radio、select）使用，双向绑定数据，可以快速 **获取** 或 **设置** 表单元素内容
**语法：** v-model="变量"

**需求：** 使用双向绑定实现以下需求
	![](assets/1681913125738.png) 

1. 点击登录按钮获取表单中的内容
2. 点击重置按钮清空表单中的内容

```html
<div id="app">
  <!-- 
    v-model 可以让数据和视图，形成双向数据绑定
    (1) 数据变化，视图自动更新
    (2) 视图变化，数据自动更新
    可以快速[获取]或[设置]表单元素的内容
   -->
  账户：<input type="text" v-model="username"> <br><br>
  密码：<input type="password" v-model="password"> <br><br>
  <button @click="login">登录</button>
  <button @click="reset">重置</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      username: '',
      password: ''
    },
    methods: {
      login () {
        console.log(this.username, this.password)
      },
      reset () {
        this.username = ''
        this.password = ''
      }
    }
  })
</script>
```

## 综合案例-小黑记事本
**功能需求：**
1. 列表渲染
2. 删除功能
3. 添加功能
4. 底部统计 和 清空

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<link rel="stylesheet" href="./css/index.css" />
<title>记事本</title>
</head>
<body>

<!-- 主体区域 -->
<section id="app">
  <!-- 输入框 -->
  <header class="header">
    <h1>小黑记事本</h1>
    <input v-model="todoName"  placeholder="请输入任务" class="new-todo" />
    <button @click="add" class="add">添加任务</button>
  </header>
  <!-- 列表区域 -->
  <section class="main">
    <ul class="todo-list">
      <li class="todo" v-for="(item, index) in list" :key="item.id">
        <div class="view">
          <span class="index">{{ index + 1 }}.</span> <label>{{ item.name }}</label>
          <button @click="del(item.id)" class="destroy"></button>
        </div>
      </li>
    </ul>
  </section>
  <!-- 统计和清空 → 如果没有任务了，底部隐藏掉 → v-show -->
  <footer class="footer" v-show="list.length > 0">
    <!-- 统计 -->
    <span class="todo-count">合 计:<strong> {{ list.length }} </strong></span>
    <!-- 清空 -->
    <button @click="clear" class="clear-completed">
      清空任务
    </button>
  </footer>
</section>

<!-- 底部 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  // 添加功能
  // 1. 通过 v-model 绑定 输入框 → 实时获取表单元素的内容
  // 2. 点击按钮，进行新增，往数组最前面加 unshift
  const app = new Vue({
    el: '#app',
    data: {
      todoName: '',
      list: [
        { id: 1, name: '跑步一公里' },
        { id: 2, name: '跳绳200个' },
        { id: 3, name: '游泳100米' },
      ]
    },
    methods: {
      del (id) {
        // console.log(id) => filter 保留所有不等于该 id 的项
        this.list = this.list.filter(item => item.id !== id)
      },
      add () {
        if (this.todoName.trim() === '') {
          alert('请输入任务名称')
          return
        }
        this.list.unshift({
          id: +new Date(),
          name: this.todoName
        })
        this.todoName = ''
      },
      clear () {
        this.list = []
      }
    }
  })

</script>
</body>
</html>
```

```js
this.list.unshift({ 
	id: +new Date(), 
	name: this.todoName 
})
```
1. `this.list.unshift({`：`unshift` 是 JavaScript 中数组的一个方法，它会向数组的开头添加一个或多个元素，并返回新的长度。`this.list` 指的是当前对象的 `list` 属性，这个属性应该是一个数组。
2. `id: +new Date(),`：这一行创建了一个新的日期对象（通过 `new Date()`），然后使用 `+` 运算符将这个日期对象转换为一个时间戳（从1970年1月1日00:00:00 UTC到现在的毫秒数）。这个时间戳被赋值给新对象的 `id` 属性。

```js
if (this.todoName.trim() === '') {
  alert('请输入任务名称')
  return
}
```
`trim()` 是一个JavaScript的字符串方法，用于删除字符串两端的空格。