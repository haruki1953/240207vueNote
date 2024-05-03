pnpm create vue 创建项目时，先择使用 TypeScript 语法即可

大致使用上与js相同，因为有类型推断。需要注意的是在使用特定API时为其标注类型： [搭配 TypeScript 使用 Vue](https://cn.vuejs.org/guide/typescript/overview.html) | [TypeScript 与组合式 API](https://cn.vuejs.org/guide/typescript/composition-api.html)


## 为组件的 props 标注类型
原来的参数声明可以用，且支持从它的参数中推导类型，这被称之为“运行时声明”
```vue
<script setup lang="ts">
const props = defineProps({
  foo: { type: String, required: true },
  bar: Number
})

props.foo // string
props.bar // number | undefined
</script>
```

另一种方法是通过泛型参数来定义 props 的类型，这被称之为“基于类型的声明”
```vue
<script setup lang="ts">
const props = defineProps<{
  foo: string
  bar?: number
}>()
</script>
```

基于类型的声明或者运行时声明可以择一使用，但是不能同时使用。
自己还是使用 运行时声明 算了，毕竟更简单

> 其中的泛型如果比较难理解的话，就把它当成一种参数，最终类型如何还是要看泛型函数里如何操作
> 这里vue的defineProps泛型函数里做了一些操作，声明了参数数据及其类型

Props 解构默认值： https://cn.vuejs.org/guide/typescript/composition-api.html#props-default-values

更复杂的 prop 类型： https://cn.vuejs.org/guide/typescript/composition-api.html#complex-prop-types


## 为组件的 emits 标注类型
https://cn.vuejs.org/guide/typescript/composition-api.html#typing-component-emits

在 `<script setup>` 中，`emit` 函数的类型标注也可以通过运行时声明或是类型声明进行：
```vue
<script setup lang="ts">
// 运行时
const emit = defineEmits(['change', 'update'])

// 基于选项
const emit = defineEmits({
  change: (id: number) => {
    // 返回 `true` 或 `false`
    // 表明验证通过或失败
  },
  update: (value: string) => {
    // 返回 `true` 或 `false`
    // 表明验证通过或失败
  }
})

// 基于类型
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()

// 3.3+: 可选的、更简洁的语法
const emit = defineEmits<{
  change: [id: number]
  update: [value: string]
}>()
</script>
```

我们可以看到，基于类型的声明使我们可以对所触发事件的类型进行更细粒度的控制。

现在创建的项目已经是"vue": "^3.4.21"了，就使用 更简洁的语法 吧（其中的语法是具名元组）

> 如果忘记了事件的触发的话可能有点难懂，复习一下：[【day11-Vue3入门】父子通信](../day11-Vue3入门.md#父子通信)
> emit触发事件就是要提供两个参数，一个是事件名，一个是事件所携带的值 `emit('changeMoney', 5)`
> 这样看来 emit基于类型的声明 就比较好懂了，就是在控制 事件名 与 事件所携带的值的类型


## 为 `ref()` 标注类型
ref 会根据初始化时的值推导其类型
```ts
import { ref } from 'vue'

// 推导出的类型：Ref<number>
const year = ref(2020)

// => TS Error: Type 'string' is not assignable to type 'number'.
year.value = '2020'
```

有时我们可能想为 ref 内的值指定一个更复杂的类型，可以通过使用 `Ref` 这个类型：
```ts
import { ref } from 'vue'
import type { Ref } from 'vue'

const year: Ref<string | number> = ref('2020')

year.value = 2020 // 成功！
```

或者，在调用 `ref()` 时传入一个泛型参数，来覆盖默认的推导行为：
```ts
// 得到的类型：Ref<string | number>
const year = ref<string | number>('2020')

year.value = 2020 // 成功！
```
需要的时候用这个吧

如果你指定了一个泛型参数但没有给出初始值，那么最后得到的就将是一个包含 `undefined` 的联合类型：
```ts
// 推导得到的类型：Ref<number | undefined>
const n = ref<number>()
```


## 为 `reactive()` 标注类型
这是是类似ref的提供响应式数据的，不常用差点忘了，好像也不推荐用
https://cn.vuejs.org/guide/typescript/composition-api.html#typing-reactive


## 为 `computed()` 标注类型

`computed()` 会自动从其计算函数的返回值上推导出类型：
```ts
import { ref, computed } from 'vue'

const count = ref(0)

// 推导得到的类型：ComputedRef<number>
const double = computed(() => count.value * 2)

// => TS Error: Property 'split' does not exist on type 'number'
const result = double.value.split('')
```

你还可以通过泛型参数显式指定类型：
```ts
const double = computed<number>(() => {
  // 若返回值不是 number 类型则会报错
})
```


## 为事件处理函数标注类型

在处理原生 DOM 事件时，应该为我们传递给事件处理函数的参数正确地标注类型。让我们看一下这个例子：
```vue
<script setup lang="ts">
function handleChange(event) {
  // `event` 隐式地标注为 `any` 类型
  console.log(event.target.value)
}
</script>

<template>
  <input type="text" @change="handleChange" />
</template>
```

没有类型标注时，这个 `event` 参数会隐式地标注为 `any` 类型。这也会在 `tsconfig.json` 中配置了 `"strict": true` 或 `"noImplicitAny": true` 时报出一个 TS 错误。因此，建议显式地为事件处理函数的参数标注类型。此外，你在访问 `event` 上的属性时可能需要使用类型断言：
```ts
function handleChange(event: Event) {
  console.log((event.target as HTMLInputElement).value)
}
```


## [为 provide / inject 标注类型](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-provide-inject)


## 为模板引用标注类型
板引用需要通过一个显式指定的泛型参数和一个初始值 `null` 来创建：
```vue
<script setup lang="ts">
import { ref, onMounted } from 'vue'

const el = ref<HTMLInputElement | null>(null)

onMounted(() => {
  el.value?.focus()
})
</script>

<template>
  <input ref="el" />
</template>
```


## 为组件模板引用标注类型
有时，你可能需要为一个子组件添加一个模板引用，以便调用它公开的方法。举例来说，我们有一个 `MyModal` 子组件，它有一个打开模态框的方法：
```vue
<!-- MyModal.vue -->
<script setup lang="ts">
import { ref } from 'vue'

const isContentShown = ref(false)
const open = () => (isContentShown.value = true)

defineExpose({
  open
})
</script>
```

为了获取 `MyModal` 的类型，我们首先需要通过 `typeof` 得到其类型，再使用 TypeScript 内置的 `InstanceType` 工具类型来获取其实例类型：
```vue
<!-- App.vue -->
<script setup lang="ts">
import MyModal from './MyModal.vue'

const modal = ref<InstanceType<typeof MyModal> | null>(null)

const openModal = () => {
  modal.value?.open()
}
</script>
```

如果组件的具体类型无法获得，或者你并不关心组件的具体类型，那么可以使用 `ComponentPublicInstance`。这只会包含所有组件都共享的属性，比如 `$el`。
```ts
import { ref } from 'vue'
import type { ComponentPublicInstance } from 'vue'

const child = ref<ComponentPublicInstance | null>(null)
```


# 在项目中的使用
对于项目中要使用的数据的类型，统一保存在 @/types/ 文件夹下，后缀名 .d.ts

## 自动按需引入组件后的类型声明
以自动按需引入vant为例 https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart
项目根目录下的 auto-imports.d.ts 与 components.d.ts 都是自动引入插件所生成的，其中包含了所使用的组件的类型信息。
需要手动将这两个类型声明文件，添加至 tsconfig.app.json 中的 include 数组内
```json
{
  // ...
  "include": [
    // ...
    "auto-imports.d.ts",
    "components.d.ts"
  ],
}
```

## 为组件模板引用标注类型
使用import type，并不会重复导入。因为使用了组件自动导入，尽量避免重复导入
对于vant4中的组件，应该导入 `组件名Instance`，否则会没有方法的类型，下面以Form表单组件为例
```ts
import type TestBox from '@/components/TestBox.vue'
import type { FormInstance } from 'vant'

// 自己封装的组件
const refTestBox = ref<InstanceType<typeof TestBox>>()
// vant4表单组件
const form = ref<FormInstance>()
```
> 其实不用像vue官方一样加上null的联合类型，直接不写的话就是加上undefined的联合类型


## 为封装的api方法标注类型
暂时还是觉得就在写方法的同时标注类型比较方便，怎么方便怎么来吧。
如果需要复用，就写在@/types/api.d.ts。如果数据类型在项目中也要使用，就在types文件夹新建文件来写

对于响应数据，方法返回值类型为 `Promise<AxiosResponse<any, any>>`，两个any分别是成功与失败的响应数据，一般只指定第一个就可以了
```ts
/* 在api.d.ts对其做封装 */
import type { AxiosResponse } from 'axios'
// api方法返回值类型的，dataType为返回数据的类型
export type ResData<DataType = undefined> = Promise<
  AxiosResponse<{
    code: number
    message: string
    data: DataType
  }>
>
// 对于登录接口，没有data属性，而是token
export type ResToken = Promise<
  AxiosResponse<{
    code: number
    message: string
    token: string
  }>
>

/* 使用示例 */
// api/auth.ts
import request from '@/utils/request'
import type { ResData, ResToken } from '@/types/api'

export const authRegisterService = (data: {
  username: string
  password: string
  email: string
}): ResData => request.post('/auth/register', data)

export const authLoginByUsernameService = (data: {
  username: string
  password: string
}): ResToken => request.post('/auth/login/username', data)

// api/public.ts
import request from '@/utils/request'
import type { ResData } from '@/types/api'
import type { UserRes } from '@/types/user'

export const publicGetUsersService = (): ResData<UserRes[]> =>
  request.get('/public/users')
```
