# 购物车功能实现
## 购物车业务逻辑梳理拆解
1. 整个购物车的实现分为俩个大分支，本地购物车操作和接口购物车操作 
2. 由于购物车数据的特殊性，采取Pinia管理购物车列表数据并添加持久化缓存
![](assets/Pasted%20image%2020240520182242.png)
[stores/cartStore.js](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/stores/cartStore.js)

## 本地购物车-加入购物车实现
![](assets/Pasted%20image%2020240520183036.png)

## 本地购物车 - 头部购物车列表渲染
![](assets/Pasted%20image%2020240520185722.png)

## 本地购物车 - 头部购物车删除实现

## 本地购物车 - 头部购物车统计计算
1. 商品总数计算逻辑：商品列表中的所有商品 count 累加之和 
2. 商品总价钱计算逻辑：商品列表中的所有商品的 count * price 累加之和
```js
// 计算属性
// 1. 总的数量 所有项的count之和
const allCount = computed(() => cartList.value.reduce((a, c) => a + c.count, 0))
// 2. 总价 所有项的count*price之和
const allPrice = computed(() => cartList.value.reduce((a, c) => a + c.count * c.price, 0))
```
[散装笔记【JavaScript数组reduce方法】](../笔记/散装笔记.md#JavaScript数组reduce方法)

## 本地购物车-列表购物车
![](assets/Pasted%20image%2020240521101213.png)
[views/CartList/index.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/CartList/index.vue)

### 单选功能
核心思路：单选的核心思路就是始终把单选框的状态和Pinia中store对应的状态保持同步
注意事项： v-model双向绑定指令不方便进行命令式的操作（因为后续还需要调用接口），所以把v-model回退到一般模式，也就是 :model-value 和 @change 的配合实现
![](assets/Pasted%20image%2020240521102106.png)

使用箭头函数，在默认参数基础上增加参数
```vue
<!-- 单选框 -->
<el-checkbox :model-value="i.selected" @change="(selected) => singleCheck(i, selected)" />
```

### 全选功能
核心思路： 
1. 操作单选决定全选：只有当cartList中的所有项都为true时，全选状态才为true 
2. 操作全选决定单选：cartList中的所有项的selected都要跟着一起变
![](assets/Pasted%20image%2020240521103345.png)

### 统计数据功能
```js
// 3. 已选择数量
const selectedCount = computed(() => cartList.value.filter(item => item.selected).reduce((a, c) => a + c.count, 0))
// 4. 已选择商品价钱合计
const selectedPrice = computed(() => cartList.value.filter(item => item.selected).reduce((a, c) => a + c.count * c.price, 0))
```

## 接口购物车-加入购物车、删除购物车
到目前为止，购物车在非登录状态下的各种操作都已经ok了，包括action的封装、触发、参数传递，剩下的事情就是在action中做登录状态的分支判断，补充登录状态下的接口操作逻辑即可
![](assets/Pasted%20image%2020240521104921.png)
```js
// 登录时调用接口
await insertCartAPI({ skuId, count })
updateNewList()
```

## 退出登录-清空购物车列表

## 合并本地购物车到服务器
在用户登录时，把本地的购物车数据和服务端购物车数据进行合并操作
实现步骤：
- 登录时调用合并购物车接口 
- 获取最新的购物车列表 
- 覆盖本地购物车列表


# 结算模块
## 路由配置和基础数据渲染

## 地址切换交互实现
![](assets/Pasted%20image%2020240521115240.png)
![](assets/Pasted%20image%2020240521115758.png)

## 生成订单功能实现
![](assets/Pasted%20image%2020240521120924.png)

# 支付模块
## 渲染基础数据
![](assets/Pasted%20image%2020240521123508.png)
[views/Pay/index.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Pay/index.vue)

## 实现支付功能
![](assets/Pasted%20image%2020240521124123.png)

## 支付结果展示
![](assets/Pasted%20image%2020240521125842.png)
[views/Pay/PayBack.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Pay/PayBack.vue)

## 封装倒计时函数
![](assets/Pasted%20image%2020240521143456.png)
![](assets/Pasted%20image%2020240521143607.png)
[composables/useCountDown.js](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/composables/useCountDown.js)
```js
// 封装倒计时逻辑函数
import { computed, onUnmounted, ref } from 'vue'
import dayjs from 'dayjs'
export const useCountDown = () => {
  // 1. 响应式的数据
  let timer = null
  const time = ref(0)
  // 格式化时间 为 xx分xx秒
  const formatTime = computed(() => dayjs.unix(time.value).format('mm分ss秒'))
  // 2. 开启倒计时的函数
  const start = (currentTime) => {
    // 开始倒计时的逻辑
    // 核心逻辑的编写：每隔1s就减一
    time.value = currentTime
    timer = setInterval(() => {
      time.value--
    }, 1000)
  }
  // 组件销毁时清除定时器
  onUnmounted(() => {
    timer && clearInterval(timer)
  })
  return {
    formatTime,
    start
  }
}
```
dayjs第三方日期格式化插件


# 会员中心
## 整体功能梳理和路由配置
![](assets/Pasted%20image%2020240521144846.png)
二级路由、三级路由

## 个人中心信息渲染
[views/Member/components/UserInfo.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Member/components/UserInfo.vue)

## 我的订单
[views/Member/components/UserOrder.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/views/Member/components/UserOrder.vue)
### 订单基础列表渲染
![](assets/Pasted%20image%2020240521160324.png)
### tab切换实现
![](assets/Pasted%20image%2020240521160337.png)
### 分页逻辑实现
![](assets/Pasted%20image%2020240521160621.png)

## 细节优化
**默认三级路由设置**
默认路由path置空，另一种方法是重定向


# SKU组件封装
SKU组件的作用是为了让用户能够选择商品的规格，从而提交购物车，在选择的过程中，组件的选中状态要进行更新，组件还要提示用户当前规格是否禁用，每次选择都要产出对应的Sku数据
![](assets/Pasted%20image%2020240521161811.png)
[components/XtxSku/index.vue](Vue3小兔鲜%20-%20配套资料/03-code/vue-rabbit/src/components/XtxSku/index.vue)

## 点击规格更新选中状态
核心思路：
1. 如果当前已经激活了，就取消激活
2. 如果当前未激活，就把和自己同排的其他规格取消激活，再把自己激活
响应式数据设计：
每一个规格项都添加一个selected字段来决定是否激活，true为激活，false为未激活
样式处理：
使用selected配合动态class属性，selected为true就显示对应激活类名

## 点击规格更新禁用状态
### 生成有效路径字典
![](assets/Pasted%20image%2020240521171155.png)
![](assets/Pasted%20image%2020240521171212.png)

### 初始化规格禁用
![](assets/Pasted%20image%2020240521171644.png)

### 点击时组合禁用更新
![](assets/Pasted%20image%2020240521195711.png)

### 产出有效的SKU信息
![](assets/Pasted%20image%2020240521200407.png)
![](assets/Pasted%20image%2020240521200422.png)
