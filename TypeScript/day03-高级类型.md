# 一、高级类型
## 交叉类型
交叉类型（&）：功能类似于接口继承（extends），用于组合多个类型为一个类型（常用于对象类型）。
```ts
interface Person {
  name: string
  say(): number
}
interface Contact {
  phone: string
}

type PersonDetail = Person & Contact

let obj: PersonDetail = {
  name: 'jack',
  phone: '133....',
  say() {
    return 1
  }
}
```

### 交叉类型vs继承
- 相同点：都可以实现对象类型的组合。 
- 不同点：两种方式实现类型组合时，对于同名属性之间，处理类型冲突的方式不同。
![](assets/Pasted%20image%2020240428102852.png)


## 泛型
泛型是可以在保证类型安全前提下，让函数等与多种类型一起工作，从而实现复用，常用于：函数、接口、class 中。

### 泛型函数
```ts
// 使用泛型来创建一个函数：
function id<Type>(value: Type): Type {
  return value
}

// 调用泛型函数：
// 1 以 number 类型调用泛型函数
const num = id<number>(10)
// 2 以 string 类型调用泛型函数
const str = id<string>('a')
const ret = id<boolean>(false)

// 简化调用泛型函数：在调用泛型函数时，可以省略 <类型> 来简化泛型函数的调用。
// 说明：当编译器无法推断类型或者推断的类型不准确时，就需要显式地传入类型参数。
// 比如以下就会被推断为字面量类型
let num1 = id(100)
let str1 = id('abc')
```

### 泛型约束
默认情况下，泛型函数的类型变量 Type 可以代表多个类型，这导致无法访问任何属性。 
解释：Type 可以代表任意类型，无法保证一定存在 length 属性，比如 number 类型就没有 length。 
此时，就需要为泛型添加约束来收缩类型（缩窄类型取值范围）。

1. 指定更加具体的类型 
```ts
// 比如，将类型修改为 Type[]（Type 类型的数组），因为只要是数组就一定存在 length 属性，因此就可以访问了。
function id<Type>(value: Type[]): Type[] {
  value.length
  return value
}
```

2. 添加约束。
```ts
interface ILength {
  length: number
}
function id<Type extends ILength>(value: Type): Type {
  value.length
  return value
}
// 注意：传入的实参（比如，数组）只要有 length 属性即可，这也符合前面讲到的接口的类型兼容性。
```

#### keyof
泛型的类型变量可以有多个，并且类型变量之间还可以约束
比如，创建一个函数来获取对象中属性的值：
```ts
function getProp<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key]
}
getProp({ name: 'jack', age: 18 }, 'age')
getProp({ name: 'jack', age: 18 }, 'name')

// 补充：（了解）
getProp(18, 'toFixed')
getProp('abc', 'split')
getProp('abc', 1) // 此处 1 表示索引
getProp(['a'], 'length')
getProp(['a'], 1000)
```

### 泛型接口
接口也可以配合泛型来使用，以增加其灵活性，增强其复用性。
```ts
interface IdFunc<Type> {
  id: (value: Type) => Type
  ids: () => Type[]
}

let obj: IdFunc<number> = {
  id(value) {
    return value
  },
  ids() {
    return [1, 3, 5]
  }
}
```
1. 在接口名称的后面添加 <类型变量>，那么，这个接口就变成了泛型接口。 
2. 接口的类型变量，对接口中所有其他成员可见，也就是接口中所有成员都可以使用类型变量。 
3. 使用泛型接口时，需要显式指定具体的类型（比如，此处的 IdFunc）。 
4. 此时，id 方法的参数和返回值类型都是 number；ids 方法的返回值类型是 `number[]`。

实际上，JS 中的数组在 TS 中就是一个泛型接口。

### 泛型类
```ts
class GenericNumber<NumType> {
  defaultValue: NumType
  add: (x: NumType, y: NumType) => NumType
  constructor(value: NumType) {
    this.defaultValue = value
  }
}
// 此时，可以省略 <类型> 不写。因为 TS 可以根据传入的参数自动推导出类型
// const myNum = new GenericNumber<number>(100)
const myNum = new GenericNumber(100)
myNum.defaultValue = 10
```

### 泛型工具类型
泛型工具类型：TS 内置了一些常用的工具类型，来简化 TS 中的一些常见操作。
说明：它们都是基于泛型实现的（泛型适用于多种类型，更加通用），并且是内置的，可以直接在代码中使用。

#### `Partial<Type>`
用来构造（创建）一个类型，将 Type 的所有属性设置为可选。
```ts
interface Props {
  id: string
  children: number[]
}
type PartialProps = Partial<Props>

// 解释：构造出来的新类型 PartialProps 结构和 Props 相同，但所有属性都变为可选的。
let p2: PartialProps = {
  id: ''
}
```

#### `Readonly<Type>`
用来构造一个类型，将 Type 的所有属性都设置为 readonly（只读）。

#### `Pick<Type, Keys>`
从 Type 中选择一组属性来构造新类型。
```ts
interface Props {
  id: string
  title: string
  children: number[]
}

type PickProps = Pick<Props, 'id' | 'title'>
```

#### `Record<Keys,Type>`
构造一个对象类型，属性键为 Keys，属性类型为 Type。
```ts
type RecordObj = Record<'a' | 'b' | 'c', string[]>

// type RecordObj = {
//   a: string[]
//   b: string[]
//   c: string[]
// }

let obj: RecordObj = {
  a: ['a'],
  b: ['b'],
  c: ['c']
}
```

## 索引签名类型
当无法确定对象中有哪些属性（或者说对象中可以出现任意多个属性），此时，就用到索引签名类型了。
```ts
interface AnyObject {
  [key: string]: number
}

let obj: AnyObject = {
  a: 1,
  abc: 124,
  abcde: 12345
}
```

## 映射类型
基于旧类型创建新类型（对象类型），减少重复、提升开发效率。

**据联合类型创建**
```ts
type PropKeys = 'x' | 'y' | 'z' | 'a' | 'b'
// type Type1 = { x: number; y: number; z: number; a: number; b: number }
type Type2 = { [Key in PropKeys]: number }
```

1. 映射类型是基于索引签名类型的，所以，该语法类似于索引签名类型，也使用了 `[]`。 
2. Key in PropKeys 表示 Key 可以是 PropKeys 联合类型中的任意一个，类似于 forin(let k in obj)。 
3. 使用映射类型创建的新对象类型 Type2 和类型 Type1 结构完全相同。 
4. 注意：映射类型只能在类型别名中使用，不能在接口中使用。

**根据对象类型创建**
```ts
type Props = { a: number; b: string; c: boolean }

type Type3 = { [key in keyof Props]: number }
```

### 泛型工具类型的实现
![](assets/Pasted%20image%2020240428131207.png)

### 索引查询类型
![](assets/Pasted%20image%2020240428131253.png)
![](assets/Pasted%20image%2020240428131308.png)
