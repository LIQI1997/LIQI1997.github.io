# JavaScript

js 运行原理，V8 引擎，

内存堆：给对象分配内存的地方

调用栈：函数执行的地方

memory heap

call stack

浏览器提供的 API，浏览器是 js 宿主环境，叫做 Web APIs，包括 DOM，AJAX，Timeout

js 是单线程语言，只有一个调用栈，意思就是你写的代码，只能在这个线程上执行，这个线程叫做主线程，js 还有其他线程，但都是通过主线程去调度的，跟程序员没关系。

调用栈是一种数据结构，记录程序时时刻刻执行的位置，代码运行到一个函数，就会把函数压进调用栈，函数返回时，就会弹出调用栈。

每一个进入调用栈的函数，都叫做：“调用帧”

当某一个函数抛出错误时，浏览器控制台错误追踪就能定义到错误发生的堆栈信息。

堆栈溢出，调用栈的容量是有限制的，到达最大时，程序就会抛出错误。



浏览器会等待 js 执行完，才会去渲染 html。

所以如果调用栈中有函数大量耗时，那就意味着网页渲染出现卡顿了。


如果在不阻塞UI的情况下执行复杂代码，通过异步和回调


事件循环，回调函数队列，主线程，堆栈溢出，阻塞浏览器

## 高阶函数

高阶函数就是参数是函数的函数

## 闭包

闭包是一种程序结构，一段代码组成的语句格式。
返回内部函数，内部函数访问外部函数中的函数作用域中的变量，导致返回的内部函数，只要没有被垃圾回收，那就一直保存在内存中。

垃圾回收
全局变量不会被回收，只有程序结束运行了操作系统才会释放js执行占用的内存。

一个引用类型的变量，比如一个对象，如果没有变量引用它，就会被回收。
如果两个对象相互引用，没有第三个变量引用，那这两个对象也会被回收。

闭包结构是彼此独立的

```js
function getCounter() {
            let count = 0;
            return function() {
                count++;
                console.log(count)
            }
        }

        getCounter()();
        getCounter()();

        const couter = getCounter();
        couter();
        couter();
```


## 面向对象

new 是一个运算符，把它理解成一个函数，后面调用的函数是它的参数，会被 new 内部执行。
这个函数是用来构造对象的，所以也叫构造函数。
new 会把 this 这个变量传递给构造函数，this 是一个空对象，new 会默认返回 this


## promise

promise 的 then 或 catch 中，如果 return 一个对象，不论对象是什么类型，新创建的 promise 都是 resolved 的，只有在 then 或 catch 中 throw 一个对象，才会走 新的 promise 的 catch。
throw 什么，catch 什么，不一定非要 throw 一个 new Error('');
throw 一个 string，catch 的 e 就是一个 string。

```js
const p = task().then(res => {
      console.log('res', res)
      return 'after then'
    }).catch(err => {
      console.log('err', err)
      // return 'after catch'
      // return new Error('error')
      // throw '132';
      throw new Error('test error')
    })

    console.log('p', p)
    p.then(res => {
      console.log('p res', res)
    }).catch(err => {
      console.log('p err', err)
    })
```


setInterval 固定时间间隔触发
setTimeout 延迟触发，如果需要循环，就需要递归
window.requestAnimationFrame(() => {})
跟 setTimeout 很像，但时间间隔是浏览器下次重绘之前，不同刷新率的屏幕，浏览器重绘的时间间隔是不一致的，60Hz 刷新率的屏幕，即1000 ms 刷新 60 次，16.6 ms 刷新一次
递归就是自己调用自己

## 防抖/节流

- 防抖是短时间内大量触发耗时的操作，只会执行最后一次

- 节流是短时间内大量触发耗时的操作，只会按照固定的时间间隔执行







递归，传递和回归？



判断数据类型，可以用 对象的 toString 方法，
Object.prototype.toString.call(obj) === '[object Date]'

或者通过调用 Date 上的方法， 如果报错，就不是该对象 try catch

判断非空，不等于 undefined 和 null
判断数字非空，不等于 NaN
判断字符串非空，不等于 ''

判断是 nodejs 还是 browser 环境，用 window 全局对象是否存在判断

[1,2,3].some(item => item % 2 === 0); // true 数组中是否有元素符合条件


判断闰年

function isLeapYear(year: number) {
  return (year % 4 === 0  year % 100 !== 0) || year % 400 === 0;
}

## 闭包 closure

函数内部可以读取函数外部的全局变量，反之不行。

函数外部无法读取函数内部的局部变量，要想实现这个功能，要么把函数内部的局部变量给 return 出去。

要么 return 一个函数内部的内部函数，这样内部函数就可以读取外部函数的局部变量了。

直接 return 出去，值只能在外面改。而 return 内部函数，每次执行内部函数，修改也好，读取也好，该局部变量都能被访问到。

内部函数可以读取外部函数的所有局部变量，这叫链式作用域 chain scope，子对象可以一层一层向上寻找所有父对象的变量，一直找到全局变量为止。

所以闭包，可以理解成在外部函数中的内部函数。

### 作用
持久化存储外部函数的局部变量，只要内部函数还在用，那外部函数就没有退栈。

而且因为它的作用域是函数作用域，所以就不用声明全局变量了，声明全局变量容易被覆盖。

因为外部函数的局部变量一直存在内存中，内存消耗很大，所以不能滥用闭包，要不然会有性能问题，IE 可能导致内存泄漏？


所以可以把外部函数当做一个对象，它的局部变量都是属性，而闭包就是这个对象的方法，可以读取并修改属性。