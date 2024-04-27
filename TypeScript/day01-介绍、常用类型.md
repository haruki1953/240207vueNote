# 一、ts初体验

### 安装编译 TS 的工具包
pnpm add -g typescript

### 编译并运行 TS 代码
TS 编译为 JS：tsc hello.ts（此时，在同级目录中会出现一个同名的 JS 文件）
执行 JS 代码：node hello.js。

### 简化运行 TS 的步骤
使用 ts-node 包
pnpm add -g ts-node（ts-node 包提供了 ts-node 命令）
使用方式：ts-node hello.ts。 
解释：ts-node 命令在内部将 TS -> JS，然后，再运行 JS 代码


# 二、常用类型
```ts
// 类型注解
let age: number = 18
let myName: string = '刘老师'
let isLoading: boolean = false
let a: null = null
let b: undefined = undefined
let s: symbol = Symbol()

// 数组类型：
let numbers: number[] = [1, 3, 5]

// 联合类型：
// 添加小括号，表示：首先是数组，然后，这个数组中能够出现 number 或 string 类型的元素
let arr: (number | string)[] = [1, 3, 5, 'a', 'b']
// 不添加小括号，表示：arr1 既可以是 number 类型，又可以是 string[]
let arr1: number | string[] = ['a', 'b']
let arr1: number | string[] = 123

// 类型别名：
type CustomArray = (number | string)[]
let arr: CustomArray = [1, 3, 5, 'a', 'b']
let arr1: CustomArray = [1, 'x', 2, 'y']

// 函数类型
// 1. 单独指定参数、返回值类型：
function add(num1: number, num2: number): number {
  return num1 + num2
}
const add = (num1: number, num2: number): number => {
  return num1 + num2
}
// 2. 同时指定参数、返回值类型：
const add: (num1: number, num2: number) => number = (num1, num2) => {
  return num1 + num2
}
// void
function greet(name: string): void {
  console.log('Hello', name)
}
// 可选参数
function mySlice(start: number, end?: number): void {
  console.log('起始索引：', start, '结束索引：', end)
}

// 对象类型
let person: {
  name: string
  age: number
  // sayHi(): void
  sayHi: () => void
  greet(name: string): void
} = {
  name: '刘老师',
  age: 18,
  sayHi() {},
  greet(name) {}
}

// 接口：
interface IPerson {
  name: string
  age: number
  sayHi(): void
}
let person: IPerson = {
  name: '刘老师',
  age: 18,
  sayHi() {}
}
let person1: IPerson = {
  name: 'jack',
  age: 16,
  sayHi() {}
}

// 接口&类型别名
// 接口：
interface IPerson {
  name: string
  age: number
  sayHi(): void
}
// 类型别名
type IPerson = {
  name: string
  age: number
  sayHi(): void
}
let person: IPerson = {
  name: '刘老师',
  age: 18,
  sayHi() {}
}

// 接口继承
interface Point2D {
  x: number
  y: number
}
interface Point3D extends Point2D {
  z: number
}
let p3: Point3D = {
  x: 1,
  y: 0,
  z: 0
}

// 元组 元组类型可以确切地标记出有多少个元素，以及每个元素的类型
let position: [number, string] = [39, '114']
```

## 类型推论
由于类型推论的存在，这些地方，类型注解可以省略不写
发生类型推论的 2 种常见场景：1 声明变量并初始化时 2 决定函数返回值时。

## 类型断言
```ts
const aLink = document.getElementById('link') as HTMLAnchorElement
// const aLink = <HTMLAnchorElement>document.getElementById('link')
```

> console.log($0)，可以打印当前选中元素，现在才知道🤡