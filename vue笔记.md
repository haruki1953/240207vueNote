https://www.bilibili.com/video/BV1HV4y1a7n4/
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


# day02-指令下+计算属性+侦听器
## 一、指令补充
### 指令修饰符
所谓指令修饰符就是通过“.”指明一些指令**后缀**，不同的**后缀**封装了不同的处理操作 —> 简化代码

#### 按键修饰符
- @keyup.enter —>当点击enter键的时候才触发

```html
  <div id="app">
    <h3>@keyup.enter  →  监听键盘回车事件</h3>
    <input @keyup.enter="fn" v-model="username" type="text">
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: ''
      },
      methods: {
        fn (e) {
          // if (e.key === 'Enter') {
          //   console.log('键盘回车的时候触发', this.username)
          // }

          console.log('键盘回车的时候触发', this.username)
        }
      }
    })
  </script>
```

#### v-model修饰符
- v-model.trim —>去除首位空格
- v-model.number —>转数字

#### 事件修饰符
- @事件名.stop —> 阻止冒泡
- @事件名.prevent —>阻止默认行为
- @事件名.stop.prevent —>可以连用 即阻止事件冒泡也阻止默认行为

```html
  <div id="app">
    <h3>v-model修饰符 .trim .number</h3>
    姓名：<input v-model.trim="username" type="text"><br>
    年纪：<input v-model.number="age" type="text"><br>

    
    <h3>@事件名.stop     →  阻止冒泡</h3>
    <div @click="fatherFn" class="father">
      <div @click.stop="sonFn" class="son">儿子</div>
    </div>

    <h3>@事件名.prevent  →  阻止默认行为</h3>
    <a @click.prevent href="http://www.baidu.com">阻止默认行为</a>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        age: '',
      },
      methods: {
        fatherFn () {
          alert('老父亲被点击了')
        },
        sonFn (e) {
          // e.stopPropagation()
          alert('儿子被点击了')
        }
      }
    })
  </script>
```

### v-bind对样式控制的增强-操作class
为了方便开发者进行样式控制， Vue 扩展了 v-bind 的语法，可以针对 **class 类名** 和 **style 行内样式** 进行控制 。
```html
<div> :class = "对象/数组">这是一个div</div>
```

**对象语法**
当class动态绑定的是**对象**时，**键就是类名，值就是布尔值**，如果值是**true**，就有这个类，否则没有这个类
```html
<div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div>
<div class="box" :class="{ pink: true, big: true }">黑马程序员</div>
```
适用场景：一个类名，来回切换

**数组语法**
当class动态绑定的是**数组**时 → 数组中所有的类，都会添加到盒子上，本质就是一个 class 列表
```html
<div class="box" :class="[ 类名1, 类名2, 类名3 ]"></div>
<div class="box" :class="['pink', 'big']">黑马程序员</div>
```
使用场景：批量添加或删除类

#### 京东秒杀-tab栏切换导航高亮
需求：当我们点击哪个tab页签时，哪个tab页签就高亮
1. 基于数据，动态渲染tab（v-for）
2. 准备一个下标 记录高亮的是哪一个 tab
3. 基于下标动态切换class的类名

```html
  <div id="app">
    <ul>
      <li v-for="(item, index) in list" :key="item.id" @click="activeIndex = index">
        <a :class="{ active: index === activeIndex }" href="#">{{ item.name }}</a>
      </li>
    </ul>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        activeIndex: 2, // 记录高亮
        list: [
          { id: 1, name: '京东秒杀' },
          { id: 2, name: '每日特价' },
          { id: 3, name: '品类秒杀' }
        ]
      }
    })
  </script>
```


### v-bind对有样式控制的增强-操作style
```html
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>
<div class="box" :style="{ width: '400px', height: '400px', 'background-color': 'green' }"></div>
<div class="box" :style="{ width: '400px', height: '400px', backgroundColor: 'green' }"></div>
```
js对象中属性名不支持带横杠（如background-color），应使用引号包裹、或驼峰命名。属性值都用引号包裹（字符串）

#### 进度条案例
```html
  <div id="app">
    <!-- 外层盒子底色 （黑色） -->
    <div class="progress">
      <!-- 内层盒子 - 进度（蓝色） -->
      <div class="inner" :style="{ width: percent + '%' }">
        <span>{{ percent }}%</span>
      </div>
    </div>
    <button @click="percent = 25">设置25%</button>
    <button @click="percent = 50">设置50%</button>
    <button @click="percent = 75">设置75%</button>
    <button @click="percent = 100">设置100%</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        percent: 30
      }
    })
  </script>
```

### v-model在其他表单元素的使用
常见的表单元素都可以用 v-model 绑定关联 → 快速 **获取** 或 **设置** 表单元素的值
它会根据 **控件类型** 自动选取 **正确的方法** 来更新元素
```
输入框  input:text   ——> value  
文本域  textarea    ——> value  
复选框  input:checkbox  ——> checked  
单选框  input:radio   ——> checked  
下拉菜单 select    ——> value  
```

```html
  <div id="app">
    <h3>小黑学习网</h3>

    姓名：
      <input type="text" v-model="username"> 
      <br><br>

    是否单身：
      <input type="checkbox" v-model="isSingle"> 
      <br><br>

    <!-- 
      前置理解：
        1. name:  给单选框加上 name 属性 可以分组 → 同一组互相会互斥
        2. value: 给单选框加上 value 属性，用于提交给后台的数据
      结合 Vue 使用 → v-model
    -->
    性别: 
      <input v-model="gender" type="radio" name="gender" value="1">男
      <input v-model="gender" type="radio" name="gender" value="2">女
      <br><br>

    <!-- 
      前置理解：
        1. option 需要设置 value 值，提交给后台
        2. select 的 value 值，关联了选中的 option 的 value 值
      结合 Vue 使用 → v-model
    -->
    所在城市:
      <select v-model="cityId">
        <option value="101">北京</option>
        <option value="102">上海</option>
        <option value="103">成都</option>
        <option value="104">南京</option>
      </select>
      <br><br>

    自我描述：
      <textarea v-model="desc"></textarea> 

    <button>立即注册</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        isSingle: false,
        gender: "2",
        cityId: '102',
        desc: ""
      }
    })
  </script>
```


## 二、computed计算属性
### 简单使用
**概念：** 基于**现有的数据**，计算出来的**新属性**。 **依赖**的数据变化，**自动**重新计算。
**语法：**
1. 声明在 **computed 配置项**中，一个计算属性对应一个函数
2. 使用起来和普通属性一样使用 {{计算属性名}}

**注意：**
1. computed配置项和data配置项是**同级**的
2. computed中的计算属性**虽然是函数的写法**，但他**依然是个属性**
3. computed中的计算属性**不能**和data中的属性**同名**
4. 使用computed中的计算属性和使用data中的属性是一样的用法
5. computed中计算属性内部的**this**依然**指向的是Vue实例**

**案例：** 比如我们可以使用计算属性实现下面这个业务场景
	![](assets/Pasted%20image%2020240128150812.png) 

```html
  <div id="app">
    <h3>小黑的礼物清单</h3>
    <table>
      <tr>
        <th>名字</th>
        <th>数量</th>
      </tr>
      <tr v-for="(item, index) in list" :key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.num }}个</td>
      </tr>
    </table>

    <!-- 目标：统计求和，求得礼物总数 -->
    <p>礼物总数：{{ totalCount }} 个</p>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        // 现有的数据
        list: [
          { id: 1, name: '篮球', num: 1 },
          { id: 2, name: '玩具', num: 2 },
          { id: 3, name: '铅笔', num: 5 },
        ]
      },
      computed: {
        totalCount () {
          // 基于现有的数据，编写求值逻辑
          // 计算属性函数内部，可以直接通过 this 访问到 app 实例
          // console.log(this.list)

          // 需求：对 this.list 数组里面的 num 进行求和 → reduce
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
    })
  </script>
```

### computed计算属性 VS methods方法
**computed计算属性**
作用：封装了一段对于**数据**的处理，求得一个**结果**
语法：
1. 写在computed配置项中
2. 作为属性，直接使用
    - js中使用计算属性： this.计算属性
    - 模板中使用计算属性：{{计算属性}}

**methods计算属性**
作用：给Vue实例提供一个**方法**，调用以**处理业务逻辑**。
语法：
1. 写在methods配置项中
2. 作为方法调用
    - js中调用：this.方法名()
    - 模板中调用 {{方法名()}} 或者 @事件名=“方法名”

#### 计算属性的优势
1. 缓存特性（提升性能）
    计算属性会对计算出来的结果缓存，再次使用直接读取缓存，
    依赖项变化了，会自动重新计算 → 并再次缓存
2. methods没有缓存特性
3. 通过代码比较

```html
  <div id="app">
    <h3>小黑的礼物清单🛒<span>{{ totalCountFn() }}</span></h3>
    <h3>小黑的礼物清单🛒<span>{{ totalCountFn() }}</span></h3>
    <h3>小黑的礼物清单🛒<span>{{ totalCountFn() }}</span></h3>
    <h3>小黑的礼物清单🛒<span>{{ totalCountFn() }}</span></h3>
    <table>
      <tr>
        <th>名字</th>
        <th>数量</th>
      </tr>
      <tr v-for="(item, index) in list" :key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.num }}个</td>
      </tr>
    </table>

    <p>礼物总数：{{ totalCountFn() }} 个</p>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        // 现有的数据
        list: [
          { id: 1, name: '篮球', num: 3 },
          { id: 2, name: '玩具', num: 2 },
          { id: 3, name: '铅笔', num: 5 },
        ]
      },

      methods: {
        totalCountFn () {
          console.log('methods方法执行了')
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      },

      computed: {
        // 计算属性：有缓存的，一旦计算出来结果，就会立刻缓存
        // 下一次读取 → 直接读缓存就行 → 性能特别高
        totalCount () {
          console.log('计算属性执行了')
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
    })
  </script>
```

#### 总结
1. computed**有缓存特性**，methods**没有缓存**
2. 当一个结果依赖其他多个值时，推荐使用计算属性
3. 当处理业务逻辑时，推荐使用methods方法，比如事件的处理函数


### 计算属性的完整写法
**既然计算属性也是属性，能访问，应该也能修改了？**
	![](assets/Pasted%20image%2020240128153939.png) 
1. 计算属性默认的简写，只能读取访问，不能 "修改"
2. 如果要 "修改" → 需要写计算属性的完整写法

```html
  <div id="app">
    姓：<input type="text" v-model="firstName"> +
    名：<input type="text" v-model="lastName"> =
    <span>{{ fullName }}</span><br><br>
    
    <button @click="changeName">改名卡</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        firstName: '刘',
        lastName: '备',
      },
      methods: {
        changeName () {
          this.fullName = '黄忠'
        }
      },
      computed: {
        // 简写 → 获取，没有配置设置的逻辑
        // fullName () {
        //   return this.firstName + this.lastName
        // }
        
        // 完整写法 → 获取 + 设置
        fullName: {
          // (1) 当fullName计算属性，被获取求值时，执行get（有缓存，优先读缓存）
          //     会将返回值作为，求值的结果
          get () {
            return this.firstName + this.lastName
          },
          // (2) 当fullName计算属性，被修改赋值时，执行set
          //     修改的值，传递给set方法的形参
          set (value) {
            // console.log(value.slice(0, 1))          
            // console.log(value.slice(1))         
            this.firstName = value.slice(0, 1)
            this.lastName = value.slice(1)
          }
        }
      }
    })
  </script>
```


### 综合案例-成绩案例
![](assets/Pasted%20image%2020240128155410.png)
功能描述：
1. 渲染功能
2. 删除功能
3. 添加功能
4. 统计总分，求平均分

思路分析：
1. 渲染功能 v-for :key v-bind:动态绑定class的样式
2. 删除功能 v-on绑定事件， 阻止a标签的默认行为
3. v-model的修饰符 .trim、 .number、 判断数据是否为空后 再添加、添加后清空文本框的数据
4. 使用计算属性computed 计算总分和平均分的值

```html
<div id="app" class="score-case">
  <div class="table">
	<table>
	  <thead>
		<tr>
		  <th>编号</th>
		  <th>科目</th>
		  <th>成绩</th>
		  <th>操作</th>
		</tr>
	  </thead>

	  <tbody v-if="list.length > 0">
		<tr v-for="(item, index) in list" :key="item.id">
		  <td>{{ index + 1 }}</td>
		  <td>{{ item.subject }}</td>
		  <!-- 需求：不及格的标红, < 60 分, 加上 red 类 -->
		  <td :class="{ red: item.score < 60 }">{{ item.score }}</td>
		  <td><a @click.prevent="del(item.id)" href="http://www.baidu.com">删除</a></td>
		</tr>
	  </tbody>

	  <tbody v-else>
		<tr>
		  <td colspan="5">
			<span class="none">暂无数据</span>
		  </td>
		</tr>
	  </tbody>

	  <tfoot>
		<tr>
		  <td colspan="5">
			<span>总分：{{ totalScore }}</span>
			<span style="margin-left: 50px">平均分：{{ averageScore }}</span>
		  </td>
		</tr>
	  </tfoot>
	</table>
  </div>
  <div class="form">
	<div class="form-item">
	  <div class="label">科目：</div>
	  <div class="input">
		<input
		  type="text"
		  placeholder="请输入科目"
		  v-model.trim="subject"
		/>
	  </div>
	</div>
	<div class="form-item">
	  <div class="label">分数：</div>
	  <div class="input">
		<input
		  type="text"
		  placeholder="请输入分数"
		  v-model.number="score"
		/>
	  </div>
	</div>
	<div class="form-item">
	  <div class="label"></div>
	  <div class="input">
		<button @click="add" class="submit" >添加</button>
	  </div>
	</div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

<script>
  const app = new Vue({
	el: '#app',
	data: {
	  list: [
		{ id: 1, subject: '语文', score: 62 },
		{ id: 7, subject: '数学', score: 89 },
		{ id: 12, subject: '英语', score: 70 },
	  ],
	  subject: '',
	  score: ''
	},
	computed: {
	  totalScore() {
		return this.list.reduce((sum, item) => sum + item.score, 0)
	  },
	  averageScore () {
		if (this.list.length === 0) {
		  return 0
		}
		return (this.totalScore / this.list.length).toFixed(2)
	  }
	},
	methods: {
	  del (id) {
		// console.log(id)
		this.list = this.list.filter(item => item.id !== id)
	  },
	  add () {
		if (!this.subject) {
		  alert('请输入科目')
		  return
		}
		if (typeof this.score !== 'number') {
		  alert('请输入正确的成绩')
		  return
		}
		this.list.unshift({
		  id: +new Date(),
		  subject: this.subject,
		  score: this.score
		})

		this.subject = ''
		this.score = ''
	  }
	}
  })
</script>
```



## 三、watch侦听器（监视器）
**监视数据变化**，执行一些业务逻辑或异步操作
**语法：**
1. watch同样声明在跟data同级的配置项中
2. 简单写法： 简单类型数据直接监视
3. 完整写法：添加额外配置项

#### 简单写法
```js
data: { 
  words: '苹果',
  obj: {
    words: '苹果'
  }
},

watch: {
  // 该方法会在数据变化时，触发执行
  数据属性名 (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  },
  '对象.属性名' (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  }
}

// 演示
data: {
  // words: ''
  obj: {
    words: ''
  }
},
// 具体讲解：(1) watch语法 (2) 具体业务实现
watch: {
  // 该方法会在数据变化时调用执行
  // newValue新值, oldValue老值（一般不用）
  // words (newValue) {
  //   console.log('变化了', newValue)
  // }

  'obj.words' (newValue) {
    console.log('变化了', newValue)
  }
}
```

### 翻译案例-代码实现
[13-watch侦听器-功能实现.html](assets/13-watch侦听器-功能实现.html)
注意点：异步函数、箭头函数中的this、防抖
```html
<div id="app">
  <!-- 条件选择框 -->
  <div class="query">
	<span>翻译成的语言：</span>
	<select>
	  <option value="italy">意大利</option>
	  <option value="english">英语</option>
	  <option value="german">德语</option>
	</select>
  </div>

  <!-- 翻译框 -->
  <div class="box">
	<div class="input-wrap">
	  <textarea v-model="obj.words"></textarea>
	  <span><i>⌨️</i>文档翻译</span>
	</div>
	<div class="output-wrap">
	  <div class="transbox">{{ result }}</div>
	</div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
  // 接口地址：https://applet-base-api-t.itheima.net/api/translate
  // 请求方式：get
  // 请求参数：
  // （1）words：需要被翻译的文本（必传）
  // （2）lang： 需要被翻译成的语言（可选）默认值-意大利
  // -----------------------------------------------
  
  const app = new Vue({
	el: '#app',
	data: {
	  // words: ''
	  obj: {
		words: ''
	  },
	  result: '', // 翻译结果
	  // timer: null // 延时器id
	},
	watch: {
	  'obj.words' (newValue) {
		// console.log('变化了', newValue)
		// 防抖: 延迟执行 → 干啥事先等一等，延迟一会，一段时间内没有再次触发，才执行
		clearTimeout(this.timer)
		this.timer = setTimeout(async () => {
		  const res = await axios({
			url: 'https://applet-base-api-t.itheima.net/api/translate',
			params: {
			  words: newValue
			}
		  })
		  this.result = res.data.data
		  console.log(res.data.data)
		}, 300)
	  }
	}
  })
</script>
```


### watch侦听器完整写法
完整写法 —>添加额外的配置项
1. deep:true 对复杂类型进行深度监听（对对象的每一个属性都进行监视）
2. immdiate:true 初始化 立刻执行一次

```js
data: {
  obj: {
    words: '苹果',
    lang: 'italy'
  },
},

watch: {// watch 完整写法
  对象: {
    deep: true, // 深度监视
    immdiate:true,//立即执行handler函数
    handler (newValue) {
      console.log(newValue)
    }
  }
}
```

**需求**
	![](assets/Pasted%20image%2020240128182012.png) 
- 当文本框输入的时候 右侧翻译内容要时时变化
- 当下拉框中的语言发生变化的时候 右侧翻译的内容依然要时时变化
- 如果文本框中有默认值的话要立即翻译

```js
const app = new Vue({
  el: '#app',
  data: {
    obj: {
      words: '小黑',
      lang: 'italy'
    },
    result: '', // 翻译结果
  },
  watch: {
    obj: {
      deep: true, // 深度监视
      immediate: true, // 立刻执行，一进入页面handler就立刻执行一次
      handler (newValue) {
        clearTimeout(this.timer)
        this.timer = setTimeout(async () => {
          const res = await axios({
            url: 'https://applet-base-api-t.itheima.net/api/translate',
            params: newValue
          })
          this.result = res.data.data
          console.log(res.data.data)
        }, 300)
      }
    } 
  }
})
```



## 综合案例-购物车案例
购物车案例
	![](assets/Pasted%20image%2020240128182942.png)  
**需求说明：**
1. 渲染功能
2. 删除功能
3. 修改个数
4. 全选反选
5. 统计 选中的 总价 和 总数量
6. 持久化到本地

**实现思路：**
1. 基本渲染： v-for遍历、:class动态绑定样式
2. 删除功能 ： v-on 绑定事件，获取当前行的id
3. 修改个数 ： v-on绑定事件，获取当前行的id，进行筛选出对应的项然后增加或减少
4. 全选反选
	1. 必须所有的小选框都选中，全选按钮才选中 → every
	2. 如果全选按钮选中，则所有小选框都选中
	3. 如果全选取消，则所有小选框都取消选中
	声明计算属性，判断数组中的每一个checked属性的值，看是否需要全部选
5. 统计 选中的 总价 和 总数量 ：通过计算属性来计算**选中的**总价和总数量
6. 持久化到本地： 在数据变化时都要更新下本地存储 watch

[15-综合案例-购物车.zip](assets/15-综合案例-购物车.zip)



# day03-生命周期+工程化开发(组件入门)

## 一、Vue生命周期
思考：什么时候可以发送初始化渲染请求？（越早越好），什么时候可以开始操作dom？（至少dom得渲染出来）
Vue生命周期：就是一个Vue实例从创建 到 销毁 的整个过程。
生命周期四个阶段：① 创建 ② 挂载 ③ 更新 ④ 销毁
	![](assets/Pasted%20image%2020240130142508.png) 
1. 创建阶段：创建响应式数据
2. 挂载阶段：渲染模板
3. 更新阶段：修改数据，更新视图
4. 销毁阶段：销毁Vue实例

### Vue生命周期函数（钩子函数）
Vue生命周期过程中，会**自动运行一些函数**，被称为【**生命周期钩子**】→ 让开发者可以在【**特定阶段**】运行**自己的代码**
	![](assets/Pasted%20image%2020240130143300.png) 

```html
  <div id="app">
    <h3>{{ title }}</h3>
    <div>
      <button @click="count--">-</button>
      <span>{{ count }}</span>
      <button @click="count++">+</button>
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        count: 100,
        title: '计数器'
      },
      // 1. 创建阶段（准备数据）
      beforeCreate () {
        console.log('beforeCreate 响应式数据准备好之前', this.count)
      },
      created () {
        console.log('created 响应式数据准备好之后', this.count)
        // this.数据名 = 请求回来的数据
        // 可以开始发送初始化渲染的请求了
      },

      // 2. 挂载阶段（渲染模板）
      beforeMount () {
        console.log('beforeMount 模板渲染之前', document.querySelector('h3').innerHTML)
      },
      mounted () {
        console.log('mounted 模板渲染之后', document.querySelector('h3').innerHTML)
        // 可以开始操作dom了
      },

      // 3. 更新阶段(修改数据 → 更新视图)
      beforeUpdate () {
        console.log('beforeUpdate 数据修改了，视图还没更新', document.querySelector('span').innerHTML)
      },
      updated () {
        console.log('updated 数据修改了，视图已经更新', document.querySelector('span').innerHTML)
      },

      // 4. 卸载阶段
      beforeDestroy () {
        console.log('beforeDestroy, 卸载前')
        console.log('清除掉一些Vue以外的资源占用，定时器，延时器...')
      },
      destroyed () {
        console.log('destroyed，卸载后')
      }
    })
  </script>
```

`app.$destroy()` 卸载当前实例

### 生命周期钩子小案例
#### 在created中请求数据
等响应式数据创建完成后才能才能保存请求获取的数据
```html
  <div id="app">
    <ul>
      <li v-for="(item, index) in list" :key="item.id" class="news">
        <div class="left">
          <div class="title">{{ item.title }}</div>
          <div class="info">
            <span>{{ item.source }}</span>
            <span>{{ item.time }}</span>
          </div>
        </div>
        <div class="right">
          <img :src="item.img" alt="">
        </div>
      </li>
    </ul>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <script>
    // 接口地址：http://hmajax.itheima.net/api/news
    // 请求方式：get
    const app = new Vue({
      el: '#app',
      data: {
        list: []
      },
      async created () {
        // 1. 发送请求获取数据
        const res = await axios.get('http://hmajax.itheima.net/api/news')
        // 2. 更新到 list 中，用于页面渲染 v-for
        this.list = res.data.data
      }
    })
  </script>
```

#### 在mounted中获取焦点
在进入页面后将焦点定位到搜索框
等页面渲染完成后才能操作dom获取焦点
```html
<div class="container" id="app">
  <div class="search-container">
    <img src="https://www.itheima.com/images/logo.png" alt="">
    <div class="search-box">
      <input type="text" v-model="words" id="inp">
      <button>搜索一下</button>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      words: ''
    },
    // 核心思路：
    // 1. 等input框渲染出来 mounted 钩子
    // 2. 让input框获取焦点 inp.focus()
    mounted () {
      document.querySelector('#inp').focus()
    }
  })
</script>
```


## 综合案例：小黑记账清单
**需求图示：**
	![](assets/Pasted%20image%2020240130154428.png) 
**需求分析：**
1. 基本渲染
2. 添加功能
3. 删除功能
4. 饼图渲染

**思路分析：**
1.基本渲染
- 立刻发送请求获取数据 created
- 拿到数据，存到data的响应式数据中
- 结合数据，进行渲染 v-for
- 消费统计 —> 计算属性

2.添加功能
- 收集表单数据 v-model，使用指令修饰符处理数据
- 给添加按钮注册点击事件，对输入的内容做非空判断，发送请求
- 请求成功后，对文本框内容进行清空
- 重新渲染列表

3.删除功能
- 注册点击事件，获取当前行的id
- 根据id发送删除请求
- 需要重新渲染

4.饼图渲染
- 初始化一个饼图 echarts.init(dom) mounted钩子中渲染
- 根据数据试试更新饼图 echarts.setOptions({...})

[04-小黑记账清单.zip](assets/04-小黑记账清单.zip)


## 二、工程化开发入门
### 工程化开发和脚手架
- 核心包传统开发模式：基于html / css / js 文件，直接引入核心包，开发 Vue。
- **工程化开发模式：基于构建工具（例如：webpack）的环境中开发Vue。**

![](assets/Pasted%20image%2020240131110400.png)

**工程化开发模式优点：**
提高编码效率，比如使用JS新语法、Less/Sass、Typescript等通过webpack都可以编译成浏览器识别的**ES3/ES5/CSS等**
工程化开发模式问题：
- webpack配置**不简单**
- **雷同**的基础配置
- 缺乏**统一的标准**

为了解决以上问题，所以我们需要一个工具，生成标准化的配置

#### 脚手架Vue CLI
Vue CLI 是Vue官方提供的一个**全局命令工具**
可以帮助我们**快速创建**一个开发Vue项目的**标准化基础架子**。【集成了webpack配置】
**好处**
1. 开箱即用，零配置
2. 内置babel等工具
3. 标准化的webpack配置

#### 使用步骤：
1. 全局安装（只需安装一次即可） yarn global add @vue/cli 或者 npm i @vue/cli -g
2. 查看vue/cli版本： vue --version
3. 创建项目架子：**vue create project-name**(项目名不能使用中文)
4. 启动项目：**yarn serve** 或者 **npm run serve**(命令不固定，找package.json)

### 项目目录介绍和运行流程
**项目目录介绍**
	![](assets/Pasted%20image%2020240131174634.png) 
虽然脚手架中的文件有很多，目前只需认识三个文件即可
1. main.js 入口文件
2. App.vue App根组件
3. index.html 模板文件

**运行流程**
	![](assets/Pasted%20image%2020240131175007.png) 


### 组件化开发
**组件化：** 一个页面可以拆分成一个个组件，每个组件有着自己独立的结构、样式、行为。
**好处：** 便于维护，利于复用 → 提升开发效率。
**组件分类：** 普通组件、根组件。
比如：下面这个页面，可以把所有的代码都写在一个页面中，但是这样显得代码比较混乱，难易维护。咱们可以按模块进行组件划分
	![](assets/Pasted%20image%2020240131185647.png) 

### 根组件 App.vue
**根组件：** 整个应用最上层的组件，包裹所有普通小组件
	![](assets/Pasted%20image%2020240131190120.png) 
#### 组件是由三部分构成
App.vue 文件（单文件组件）的三个组成部分
	![](assets/Pasted%20image%2020240131190508.png) 
- template：结构 （有且只能一个根元素）
- script: js逻辑
- style： 样式 (可支持less，需要装包)

**语法高亮插件：** vetur
**让组件支持less**
    （1） style标签，lang="less" 开启less功能
    （2） 装包: yarn add less less-loader -D 或者npm i less less-loader -D

### 普通组件的注册使用-局部注册
只能在注册的组件内使用
**步骤：**
1. 创建.vue文件（三个组成部分）
2. 在使用的组件内先导入再注册，最后使用

**使用方式：** 当成html标签使用即可 `<组件名></组件名>`
**注意：** 组件名规范 —> 大驼峰命名法， 如 HmHeader
**语法：**
	![](assets/Pasted%20image%2020240131193302.png) 
```html
<template>
  <div class="App">
    <!-- 头部组件 -->
    <HmHeader></HmHeader>
    <!-- 主体组件 -->
    <HmMain></HmMain>
    <!-- 底部组件 -->
    <HmFooter></HmFooter>

    <!-- 如果 HmFooter + tab 出不来 → 需要配置 vscode
         设置中搜索 trigger on tab → 勾上
    -->
  </div>
</template>

<script>
// 导入需要注册的组件
import 组件对象 from '.vue文件路径'
import HmHeader from './components/HmHeader.vue'
import HmMain from './components/HmMain.vue'
import HmFooter from './components/HmFooter.vue'
export default {
  components: { // 局部注册
    // '组件名': 组件对象，相同可以省略
    HmHeader: HmHeader,
    HmMain,
    HmFooter
  }
}
</script>

```

技巧：`<vue>`自动补全结构


### 普通组件的注册使用-全局注册
全局注册的组件，在项目的**任何组件**中都能使用
**步骤：**
1. 创建.vue组件（三个组成部分）
2. **main.js**中进行全局注册

**使用方式：** 当成html标签使用即可 `<组件名></组件名>`

```js
// 导入需要全局注册的组件
import HmButton from './components/HmButton'
// 进行全局注册 → 在所有的组件范围内都能直接使用
// Vue.component('组件名', 组件对象)
Vue.component('HmButton', HmButton)
```

一般都用**局部注册**，如果确实发现是**通用组件**，再抽离到全局



## 综合案例：小兔鲜首页
小兔仙组件拆分示意图
	![](assets/Pasted%20image%2020240201080954.png) 

**开发思路**
1. 分析页面，按模块拆分组件，搭架子 (局部或全局注册)
2. 根据设计图，编写组件 html 结构 css 样式 (已准备好)
3. 拆分封装通用小组件 (局部或全局注册)
    将来 → 通过 js 动态渲染，实现功能

[综合案例：小兔鲜首页.zip](assets/综合案例：小兔鲜首页.zip)

技巧：
	所有都折叠 ctrl + k , ctrl + 0 
	所有都展开 ctrl + k , ctrl + J 



# day04-组件通信&进阶用法

## 一、组件的三大组成部分（结构/样式/逻辑）
### scoped解决样式冲突
写在组件中的样式会 **全局生效** → 因此很容易造成多个组件之间的样式冲突问题。
1. **全局样式**: 默认组件中的样式会作用到全局，任何一个组件中都会受到此样式的影响
2. **局部样式**: 可以给组件加上**scoped** 属性,可以**让样式只作用于当前组件**

```html
<template>
  <div class="base-one">
    BaseOne
  </div>
</template>

<script>
export default {

}
</script>
<style scoped>
</style>
```
组件都应该有独立的样式，推荐加scoped

**scoped原理**
1. 当前组件内标签都被添加**data-v-hash值** 的属性
2. css选择器都被添加 `[data-v-hash值]` 的属性选择器

最终效果: **必须是当前组件的元素**, 才会有这个自定义属性, 才会被这个样式作用到
	![](assets/Pasted%20image%2020240201114126.png) 

### data必须是一个函数
一个组件的 **data** 选项必须**是一个函数**。目的是为了：保证每个组件实例，维护**独立**的一份**数据**对象。
每次创建新的组件实例，都会新**执行一次data 函数**，得到一个新对象。
	![](assets/Pasted%20image%2020240201114553.png) 


## 二、组件通信
组件通信，就是指**组件与组件**之间的**数据传递**
- 组件的数据是独立的，无法直接访问其他组件的数据。
- 想使用其他组件的数据，就需要组件通信

**组件关系分类**
	![](assets/Pasted%20image%2020240201121710.png) 
1. 父子关系
2. 非父子关系

**通信解决方案**
	![](assets/Pasted%20image%2020240201121750.png) 

### 父子通信
流程
	![](assets/Pasted%20image%2020240201122032.png) 
1. 父组件通过 **props** 将数据传递给子组件
2. 子组件利用 **$emit** 通知父组件修改更新

#### 父组件通过**props**将数据传递给子组件
父向子传值步骤
	![](assets/Pasted%20image%2020240201123909.png) 
1. 给子组件以添加属性的方式传值
2. 子组件内部通过props接收
3. 模板中直接使用 props接收的值

#### 子组件利用 **$emit** 通知父组件，进行修改更新
子向父传值步骤
	![](assets/Pasted%20image%2020240201124741.png) 
1. $emit触发事件，给父组件发送消息通知
2. 父组件监听$emit触发的事件
3. 提供处理函数，在函数的性参中获取传过来的参数

### props 详解
**Props 定义：** 组件上注册的一些 自定义属性
**Props 作用：** 向子组件传递数据
**特点** 
1. 可以 传递 **任意数量** 的prop
2. 可以 传递 **任意类型** 的prop
![](assets/Pasted%20image%2020240201125753.png) 

#### props 校验
**作用：** 为组件的 prop 指定**验证要求**，不符合要求，控制台就会有**错误提示** → 帮助开发者，快速发现错误
**语法：**
	 ![](assets/Pasted%20image%2020240201130139.png) 
	 ![](assets/Pasted%20image%2020240201132145.png) 
- **类型校验**
- 非空校验
- 默认值
- 自定义校验

```js
export default {
  // 1.基础写法（类型校验）
  // props: {
  //   w: Number,
  // },

  // 2.完整写法（类型、非空、默认值、自定义校验）
  props: {
    w: {
      type: Number,
      required: true,
      default: 0,
      validator(val) {
        // console.log(val)
        if (val >= 100 || val <= 0) {
          console.error('传入的范围必须是0-100之间')
          return false
        } else {
          return true
        }
      },
    },
  },
}
```
**注意：**
1. default和required一般不同时写（因为当时必填项时，肯定是有值的）
2. default后面如果是简单类型的值，可以直接写默认。如果是复杂类型的值，则需要以函数的形式return一个默认值


#### props和data，单向数据流
**共同点：** 都可以给组件提供数据
**区别：**
- data 的数据是**自己**的 → 随便改
- prop 的数据是**外部**的 → 不能直接改，要遵循 **单向数据流**

**单向数据流：**
父级props 的数据更新，会向下流动，影响子组件。这个数据流动是单向的
	![](assets/Pasted%20image%2020240201122032.png) 

### 非父子通信
#### event bus 事件总线
非父子组件之间，进行简易消息传递。
1.创建一个都能访问的事件总线 （就是在一个js文件里创建空Vue实例），EventBus.js
   ```js
   import Vue from 'vue'
   const Bus = new Vue()
   export default Bus
   ```
2.A组件（接受方），监听Bus的 $on事件
   ```js
   import Bus from '../utils/EventBus'
   created () {
     Bus.$on('sendMsg', (msg) => {
       this.msg = msg
     })
   }
   ```
3.B组件（发送方），触发Bus的$emit事件
   ```js
   import Bus from '../utils/EventBus'
   Bus.$emit('sendMsg', '这是一个消息')
   ```
![](assets/Pasted%20image%2020240201171201.png) 


#### provide&inject
跨层级共享数据
	![](assets/Pasted%20image%2020240201172438.png) 

**父组件 provide提供数据**
```js
export default {
  provide() {
    return {
      // 简单类型 是非响应式的
      color: this.color,
      // 复杂类型 是响应式的
      userInfo: this.userInfo,
    }
  },
  data() {
    return {
      color: 'pink',
      userInfo: {
        name: 'zs',
        age: 18,
      },
    }
  }
}
```

**子/孙组件 inject获取数据**
```js
export default {
  inject: ['color','userInfo'],
  created () {
    console.log(this.color, this.userInfo)
  }
}
```

**注意**
- provide提供的简单类型的数据不是响应式的，复杂类型数据是响应式。（推荐提供复杂类型数据）
- 子/孙组件通过inject获取的数据，不能在自身组件内修改


## 综合案例：小黑记事本（组件版）
[09-小黑记事本.zip](assets/09-小黑记事本.zip)

### 组件拆分
**需求说明：**
- 拆分基础组件
- 渲染待办任务
- 添加任务
- 删除任务
- 底部合计 和 清空功能
- 持久化存储

可以把小黑记事本原有的结构拆成三部分内容：头部（TodoHeader）、列表(TodoMain)、底部(TodoFooter)
	![](assets/Pasted%20image%2020240201142659.png) 

### 列表渲染
思路分析：
1. 提供数据：提供在公共的父组件 App.vue
2. 通过父传子，将数据传递给TodoMain
3. 利用v-for进行渲染

### 添加功能
思路分析：
1. 收集表单数据 v-model
2. 监听时间 （回车+点击 都要进行添加）
3. 子传父，将任务名称传递给父组件App.vue
4. 父组件接受到数据后 进行添加 **unshift**(自己的数据自己负责)

### 删除功能
思路分析：
1. 监听时间（监听删除的点击）携带id
2. 子传父，将删除的id传递给父组件App.vue
3. 进行删除 **filter** (自己的数据自己负责)

### 底部功能及持久化存储
思路分析：
1. 底部合计：父组件传递list到底部组件 —>展示合计
2. 清空功能：监听事件 —> **子组件**通知父组件 —>父组件清空
3. 持久化存储：watch监听数据变化，持久化到本地

## 三、进阶语法
### v-model原理
v-model本质上是一个语法糖。例如应用在输入框上，就是value属性 和 input事件 的合写
```html
<template>
  <div id="app" >
    <input v-model="msg" type="text">

    <input :value="msg" @input="msg = $event.target.value" type="text">
  </div>
</template>
```
**$event** 用于在模板中，获取事件的形参

不同的表单元素， v-model在底层的处理机制是不一样的。比如给checkbox使用v-model
底层处理的是 checked属性和change事件。
**不过咱们只需要掌握应用在文本框上的原理即可**

#### 表单类组件封装
实现子组件和父组件数据的双向绑定 （实现App.vue中的selectId和子组件选中的数据进行双向绑定）
	![](assets/Pasted%20image%2020240202082707.png) 

#### v-model简化代码
v-model其实就是 :value和@input事件的简写
- 子组件：props通过value接收数据，事件触发 input
- 父组件：v-model直接绑定数据
	![](assets/Pasted%20image%2020240202083304.png) 

### .sync修饰符
**作用：** 可以实现 **子组件** 与 **父组件数据** 的 **双向绑定**，简化代码
简单理解：**子组件可以修改父组件传过来的props值**
**特点：** prop属性名，可以自定义，非固定为 value 
**场景：** 封装弹框类的基础组件， visible属性 true显示 false隐藏 
**本质：** 就是 :属性名 和 @update:属性名 合写
![](assets/Pasted%20image%2020240202090942.png)

### ref和$refs
利用ref 和 $refs 可以用于 获取 dom 元素 或 组件实例
![](assets/Pasted%20image%2020240202092958.png)
![](assets/Pasted%20image%2020240202094903.png)


### 异步更新 & $nextTick
**Vue 是异步更新DOM**
![](assets/Pasted%20image%2020240202100846.png)
```html
<template>
  <div class="app">
    <div v-if="isShowEdit">
      <input type="text" v-model="editValue" ref="inp" />
      <button>确认</button>
    </div>
    <div v-else>
      <span>{{ title }}</span>
      <button @click="editFn">编辑</button>
    </div>
  </div>
</template>
```

$nextTick：**等 DOM更新后**,才会触发执行此方法里的函数体
![](assets/Pasted%20image%2020240202100928.png)

**注意：**$nextTick 内的函数体 一定是**箭头函数**，这样才能让函数内部的this指向Vue实例
> 箭头函数没有自己的this，在箭头函数里使用的this是在定义时被闭包的



# day05-自定义指令&插槽&路由入门
## 一、自定义指令
- 内置指令：**v-html、v-if、v-bind、v-on**... 这都是Vue给咱们内置的一些指令，可以直接使用
- 自定义指令：同时Vue也支持让开发者，自己注册一些指令。这些指令被称为**自定义指令**
	通常**封装一些DOM操作**，扩展额外的功能

### 自定义指令语法
- 全局注册
```js
//在main.js中
Vue.directive('指令名', {
  "inserted" (el) {  // inserted 会在 指令所在的元素，被插入到页面中时触发
    // 可以对 el 标签，扩展额外功能
    el.focus()
  }
})
```
- 局部注册
```html
<script>
export default {
  //在Vue组件的配置项中
  directives: {
    "指令名": {
      inserted () {
        // 可以对 el 标签，扩展额外功能
        el.focus()
      }
    }
  }
}
</script>
```
- 使用指令
  注意：在使用指令的时候，一定要**先注册**，**再使用**，否则会报错
  使用指令语法： v-指令名。如：`<input type="text" v-focus/>`  
  **注册**指令时**不用**加**v-前缀**，但**使用时**一定要**加v-前缀**

**inserted：** 被绑定元素插入父节点时调用的钩子函数
**el：** 使用指令的那个DOM元素


### 指令的值
![](assets/Pasted%20image%2020240203120109.png)

1、在绑定指令时，可以通过“等号”的形式为指令 绑定 具体的参数值
`<div v-color="color">我是内容</div>`
2、通过 binding.value 可以拿到指令值，**指令值修改会 触发 update 函数**
```js
directives: {
  color: {
    // 1. inserted 提供的是元素被添加到页面中时的逻辑
    inserted (el, binding) {
      // console.log(el, binding.value);
      // binding.value 就是指令的值
      el.style.color = binding.value
    },
    // 2. update 指令的值修改的时候触发，提供值变化后，dom更新的逻辑
    update (el, binding) {
      console.log('指令的值修改了');
      el.style.color = binding.value
    }
  }
}
```

① v-指令名 = "指令值" ，通过 等号可以绑定指令的值
② 通过 binding.value 可以拿到指令的值
③ 通过 update 钩子，可以监听指令值的变化，进行dom更新操作


### v-loading指令的封装
**场景：** 实际开发过程中，发送请求需要时间，在请求的数据未回来时，页面会处于空白状态=> 用户体验不好
**需求：** 封装一个 v-loading 指令，实现加载中的效果

**分析**
1. 本质 loading效果就是一个蒙层，盖在了盒子上
2. 数据请求中，开启loading状态，添加蒙层
3. 数据请求完毕，关闭loading状态，移除蒙层

**实现**
1. 准备一个 loading类，通过伪元素定位，设置宽高，实现蒙层
2. 开启关闭 loading状态（添加移除蒙层），本质只需要添加移除类即可
3. 结合自定义指令的语法进行封装复用
```css
.loading:before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: #fff url("./loading.gif") no-repeat center;
}
```
```js
// 安装axios =>  yarn add axios
import axios from 'axios'

// 接口地址：http://hmajax.itheima.net/api/news
// 请求方式：get
export default {
  data () {
    return {
      list: [],
      isLoading: true,
      isLoading2: true
    }
  },
  async created () {
    // 1. 发送请求获取数据
    const res = await axios.get('http://hmajax.itheima.net/api/news')
    
    setTimeout(() => {
      // 2. 更新到 list 中，用于页面渲染 v-for
      this.list = res.data.data
      this.isLoading = false
    }, 2000)
  },
  directives: {
    loading: {
      inserted (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      },
      update (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      }
    }
  }
}
```
inserted 钩子中，binding.value 判断指令的值，设置默认状态
update 钩子中，binding.value 判断指令的值，更新类名状态


## 二、插槽
### 默认插槽
**作用：** 让组件内部的一些 结构 支持 自定义 
**需求：** 将需要多次显示的对话框, 封装成一个组件 
**问题：** 组件的内容部分，不希望写死，希望能使用的时候自定义。怎么办？
	![](assets/Pasted%20image%2020240203125237.png) 
#### 插槽的基本语法
1. 组件内需要定制的结构部分，改用`<slot></slot>`占位
2. 使用组件时, `<MyDialog></MyDialog>`标签内部, 传入结构替换slot
3. 给插槽传入内容时，可以传入**纯文本、html标签、组件**
	![](assets/Pasted%20image%2020240203125149.png) 


### 后备内容（默认值）
> 通过插槽完成了内容的定制，传什么显示什么, 但是如果不传，则是空白 
> 能否给插槽设置 默认显示内容 呢？

**插槽后备内容：** 封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）。
	![](assets/Pasted%20image%2020240203125928.png) 


### 具名插槽
需求：一个组件内有多处结构，需要外部传入标签，进行定制
**语法：** 多个slot使用name属性区分名字，template配合v-slot:名字来分发对应标签
	![](assets/Pasted%20image%2020240203130540.png) 
 一旦插槽起了名字，就是具名插槽，只支持定向分发
**v-slot的简写**，**v-slot —> #**
	![](assets/Pasted%20image%2020240203130639.png) 


### 作用域插槽
> 插槽只有两种，作用域插槽不属于插槽的一种分类，只是插槽的一个传参语法

定义slot 插槽的同时, 是可以**传值**的。给 **插槽** 上可以 **绑定数据**，将来 **使用组件时可以用**

场景：
	![](assets/Pasted%20image%2020240203132336.png) 

基本使用步骤：
	![](assets/Pasted%20image%2020240203132950.png) 

1. 给 slot 标签, 以 添加属性的方式传值
```html
<div v-for="(item, index) in data" :key="item.id">
  <slot :id="item.id" msg="测试文本"></slot>
</div>
```

2. 所有添加的属性, 都会被收集到一个对象中
	`{ id: 3, msg: '测试文本' }`

3. 在template中, 通过 ` #插槽名= "obj" ` 接收，默认插槽名为 default
```html
<MyTable :list="list">
  <template #default="obj">
    <button @click="del(obj.id)">删除</button>
  </template>
</MyTable>
```


## 综合案例：商品列表
[08-商品案例-组件封装.zip](assets/08-商品案例-组件封装.zip)
需求
	![](assets/Pasted%20image%2020240203143208.png) 

1. **my-tag 标签组件封装**
(1) 双击显示输入框，输入框获取焦点
(2) 失去焦点，隐藏输入框
(3) 回显标签信息
(4) 内容修改，回车 → 修改标签信息

2. **my-table 表格组件封装**
(1) 动态传递表格数据渲染
(2) 表头支持用户自定义
(3) 主体支持用户自定义


## 三、路由入门

### 单页应用程序
![](assets/Pasted%20image%2020240203185932.png)
![](assets/Pasted%20image%2020240203190100.png)


### 路由概念
单页面应用程序，之所以开发效率高，性能好，用户体验好
最大的原因就是：**页面按需更新**
要按需更新，首先就需要明确：**访问路径**和 **组件**的对应关系！**路由**

**Vue中的路由**：**路径和组件**的**映射**关系
	![](assets/Pasted%20image%2020240203191030.png) 

### VueRouter 的基本使用(5 + 2)
作用：修改地址栏路径时，切换显示匹配的组件 
说明：Vue 官方的一个路由插件，是一个第三方包 
官网：https://v3.router.vuejs.org/zh/

注意：
	Vue2 VueRouter3.x Vuex3.x
	Vue3 VueRouter4.x Vuex4.x

**固定5个固定的步骤**（不用死背，熟能生巧）
	![](assets/Pasted%20image%2020240203192103.png) 
```js
// 路由的使用步骤 5 + 2
// 5个基础步骤
// 1. 下载 v3.6.5
// yarn add vue-router@3.6.5
// 2. 引入
// 3. 安装注册 Vue.use(Vue插件)
// 4. 创建路由对象
// 5. 注入到new Vue中，建立关联

import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件初始化

const router = new VueRouter()

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

当我们配置完以上5步之后 就可以看到浏览器地址栏中的路由 变成了 /#/的形式。表示项目的路由已经被Vue-Router管理了

**2 个核心步骤**
	![](assets/Pasted%20image%2020240203192705.png) 

1. 创建需要的组件 (views目录)，配置路由规则
main.js
```js
import Find from './views/Find.vue'
import My from './views/My.vue'
import Friend from './views/Friend.vue'
const router = new VueRouter({
  routes: [
    { path: '/find', component: Find },
    { path: '/my', component: My },
    { path: '/friend', component: Friend },
  ]
})
```
2. 配置导航，配置路由出口(路径匹配的组件显示的位置)
App.vue
```html
<div class="footer_wrap">
  <a href="#/find">发现音乐</a>
  <a href="#/my">我的音乐</a>
  <a href="#/friend">朋友</a>
</div>
<div class="top">
  <router-view></router-view>
</div>
```


### 组件目录存放问题
注意： **.vue文件** 本质无区别
#### 组件分类
.vue文件分为2类，都是 **.vue文件（本质无区别）**
- 页面组件 （配置路由规则时使用的组件）
- 复用组件（多个组件中都使用到的组件）
	![](assets/Pasted%20image%2020240203200736.png) 

#### 存放目录
分类开来的目的就是为了 **更易维护**
1. src/views文件夹
    页面组件 - 页面展示 - 配合路由用
2. src/components文件夹
    复用组件 - 展示数据 - 常用于复用


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



# day07-Vuex的基本使用
## 一、vuex概述
vuex 是什么，应用场景，优势
	![](assets/Pasted%20image%2020240205120336.png) 
Vuex 是一个 Vue 的 状态管理工具
Vuex 是一个插件，可以帮我们管理 Vue 通用的数据 (多组件共享的数据)。例如：购物车数据 个人信息数据

官方原文：
- 不是所有的场景都适用于vuex，只有在必要的时候才使用vuex
- 使用了vuex之后，会附加更多的框架中的概念进来，增加了项目的复杂度 （数据的操作更便捷，数据的流动更清晰）
Vuex就像《近视眼镜》, 你自然会知道什么时候需要用它~


## 二、构建 vuex【多组件数据共享】环境
需求
	![](assets/Pasted%20image%2020240205120931.png) 

### 创建一个空仓库
![](assets/Pasted%20image%2020240205122032.png) 

创建仓库 `store/index.js`
```js
// 导入 vue
import Vue from 'vue'
// 导入 vuex
import Vuex from 'vuex'
// vuex也是vue的插件, 需要use一下, 进行插件的安装初始化
Vue.use(Vuex)

// 创建仓库 store
const store = new Vuex.Store()

// 导出仓库
export default store
```

main.js 中导入挂载到 Vue 实例上
```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```
此刻起, 就成功创建了一个 **空仓库!!**

测试打印Vuex
App.vue
```js
created(){
  console.log(this.$store)
}
```


## 三、核心概念 - state 状态
**目标**
明确如何给仓库 提供 数据，如何 使用 仓库的数据

#### 提供数据
State提供唯一的公共数据源，所有共享的数据都要统一放到Store中的State中存储。
打开项目中的store.js文件，在state对象中可以添加我们要共享的数据。
```js
// 创建仓库 store
const store = new Vuex.Store({
  // state 状态, 即数据, 类似于vue组件中的data,
  // 区别：
  // 1.data 是组件自己的数据, 
  // 2.state 中的数据整个vue项目的组件都能访问到
  state: {
    count: 101
  }
})
```

#### 访问Vuex中的数据
问题: 如何在组件中获取count?
1. 通过$store直接访问 —> {{ $store.state.count }}
2. 通过辅助函数mapState 映射计算属性 —> {{ count }}

**通过$store访问的语法**
```
获取 store：
 1.Vue模板中获取 this.$store
 2.js文件中获取 import 导入 store

模板中：     {{ $store.state.xxx }}
组件逻辑中：  this.$store.state.xxx
JS模块中：   store.state.xxx
```

可以将state属性定义在计算属性中 [https://vuex.vuejs.org/zh/guide/state.html](https://vuex.vuejs.org/zh/guide/state.html)
```js
// <h1>state的数据 - {{ count }}</h1>

// 把state中数据，定义在组件内的计算属性中
  computed: {
    count () {
      return this.$store.state.count
    }
  }
```

#### 通过mapState辅助函数 (简化)
mapState是辅助函数，帮助我们把store中的数据 **自动** 映射到 组件的计算属性中, 它属于一种方便的用法
	![](assets/Pasted%20image%2020240205124951.png) 
`mapState(['count']) ` 
上面代码的最终得到的是 **类似于**
`count () { return this.$store.state.count }`

```js
import { mapState } from 'vuex'
export default {
  computed: {
    ...mapState(['count, title'])
  }
}
```

## 四、核心概念 - mutations
vuex 同样遵循单向数据流，**组件中不能直接修改仓库的数据**（可以修改但是不符合规范）
	![](assets/Pasted%20image%2020240205130256.png) 
开发中建议开启严格模式strict: true （监测消耗性能），上线后不需要开启

#### mutations 修改 state数据
**state数据的修改只能通过 mutations**
	![](assets/Pasted%20image%2020240205131502.png) 
```js
// 定义 mutations 对象，对象中存放修改 state 的方法
const store = new Vuex.Store({
  state: {
    count: 0
  },
  // 定义mutations
  mutations: {
    // 方法里参数 第一个参数是当前store的state属性
    // payload 载荷 运输参数 调用mutaiions的时候 可以传递参数 传递载荷
    addCount (state) {
      state.count += 1
    }
  }
})

// 组件中提交调用 mutations
this.$store.commit('addCount')
```

#### mutations 传参语法
提交 mutation 是可以传递参数的 `this.$store.commit( 'xxx', 参数 )`
	![](assets/Pasted%20image%2020240205133806.png) 

#### Vuex中的值和组件中的input双向绑定
![](assets/Pasted%20image%2020240205145918.png)

App.vue
```js
// <input :value="count" @input="handleInput" type="text">

export default {
  methods: {
    handleInput (e) {
      // 1. 实时获取输入框的值
      const num = +e.target.value
      // 2. 提交mutation，调用mutation函数
      this.$store.commit('changeCount', num)
    }
  }
}
```

store/index.js
```js
mutations: { 
   changeCount (state, newCount) {
      state.count = newCount
   }
},
```

#### 辅助函数 - mapMutations
mapMutations 和 mapState很像，它是把位于mutations中的方法提取了出来，映射到组件methods中
	![](assets/Pasted%20image%2020240206112445.png) 

## 五、核心概念 - actions
![](assets/Pasted%20image%2020240206113451.png)
mutations 必须是同步的 (便于监测数据变化，记录调试)
```js
// 提供 action 方法
// 注意：不能直接操作 state，操作 state，还是需要 commit mutation
actions: {
  // context 上下文 (此处未分模块，可以当成store仓库)
  // context.commit('mutation名字', 额外参数)
  changeCountAction (context, num) {
    // 这里是setTimeout模拟异步，以后大部分场景是发请求
    setTimeout(() => {
      context.commit('changeCount', num)
    }, 1000)
  }
},

// 页面中 dispatch 调用
this.$store.dispatch('changeCountAction',200)
```

#### 辅助函数 - mapActions
![](assets/Pasted%20image%2020240206115147.png)
```js
methods: {
  // mapMutations 和 mapActions 都是映射方法
  // 全局级别的映射
  ...mapMutations(['subCount', 'changeTitle']),
  ...mapActions(['changeCountAction']),
}
```


## 六、核心概念 - getters
![](assets/Pasted%20image%2020240206121256.png)
```js
// getters 类似于计算属性
getters: {
  // 注意点：
  // 1. 形参第一个参数，就是state
  // 2. 必须有返回值，返回值就是getters的值
  filterList (state) {
    return state.list.filter(item => item > 5)
  }
},

// mapState 和 mapGetters 都是映射属性
computed: {
    ...mapState(['count', 'user', 'setting']),
    ...mapGetters(['filterList']),
},
```


## 七、核心概念 - 模块 module (进阶语法)
![](assets/Pasted%20image%2020240206123145.png) 

### **模块定义** - 准备 state
![](assets/Pasted%20image%2020240206123424.png)
store/modules/user.js
```js
const state = {
  userInfo: {
    name: 'zs',
    age: 18
  }
}
const mutations = {}
const actions = {}
const getters = {}
export default {
  state,
  mutations,
  actions,
  getters
}
```
在`store/index.js`文件中的modules配置项中，注册模块
```js
import user from './modules/user'
import setting from './modules/setting'

const store = new Vuex.Store({
    modules:{
        user,
        setting
    }
})
```

### 获取模块内的state数据
![](assets/Pasted%20image%2020240206130300.png)
1. 直接通过模块名访问 $store.state.模块名.xxx
2. 通过 mapState 映射：
    1. 默认根级别的映射 `mapState([ 'xxx' ])`
    2. 子模块的映射 ：`mapState('模块名', ['xxx'])` - 需要开启命名空间 **`namespaced:true`**

```html
<!-- 直接通过模块名访问 $store.state.模块名.xxx -->
<div>{{ $store.state.user.userInfo.name }}</div>
<div>{{ $store.state.setting.theme }}</div>

<!-- 默认根级别的映射 mapState([ 'xxx' ]) -->
<div>{{ user.userInfo.name }}</div>
<div>{{ setting.theme }}</div>

<!-- 子模块的映射 ：mapState('模块名', ['xxx']) -->
<div>{{ userInfo.name }}</div>
<div>{{ theme }}</div>
```
```js
computed: {
  ...mapState(['count', 'user', 'setting']),
  ...mapState('user', ['userInfo']),
  ...mapState('setting', ['theme', 'desc']),
},
```

### 获取模块内的getters数据
![](assets/Pasted%20image%2020240206132239.png)
```js
const getters = {
  // 分模块后，state指代子模块的state
  UpperCaseName (state) {
    return state.userInfo.name.toUpperCase()
  }
}
```

### 获取模块内的 mutations actions 方法
![](assets/Pasted%20image%2020240206133536.png)
![](assets/Pasted%20image%2020240206135555.png)
```js
methods: {
  // 分模块的映射
  ...mapMutations('setting', ['setTheme']),
  ...mapMutations('user', ['setUser']),
  ...mapActions('user', ['setUserSecond'])
}
```

```js
const mutations = {
  setUser (state, newUserInfo) {
    state.userInfo = newUserInfo
  }
}
const actions = {
  setUserSecond (context, newUserInfo) {
    // 将异步在action中进行封装
    setTimeout(() => {
      // 调用mutation   context上下文，默认提交的就是自己模块的action和mutation
      context.commit('setUser', newUserInfo)
    }, 1000)
  }
}
```


## 综合案例 - 购物车
[vue-cart-demo.zip](assets/vue-cart-demo.zip)
![](assets/Pasted%20image%2020240206141847.png)

### 构建vuex-cart模块
![](assets/Pasted%20image%2020240206144918.png)
state写为返回一个对象的函数，是官方推荐的写法，可以保证每个模块间数据的独立性

### 基于 json-server 工具，准备后端接口服务环境
![](assets/Pasted%20image%2020240206150736.png)
https://www.npmjs.com/package/json-server
```sh
yarn global add json-server 
npm i json-server  -g
```

### 请求获取数据存入 vuex, 映射渲染
![](assets/Pasted%20image%2020240206153650.png)

### 修改数量功能完成
![](assets/Pasted%20image%2020240206155646.png)

### 底部 getters 统计
![](assets/Pasted%20image%2020240206163054.png)

# day08-day10-智慧商城项目
[day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)
![day08-day10-智慧商城项目.pdf](assets/day08-day10-智慧商城项目.pdf)
[MD笔记-智慧商城项目.pdf](assets/MD笔记-智慧商城项目.pdf)
[hm-shopping.zip](assets/hm-shopping.zip)

**创建项目** ：基于 VueCli 自定义创建项目架子
**调整初始化目录** ：将目录调整成符合企业规范的目录
**vant 组件库** ：认识第三方 Vue组件库 vant-ui
**项目中的 vw 适配** ：基于 postcss 插件 实现项目 vw 适配
**路由设计配置** ：分析项目页面，设计路由，配置一级路由
	阅读vant组件库文档，实现底部导航 tabbar
**登录页静态布局** 
**request模块 - axios 封装** ：将 axios 请求方法，封装到 request 模块
**图形验证码功能完成** ：基于请求回来的 base64 图片，实现图形验证码功能
**api 接口模块 -封装图片验证码接口** ：将请求封装成方法，统一存放到 api 模块，与页面分离
**Toast 轻提示**
**短信验证倒计时**
**登录功能** ：封装api登录接口，实现登录功能
**响应拦截器统一处理错误提示** ：通过响应拦截器，统一处理接口的错误提示
**登录权证信息存储** ：vuex 构建 user 模块存储登录权证 (token & userId)
**storage存储模块 - vuex 持久化处理** ：封装 storage 存储模块，利用本地存储，进行 vuex 持久化处理

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

## 三、路由设计配置
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


## 四、登录页静态布局
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