# Python

编程语言要编译成机器指令，才能被计算机运行。



Java 编译成字节码，也叫抽象的机器指令，由 jvm Java 虚拟机执行，而 Java 虚拟机也是要把自身编译成机器指令执行，而最原始的机器指令，肯定是二进制的。

龟叔 Guido van Rossum

TIOBE 排行榜

开发电子邮件客户端

电子邮件协议
SMTP
Simple Mail Transport Protocol

GUI 图形化用户接口


C 语言编译后的机器码，在 Windows 上就是 test.exe

Python 解释器，负责执行 Python 代码，用从官方下载的 CPython 解释器即可，用 C 语言开发的。

像其他的解释器如 IPython，PyPy，Jython，IconPython


Windows 记事本会在文件开始的地方加上几个特殊字符 UTF-8 BOM

文本编辑器

直接运行 py 文件，就是不用在终端中键入 python test.py 直接键入 test.py 即可

这就要在代码文件第一行加入特殊注释，说明用操作系统哪个程序执行本文件，也叫做 shebeng 注释？

#! /test python
print('hello world')

Linux 给文件赋予执行权限 chmod a+x hello.py

就可以直接执行了 ./test.py

终端输入输出

name = input('please input your name:')
print('hello', name)

输入输出统称为 input/output，简写 IO

4个空格缩进
# 注释
：后的缩进是代码块
大小写敏感

数据类型
整数
十进制，十六进制0xff，数字分隔符_，提高代码阅读性

浮点数
科学计数法 e



算术运算
 / 得到的结果是浮点数，即使除数和被除数都是整数。
// 地板除，得到的结果是整数，即使除数和被除数都是浮点数，它会丢弃结果中的余数
获取余数 %

整数和浮点数没有大小限制，但超过一定范围，浮点数显示 inf

字符串
单引号，双引号，转义字符 \ 用来转义无法用键盘敲击出来，或者被占用，有歧义的字符
如单引号字符串中的单引号，\n 换行符，\t 制表符，\\ 反斜线

字符串默认是转义的，也可以用 r""，不转义

多行字符串 """ ... """ 会保留键盘输入的换行符

布尔值：True，False

布尔运算
2 > 1
1 < 2
1 == 1
1 != 1
age >= 18

逻辑运算
True and True
False or True
not True

空值：None

变量
给变量赋值 =，用等号这个符号
赋值语句是先执行 = 后面的表达式或值，再将值赋予变量

变量的数据类型不固定，叫做动态语言
如果变量的数据类型固定，叫做静态语言

变量的存储类型：基础类型，引用类型

变量的作用域，也就是可访问性，函数作用域，块级作用域，全局作用域，模块作用域

 Python 没有常量，用纯大写加 _ 表示常量而已


字符编码
计算机只能存储0和1 这两个数字
最早的计算机在设计时，将 8位二进制数，作为一个字节 byte，一位就是一个 bit
而一个 8 位的二进制数，能表示的最大数字就是 11111111，转换成十进制就是 255

操作系统是 32 位，还是 64 位，取决于内存条每一个存储单位的位数，也就是 bit 数
32 位就是一个存储单元能放最大 32 个 1，也就是 4个字节。
64 位，也就是 8 个字节

计算机想要显示字符，就要制定规则，用数字表示字符，这叫字符编码。
比如用 65 表示 A，计算机在遇到 65 的二进制数的时候，就显示成字符串。

计算机是由美国人发明的，所以他们最初制定的编码只包含 a-zA-Z0-9，以及一些特殊符号，总共127个，叫做 ASCII 编码，一个字节就够了。

中文不够，汉字太多了，所以用两个字节表示汉字，兼容 ASCII 编码，因为一个表示字符的二进制数，高位用0表示。这就是 GB2312

各国有各国的语言，又各自制定了编码，导致一个文档如果声明了编码类型，显示其他国家的字符就乱码了。

所以就把所有语言的字符全部编到了一套标准中，叫做  Unicode 标准。

Unicode 标准包含很多编码类型，如 UCS-16 编码，用两个字节表示一个字符，特殊符号用4个字节。其实就类似于 GB2312了，ASCII 的字符高位补0即可，这样就造成了如果是纯 ASCII 编码的文档，体积大一倍，存储和传输，浪费磁盘和网络带宽。

所以 Unicode 标准又制定了 UTF-8 编码，UTF-8 的特点是可变长，英文还是一个字节，汉字3个字节，这样存储和传输就没问题了。

所以任何编程语言，都可以获取字符的 unicode 整数编码的整数值，可以用十进制表示，也可以用十六进制表示，
比如“中” 用十六进制表示，就是 4e2d，所以打印 '\u4e2d' 就可以显示正确的中字。

获取 python bytes 类型，打印形式就是字符串前面加b，字符串内容如果是中文，那就用\x## 表示
'中文'.encode('utf-8')

len('abc') 计算字符数量
len(b'中午') 计算 bytes 类型的长度时，计算的是字节数量

现在写程序，保存文件时，都是用 utf-8 编码，python 解释器解释代码时，让它用 utf-8 去读取，可以在代码文件的开头加上
`# -*- coding: utf-8 -*-

正如 js 引擎遇到 hello ${name}${} 时，就会把它解析成一个表达式

而 python在字符串中遇到 ‘%s %d %f %x %%’
遇到 % 就表示这里是一个表达式，值从后面传入的 % 参数列表中取
参数列表中只有一个，可以省略小括号
"hello %s, your age is %d" % ("liqi", 18)

字符串的 format 方法
'hello {0}'.format('liqi')

f-string 直接替换字符串里面的变量
name = 'liqi'
f"hello {name}"

list
names = ['liqi', 'song']
len(names)
names[0]
names[-1]
names.append('other')
names.insert(1, 'other guy') # 在指定索引的位置插入元素
names.pop()
names.pop(1) # 删除指定索引的元素

names[1] = 'new user'
names = []

tuple 元组，也是一种有序列表，只不过不能新增，修改，删除，也就是说，没有 list 的 append，insert，pop方法

names = (1,2,3)
names = ()
names = (1,)

条件判断
if age > 18:
pass
elif age > 10:
pass
else:
pass


类型转换函数
int('123')

python 没有 switch，只有 match

age = 0

match age:
  case 18:
    print("18")
  case 19 | 20 | 21:
    print("19/20/21")
  case x if x > 21:
    print("> 21")
  case _:
    print("no match")


visitors = ["test"]

match visitors:
  case ["liqi"]:
    print('liqi visited')
  case ['liqi', 'other']:
    print('all visited')
  case _:
    print('no one visited')


list 和 dict 的区别

list 是有序列表，在内存中，要么是连续的内存块，读取的时候，通过索引加首个内存块的地址，就能快速地检索到。
要么是链表，通过迭代来获取到指定索引的元素，这样效率就很低。

而 dict 是 key-value 键值对，key 不重复，所以对 key 获取摘要值，将哈希值进行高效存储迭代，就很快？

所以 dict 的 key 要求不可变？因为要计算它的哈希值？



dict 占据的内存空间比 list 要大。

d = { 'name': 'liqi', age: 18 }
d['name']
d['age'] = 80
'name' in d
d.get('name') # None

d.pop('name')

set 和 dict 类似，但只存储 key，key 不重复。
s = { 1,2,3 }
s = set([1,2,3])

s.add(key)
s.remove(key)

set 就是无序，无重复元素的集合，做数学上的交集，并集操作
s1 & s2
s1 | s2

可变对象和不可变对象

可变对象像 list，里面的元素是不固定的
不可变对象像 str，字符串里面的字符是不可以修改的

可变对象计算哈希值，变化后，计算的结果不一样
不可变对象计算哈希值，每次计算都是一样的结果

而 dict 就是用 key 的哈希值来计算出该 value 应该存储在哪里。
所以不能用可变对象来作为 dict 的 key，这样就意味着，key变了，计算 key 的哈希就变了，就意味着永远也找不到原来的 key 对应的 value 了。

调用不可变对象的任何方法，都不会修改该对象，只会返回新的对象。

函数是什么，是功能的封装，就跟 react hook 一样，是业务逻辑的封装，而业务的状态，是彼此独立的。

help(abs) 在交互式命令行查看函数的帮助信息：包含用法

内置函数
abs(-20)
max(1,2,3)

int('123') # 123
int(12.34) # 12
int() 把其他类型转换成整数

str(100)
把其他类型变成字符串

bool(1)
bool('')

hex(255)

def my_abs(x):
if x > 0:
return x
else:
return -x


names = ['liqi', 'admin']

for name in names:
  print(name)


for item in list(range(10)):
  print(item)

# range(10) 返回一个整数序列

```py
sum = 0
count = 1
while count <= 100:
  sum += count
  count = count + 1

print('sum', sum)
```

break continue


## dict

dictionary 字典，key-value，其他语言叫 map


函数返回多个数值，其实返回的是 tuple

位置参数
必选参数在前，默认参数在后
多个默认参数，如果想给其中一个传入，但它的位置不在前面，传参的时候需要把参数名带上。

默认参数不能是引用类型，也就是可变对象，比如 list，函数定义时会创建一个 list 实例，
如果这个实例内部的元素变动了， 所有调用的函数，都会应用，因为本质上是单例。

可变参数，指的是可以传入N个参数
你想接受一个 list，要么传入一个 list，要么就用可变参数

def printNames(*names):
  for name in names:
    print(name)
names = ['admin', 'test']

printNames(*names)
如果已经有一个 list，相当于解构，也就是把 list 中的元素给迭代出来，依次传给函数。

可变参数的本质上，可以传入0个或多个参数，函数会把它组装到一个 tuple 中，你想从中取一个用，就很麻烦。

关键字参数：
定义0个或多个命名的参数，传入进来，函数会组装成一个 dict

```py
def printUser(name, **kw):
  print('name', name)
  print('other', kw)

printUser('admin', age=18)

user = {
  'age': 18,
  'gender': 'male'
}

printUser('test', **user)

```

限制关键字参数的命名，就得用命名关键字参数

如果前面有可变参数，那就只需要在可变参数后面，写上具体的命名关键字参数即可

如果前面没有可变参数，那就需要在命名关键字参数前面，加个 *，如果不加 *，那就是位置参数了。

如果还有关键字参数，那就放在所有的命名关键字参数后面即可。

命名关键字参数可以设置默认值

命名关键字参数传递参数时，必须写明参数名



```py

```


总结
可以全部放在一起使用

```py
def test(*args, **kw):
    pass
```

这种函数，就可以传入任意个参数，如果加参数名，就放在关键字参数中，如果不加，那就是可变参数

所以传递参数时，如果给了参数名，要么是关键字参数，不管是不是命名关键字参数，要么是不按照顺序传入位置参数


## 递归函数
一个函数调用自身，那就是递归函数了。

理论上，所有的递归都可以改成循环。

使用递归要防止栈溢出，因为计算机在执行一个个函数时，就是不断地在栈中增加一个个节点，等函数执行完，该节点就会退栈。

函数内部的函数如果没有执行完毕，那内部的函数是不会退栈的。

因为递归是不断调用自身，如果调用的多了，那就相当于不断向栈里添加，而栈的容量不是无限制的，超过最大容量，就是栈溢出。

防止栈溢出，可以用尾递归优化，尾递归实质上跟循环一样。

也就是在函数 return 时调用自身，并且 return 后没有表达式，这样编译器和解释器就可以做尾递归优化，无论递归调用多少次，都只占一个栈帧。

但大多数编程语言都没有做尾递归优化，只有做了优化的语言，尾递归才有效果。



## 切片

slice

```py
names = [1,2,3,4,5]

names[0:3]
names[:3]
names[1:3]
names[-2:]

names[0:10:2]
names[::5] # 每5个取一个
```

copy list
names[:]

tuple 和 str 也可以切片

很多编程语言，针对数组和字符串，都有切片函数，而 python 没有，用 slice 运算符就足够了。

## 迭代

C 语言迭代是用数组索引下标来实现的，而 Python 是用 for ... in 来实现的。

for ... in 不仅可以用在 list，还可以用在 tuple，以及其他可迭代对象，如 dict，str

```py
for c in 'abc':
  print(c)

for key in d:
  print(key, d[key])

for value in d.values():
  print(value)

for key, value in d.items():
  print(key, value)
```

判断是否是可迭代对象

list/tuple 同时迭代索引和元素

```py

from collections.abc import Iterable

print(isinstance('abc', Iterable))

for index, value in enumerate(['admin', 'teset']):
  print(index, value)

```

## 列表生成式

用来创建 list

语法：
[] 表明是一个 list
x 表明每一个元素
for ... in 表明每一个元素来源
if 过滤条件

循环可以双层即以上

```py
values = [ m + n for m in 'abc' for n in 'xyz' ]

nums = [ x for x in range(10) if x % 2 == 0]
```

在 js 中，用循环或高阶函数 map filter 的地方，在 python 中，用列表生成式即可

```py
names = [ s.lower() for s in ['A','B'] ] 

nums = [ x if x > 0 else -x for x in [10, -20, 38] ]

```

## generator

生成100万个元素没有必要，一边循环，一边计算会好点，这就是 generator

Iterator


CPU 执行的是加减乘除的指令代码，以及各种条件判断和跳转指令

汇编语言是最贴近计算机的语言。

## 高阶函数

一个函数接收另一个函数作为参数，这种函数就叫高阶函数。

map
reduce
filter
sorted

返回函数，闭包

匿名函数
```py
def f(x):
  return x * x

values = list( map(f, [1,2,3,4]) )

print(values)

values1 = list( map(lambda x: x * x, [1,2,3,4,5]) )

print(values1)
```

## decorator 装饰器
在函数中添加功能，但又不想修改函数的代码，那就给函数添加一个装饰器

在代码运行期间动态增加功能的方式，就叫装饰器

没装装饰器的函数执行了几次，之后再装上装饰器，就扩展了功能

本质上装饰器就是高阶函数，你给哪个函数装，就是把那个函数传递给装饰器的高阶函数，


if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
