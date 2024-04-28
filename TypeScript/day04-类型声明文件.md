# 一、类型声明文件
用来为已存在的 JS 库提供类型信息。

## TS 中的两种文件类型
1. .ts 文件：
	1. 既包含类型信息又可执行代码。
	2. 可以被编译为 .js 文件，然后，执行代码。
	3. 用途：编写程序代码的地方。
2. .d.ts 文件：
	1. 只包含类型信息的类型声明文件。
	2. 不会生成 .js 文件，仅用于提供类型信息。
	3. 用途：为 JS 提供类型信息。

总结：.ts 是 implementation（代码实现文件）；.d.ts 是 declaration（类型声明文件）。
如果要为 JS 库提供类型信息，要使用 .d.ts 文件。

## 类型声明文件的使用说明
### 使用已有的类型声明文件
1. 内置类型声明文件：TS 为 JS 运行时可用的所有标准化内置 API 都提供了声明文件。
2. 第三方库的类型声明文件：目前，几乎所有常用的第三方库都有相应的类型声明文件。

第三方库的类型声明文件有两种存在形式：1 库自带类型声明文件 2 由 DefinitelyTyped 提供。

DefinitelyTyped 是一个 github 仓库，用来提供高质量 TypeScript 类型声明。
可以通过 npm/yarn 来下载该仓库提供的 TS 类型声明包，这些包的名称格式为：`@types/*`。 比如，@types/react、@types/lodash 等。 
说明：在实际项目开发时，如果你使用的第三方库没有自带的声明文件，VSCode 会给出明确的提示。

### 创建自己的类型声明文件
1. 项目内共享类型
如果多个 .ts 文件中都用到同一个类型，此时可以创建 .d.ts 文件提供该类型，实现类型共享。
```ts
// index.d.ts
type Props = { x: number; y: number }
export { Props }

// a.ts
import { Props } from './index'
let p1: Props = {
  x: 1,
  y: 2
}
```

2. 为已有 JS 文件提供类型声明
**在将 JS 项目迁移到 TS 项目时，为了让已有的 .js 文件有类型声明。**
> 开发环境准备：使用 webpack 搭建，通过 ts-loader 处理 .ts 文件。【暂时了解】

- 说明：在导入 .js 文件时，TS 会自动加载与 .js 同名的 .d.ts 文件，以提供类型声明。
- declare 关键字：用于类型声明，为其他地方（比如，.js 文件）已存在的变量声明类型，而不是创建一个新的变量。 
	1. 对于 type、interface 等这些明确就是 TS 类型的（只能在 TS 中使用的），可以省略 declare 关键字。 
	2. 对于 let、function 等具有双重含义（在 JS、TS 中都能用），应该使用 declare 关键字，明确指定此处用于类型声明。

[utils.js](assets/TypeScript-资料/day-04/codes/day-04/06-demo/src/utils.js) | [utils.d.ts](assets/TypeScript-资料/day-04/codes/day-04/06-demo/src/utils.d.ts)

> 在 React 中使用 TypeScript【以后再说】

