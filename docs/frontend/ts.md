# TypeScript

用 TS 的原因，类型安全（Type Safe）

在 JS 中可以在对象上调用不存在的方法，不同数据类型之间可以做算术运算，给函数传递不符合预期的参数。


这些问题在代码编写阶段 JS 一点提示都没有，只有代码运行起来的时候才会报错，而 TS 会在编译成 JS 的过程中报错，如果使用 VS Code 这种支持 TS 语法提示的编辑器进行开发，则编写的代码会直接在显示上就提示程序员出错。

## any 和 unknown

都表示任意类型，但 TS 编译时，不检查 any 类型的变量，即可以进行任何操作，而 unknown 类型必须将类型收窄成具体的类型，才能进行大部份操作，unknown 只支持有限的操作：== === || && ! ? : typeof instanceof 等等。

所以为了代码更加安全，应始终使用 unknown。 any 的价值仅仅是兼容老版本的 JS 能快速地切换到 TS 中。

```ts
let a: {}; // 表示非 null 和 undefined 的任意类型
let b: object; // object 表示常规的 JS 对象类型
let c: Object; // 同 {}，但对 JS 对象内置的方法进行校验，不支持复写？也就是不能改 toString 这种方法
```

如果想要表示有任意属性的对象类型，建议使用 object

## type 和 interface

都用来表示类型，interface 只能用来表示对象类型，而 type 的本意是类型的别名

interface 多次声明会合并

type 不支持多次声明

type 具有块级作用域

type 支持的种类更多，联合类型，字面量类型，模版字符串类型，获取对象的值的类型

如果开发一个第三方包，允许别人扩展，就用 interface，最简单

## enum 

会被编译成 JS 对象，支持反向查找

const enum 不会被编译成 JS 对象，而是直接把值编译进去。

```ts
export enum EColors {
    Red = 0,
    Blue = 7
}

export enum EType {
    Success = 'Success',
    Fail = 'Fail'
}

let value = 0;
value = 7;

if (value === EColors.Blue) {
  console.log(EColors[value])
}

function print(title: string) {
  console.log(title)
}

print(EType.Success)
```

## 加载方式

JS 有两种加载方式，一种是由 script 标签 src 引入的 js，这种叫脚本模式

另一种是模块模式，js 文件中包含 export 或 import。所以最简单把一个 js 文件变成模块，就是加一行 export {};

脚本模式下，变量定义，类型声明都是全局的，多个文件定义一个变量报错？同名 interface 自动合并。

模块模式下，变量定义，类型声明都是模块内有效的。


脚本模式下，声明全局变量的类型

declare var Test {
  abc: string
}

模块模式下，声明全局变量的类型

declare global {
  var Test: {
    abc: string
  }
}

## 类型运算

- & 计算两个类型的交集
- | 计算两个类型的并集

type A = 'a' | 'b'
type B = 'b' | 'c'
type C = A & B
type D = A | B
