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



Typescript

TS 是微软开发的开源的编程语言，它是 javascript 的超集，提供可选的静态类型和其他 js 没有的功能。



Ts 在函数上面写 /**
	test
	@author liquid
*/

interface Test1 {
    name: string;
}

interface Test2 {
    age: number;
}

type User = Test1 & Test2; // 交叉类型
type Test = Test1 | Test2; // 联合类型


const user: User = {
    name: 'liqi',
    age: 18,
}

const test: Test = {
    name: 'admin'
}

type S = 'admin' | 18 | false; // 字面量类型
type Url = `https://${string}`; // 字符串模版类型
type ImageType = 'png' | 'jpg' | 'jpeg';
type ImageUrl = `https://${string}.${ImageType}`;

let img: ImageUrl = 'https://test.png';

// 断言
let name: unknown = '';
const length = (name as string).length;

console.log((<string>name).length);

(<string>name).length;

// 非空断言

let x: string;

x!.length;

// obj null undefined 时停止表达式执行
let obj = { name: '' }
const name2 = obj?.name;

// 空值合并
let name3 = '123' ?? '12345'

// 获取所有键，返回值是联合类型
type User22 = {
    name: string;
    age: number;
}
type keys = keyof User22;

// typeof 获取变量或对象的类型

// 获取值的类型
type Age = User22['age'];

// in 遍历所有属性
type A = {
    readonly [K in keyof User22]: User22[K];
}

// 类型别名

// 类型断言
// <string>obj;
// obj as string;

// 类型收窄
function test2(value: any) {
    if (typeof value === 'number') {
        value.toFixed(2)
    }
}

// 范型，就像给函数传参一样，可以给类型传参，得到特定的类型
type MyArray<T> = T[];
let arr: MyArray<string> = []
arr.push('123')
// arr.push(444)

// 常见的范型命名 T: type, K: key, V: value, E: element

// 类型范型，接口范型
interface U<T> {
    name: T
}

let u: U<string> = {
    name: '123'
}

// 范型约束
interface User3 {
    name: string;
    age: number;
}
type U3<K extends keyof User3> = K;

let u3: U3<'age'> = 'age';

// 范型默认值
type SS<T = number> = {
    name: T,
}

let ss: SS = {
    name: 123
}

let ss1: SS<string> = {
    name: '123'
}

// 影射类型，其实就是利用类型范型将之前的对象类型生成新的对象类型

type ReadonlyUser = Partial<User22>;

type MyPartial<O> = {
    [K in keyof O]+?: O[K]
}

// Required

type MyReadonlyUser = MyPartial<User22>;


export default {}


这种注释，可以被 vscode 识别显示出来

先写对象，再写类型

export const user = {
    name: 'liqi'
}

type User = typeof user;


提取子类型出来
查找类型

export type User = {
    name: string;
    address: {
        province: string;
        city: string;
    }
}

type Address = User['address'];

const a: Address = {
    province: 'js',
    city: 'nj'
}

综合使用：查找类型，范型，keyof

export interface API {
    '/user': {
        name: string;
        age: number;
    },
    '/book': {
        name: string;
        publishDate: string;
    }
}

export function getData<Url extends keyof API>(url: Url): Promise<API[Url]> {
    return new Promise((resolve, reject) => {

    });
}


getData('/user').then(res => {
    console.log(res.name.length)
    console.log(res.age.toFixed(2))
})

export function $<T extends HTMLElement>(id: string): T {
    return document.getElementById(id);
}

// 递归定义类型

export type DeepReadonly<T> = {
    readonly [P in keyof T]: DeepReadonly<T[P]>
}


const info = {
    address: {
        city: 'nj'
    }
}

const info2 = info as DeepReadonly<typeof info>;

info2.address.city = '123';


import { Button, ButtonProps } from './components/button'

type Omit<T, K> = Pick<T, Exclude<keyof T, K>>
type BigButtonProps = Omit<ButtonProps, 'size'>

function BigButton(props: BigButtonProps) {
  return Button({ ...props, size: 'big' })
}


export type AnimalType = 'cat' | 'dot' | 'frog';

Enum AnimalType {
	cat = ‘cat’,
	dog = ‘dog’
}

export type AnimalOption = {
    name: string;
    age: number;
}

export type Animals = Record<AnimalType, AnimalOption>;

let animal: Animals = {
    cat: {
        name: '',
        age: 0,
    },
    dot: {
        name: '',
        age: 0,
    },
    frog: {
        name: '',
        age: 0
    }
}
