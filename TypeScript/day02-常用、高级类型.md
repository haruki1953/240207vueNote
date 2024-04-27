# 一、常用类型
## 字面量类型
```ts
let str1 = 'Hello TS'
const str2 = 'Hello TS'
```
1. str1 是一个变量（let），它的值可以是任意字符串，所以类型为：string。 
2. str2 是一个常量（const），它的值不能变化只能是 'Hello TS'，所以，它的类型为：'Hello TS'。 

注意：此处的 'Hello TS'，就是一个字面量类型。也就是说某个特定的字符串也可以作为 TS 中的类型。 除字符串外，任意的 JS 字面量（比如，对象、数字等）都可以作为类型使用。

使用模式：字面量类型配合联合类型一起使用。 
使用场景：用来表示一组明确的可选值列表。 
```ts
function changeDirection(direction: 'up' | 'down' | 'left' | 'right') {}
```

## 枚举类型
枚举的功能类似于字面量类型+联合类型组合的功能，也可以表示一组明确的可选值。
```ts
// 枚举：
enum Direction {
  Up,
  Down,
  Left,
  Right
}
function changeDirection(direction: Direction) {}
changeDirection(Direction.Left)
```

注意：枚举成员是有值的，默认为：从 0 开始自增的数值。
我们把，枚举成员的值为数字的枚举，称为：数字枚举。 
当然，也可以给枚举中的成员初始化值。
```ts
enum Direction {
  Up = 2,
  Down = 4,
  Left = 8,
  Right = 16
}
```

字符串枚举：枚举成员的值是字符串。
注意：字符串枚举没有自增长行为，因此，字符串枚举的每个成员必须有初始值
```ts
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT'
}
```

枚举是 TS 为数不多的非 JavaScript 类型级扩展（不仅仅是类型）的特性之一。 
因为：其他类型仅仅被当做类型，而枚举不仅用作类型，还提供值（枚举成员都是有值的）。 
也就是说，其他的类型会在编译为 JS 代码时自动移除。但是，枚举类型会被编译为 JS 代码！
![](assets/Pasted%20image%2020240427133029.png)

一般情况下，**推荐使用字面量类型+联合类型组合的方式**，因为相比枚举，这种方式更加直观、简洁、高效。

## any 类型
原则：不推荐使用 any！
当值的类型为 any 时，可以对该值进行任意操作，并且不会有代码提示。
尽可能的避免使用 any 类型，除非临时使用 any 来“避免”书写很长、很复杂的类型！ 
其他隐式具有 any 类型的情况：1 声明变量不提供类型也不提供默认值 2 函数参数不加类型。 
注意：因为不推荐使用 any，所以，这两种情况下都应该提供类型！

## typeof
TS 也提供了 typeof 操作符：可以在类型上下文中引用变量或属性的类型（类型查询）。 
使用场景：根据已有变量的值，获取该值的类型，来简化类型书写
```ts
let p = { x: 1, y: 2 }

// function formatPoint(point: { x: number; y: number }) {}
function formatPoint(point: typeof p) {}

formatPoint({ x: 1, y: 100 })
```


# 二、高级类型

## class 类
TS 中的 class，不仅提供了 class 的语法功能，也作为一种类型存在。
```ts
class Person {
  age: number
  gender = '男'
  // gender: string = '男'
}
const p = new Person()
```

```ts
// 构造函数
class Person {
  age: number
  gender: string

  constructor(age: number, gender: string) {
    this.age = age
    this.gender = gender
  }
}
const p = new Person(18, '男')

// 实例方法
class Point {
  x = 1
  y = 2
  scale(n: number): void {
    this.x *= n
    this.y *= n
  }
}

// 继承
class Animal {
  move() {
    console.log('走两步')
  }
}
class Dog extends Animal {
  name = '二哈'
  bark() {
    console.log('旺旺！')
  }
}

// 实现接口
interface Singale {
  sing(): void
  name: string
}
class Person implements Singale {
  name = 'jack'
  sing() {
    console.log('你是我的小呀小苹果')
  }
}

// 类成员可见性 
// 1 public（公有的） 2 protected（受保护的） 3 private（私有的）。

// readonly：表示只读，用来防止在构造函数之外对属性进行赋值。
class Person {
  // 注意：只要是 readonly 来修饰的属性，必须手动提供明确的类型
  // 如果不加，则 age 的类型为 18 （字面量类型）。
  readonly age: number = 18
  constructor(age: number) {
    this.age = age
  }
}

// 接口或者 {} 表示的对象类型，也可以使用 readonly
interface IPerson {
  readonly name: string
}
let obj: IPerson = {
  name: 'jack'
}
let obj: { readonly name: string } = {
  name: 'jack'
}

```


## 类型兼容性
两种类型系统：1 Structural Type System（结构化类型系统） 2 Nominal Type System（标明类型系统）。 
TS 采用的是结构化类型系统，也叫做 duck typing（鸭子类型），类型检查关注的是值所具有的形状。 也就是说，在结构类型系统中，如果两个对象具有相同的形状，则认为它们属于同一类型。
```ts
class Point {
  x: number
  y: number
}
class Point2D {
  x: number
  y: number
}
const p: Point = new Point2D()
```

注意：在结构化类型系统中，如果两个对象具有相同的形状，则认为它们属于同一类型，这种说法并不准确。 
更准确的说法：对于对象类型来说，y 的成员至少与 x 相同，则 x 兼容 y（成员多的可以赋值给少的）。
```ts
class Point {
  x: number
  y: number
}
class Point3D {
  x: number
  y: number
  z: number
}
const p1: Point = new Point3D()
```

### 接口兼容性
除了 class 之外，TS 中的其他类型也存在相互兼容的情况，包括：1 接口兼容性 2 函数兼容性 等。
接口之间的兼容性，类似于 class。并且，class 和 interface 之间也可以兼容。
```ts
interface Point {
  x: number
  y: number
}
interface Point2D {
  x: number
  y: number
}
interface Point3D {
  x: number
  y: number
  z: number
}

let p1: Point
let p2: Point2D
let p3: Point3D

// 正确：
// p1 = p2
// p2 = p1
// p1 = p3

// 错误演示：
// p3 = p1

// 类和接口之间也是兼容的
class Point4D {
  x: number
  y: number
  z: number
}
p2 = new Point4D()
```

### 函数兼容性
函数之间兼容性比较复杂，需要考虑：1 参数个数 2 参数类型 3 返回值类型。 
1. 参数个数，参数多的兼容参数少的（或者说，参数少的可以赋值给多的）。
```ts
// 1 参数个数： 参数少的可以赋值给参数多的
type F1 = (a: number) => void
type F2 = (a: number, b: number) => void

let f1: F1
let f2: F2

f2 = f1
```

在 JS 中省略用不到的函数参数实际上是很常见的，这样的使用方式，促成了 TS 中函数类型之间的兼容性。

2. 参数类型，相同位置的参数类型要相同（原始类型）或兼容（对象类型）。
```ts
// 2 参数类型： 相同位置的参数类型要相同或兼容

// 原始类型：
// type F1 = (a: number) => void
// type F2 = (a: number) => void

// let f1: F1
// let f2: F2

// f1 = f2
// f2 = f1

// --

// 对象类型
interface Point2D {
  x: number
  y: number
}
interface Point3D {
  x: number
  y: number
  z: number
}

type F2 = (p: Point2D) => void // 相当于有 2 个参数
type F3 = (p: Point3D) => void // 相当于有 3 个参数

let f2: F2
let f3: F3

f3 = f2

// f2 = f3
```

- 注意，此处与前面讲到的接口兼容性冲突。 
- 技巧：将对象拆开，把每个属性看做一个个参数，则，参数少的（f2）可以赋值给参数多的（f3）。

3. 返回值类型，只关注返回值类型本身即可
```ts
// 3 返回值类型，只需要关注返回值类型本身即可

// 原始类型：
type F5 = () => string
type F6 = () => string

let f5: F5
let f6: F6

f6 = f5
f5 = f6

// 对象类型：
type F7 = () => { name: string }
type F8 = () => { name: string; age: number }

let f7: F7
let f8: F8

f7 = f8

// 错误演示
// f8 = f7
```