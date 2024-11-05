# React

## State

传入一个函数，是懒加载，如果传入一个执行的函数，相当于每次组件渲染时都会执行，如果这个函数比较耗时，就影响了性能。

```ts
import {  useState } from "react"


function App() {

  function getValue1() {
    console.log('get value1')
    return Math.random();
  }

  function getValue2() {
    console.log('get value2')
    return Math.random();
  }

  const [value1] = useState(getValue1)
  const [value2] = useState(getValue2())
  
  const [count, setCount] = useState(1)

  return (
    <div>
      <h1>{value1}</h1>
      <h1>{value2}</h1>
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  )
}

export default App
```


## useEffect

第一次挂载封装一个 hook

第一次更新才执行，封装一个 hook

想在 react 组件中定义一个计时器，需要在 use effect [] 中注册，在组件卸载时删除。如果需要提前删除，那就得把计时器的 id 通过 useRef 暴露出去

这样其实就可以封装一个自定义 hook 了

useInterval



redux 更新状态，通过 dispatch action plain object to store，所以没有异步更新的方式，可以通过 redux 插件来实现异步更新，比如 redux-saga，还是什么其他的。

当然你也可以自己写 hook 来更新，自己写 hook 的好处就是可以把用户信息顺便暴露出来，就不需要再导入 useSelector 包了。

但还是建议异步更新还是用 redux 插件，然后再封装 hook 来设置和读取 redux store 中的 user state，这样就是最佳实践，也很好用

配置路径别名，不光要在打包工具中配置，因为打包时要告诉工具，正常打包，还要配置 tsconfig，要不然写的代码，ts 不认识这个路径。

webpack 虽然慢，但是生态成熟，还是值得使用的，毕竟很多公司有很多老项目，而且可以学习 webpack 的思想

学好一个技术，然后做技术分享，也是有价值的，有含金量，一定是别人不会的，我现在最欠缺的技术有很多，计算机，Linux，network

网盘也可以存储3d数字资产，只要可以预览即可。
要分析网盘的用户群体。

而预览很简单，通过 web assembly 或是 three.js 都可以。但最好的方式还是后端推流。
后端推流意味着自己的成本就要增加，要增加 gpu 服务器，没有充分利用客户机器。
其实选择什么方式都无所谓，主要还是技术，技术强了，任何路都能走到终点。

自定义 hook 封装的是逻辑，即hook里面的状态是每个组件彼此独立的。
可我需要的是一个全局的 api，单例，可以存储数据，这就够了。因为自定义hook是在一个独立的js 文件中，所以也就是独立的js模块，只要声明一个模块内顶层变量作为数据容器即可。


什么时候封装 hook？在需要用到组件的副作用，或者数据需要参与视图渲染的时候。
什么时候定义 state，当需要触发视图重新渲染，或者需要依赖它来执行 effect 的时候。


为什么现在市面上有很多封装好的 hook，因为都想把 js 常见的代码块封装到 react 组件里面去，而 hook 就让 react 组件很容易集成，因为无非就三样东西，useState，useEffect，useRef


 因为用户的会员信息，每次进入组件都要请求，而用户的基本信息，可以从 local storage 中读取，因为登录成功后都会浏览器本地缓存好 uid，mobile，email，这里就不用再次请求了，只需要每次 report 的时候读取 local storage 中读取即可。
而换到新页面的时候，重新请求用户的会员信息即可，比较方便。

缓存是一把双刃剑，开多个 tab，如果下个 tab 登录了其他的账号，而上一个 tab 里面的 token 其实是新用户的了，可是页面上却是旧用户的数据。

不断学技术就行了，这是我现阶段最应该做的事情。


更新状态
import {  useState } from "react"


function App() {

  const [count, setCount] = useState(1)

  const handleClick = () => {
    // 这是个闭包，当组件渲染（App 函数重新执行）时，会创建新的 handleClick，
    // 里面读取的 count 这个状态，是固定的，不会变的

    setCount(count + 1)
    setCount(count + 1)
    setCount(count + 1)

    // 调用修改状态的函数，本质上就是在更新状态的队列中加了三次，但每次给的值都是同一个值。

    // 这个闭包结构里使用了三次 count，都是相同的值，如果依赖之前的状态，
    // 得传入一个函数，

    // setCount(prevCount => prevCount + 1)
    // setCount(prevCount => prevCount + 1)
    // setCount(prevCount => prevCount + 1)

    // 这种就是状态的更新队列里有三个待执行的函数，React 会在执行更新函数时，会把当时的状态
    // 作为参数传递给函数
  }

  return (
    <div>
      <button onClick={handleClick}>{count}</button>
    </div>
  )
}

export default App

修改引用类型的状态
import {  useEffect, useState } from "react"


function App() {

  const [user, setUser] = useState({
    name: 'liqi',
    age: 18
  })

  const handleClick = () => {
    // user.age = 99; // 直接修改引用类型的状态中的数据，不会触发重新渲染，也不会触发 effect 发生
    setUser({
      ...user,
      age: 99
    })
    // 创建新对象，覆盖状态才行
  }

  useEffect(() => {
    console.log('component changed', user.age)
  })

  useEffect(() => {
    console.log('user.age changed', user.age)
  }, [user])

  useEffect(() => {
    console.log('age changed', user.age)
  }, [user.age])

  // 会触发更新，副作用函数也会执行

  return (
    <div>
      <button onClick={handleClick}>{user.name}-{user.age}</button>
    </div>
  )
}

export default App

状态存函数类型？没遇到过这种需求！

改了 state，然后立刻使用 state，肯定不行，
改了 state，立刻使用依赖 state 的 computed state，也不行
如果数据没有被视图使用，那就没有必要定义 state



避免条件竞争
import { useEffect, useState } from "react";

export function useData(url: string) {

    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    const [data, setData] = useState<any>(null)

    useEffect(() => {

        let ignore = false;

        fetch(url).then(res => res.json()).then(res => {
            if (!ignore){
                setData(res)
            }
        }).catch(err => {
            setData({
                err: err.message
            })
        })

        return () => {
            ignore = true;
        }
    }, [url])

    return data;
}

自定义hook
import { useEffect, useState } from "react";

export function useOnlineStatus() {

    const [isOnline, setIsOnline] = useState(true)

    function handleOnline() {
        setIsOnline(true)
    }

    function handleOffline() {
        setIsOnline(false)
    }

    useEffect(() => {
        window.addEventListener('online', handleOnline)
        window.addEventListener('offline', handleOffline)

        return () => {
            window.removeEventListener('online', handleOnline)
            window.removeEventListener('offline', handleOffline)
        }
    }, [])

    return isOnline

}


redux 工作流程是组件给 store dispatch 一个动作，提交 plain object，覆盖 store 中的 state，组件中通过 useSelector 监听的 state 变化，自动更新UI。

redux 默认写法
npm i redux
npm i `@ reduxjs/toolkit`
npm i react-redux
npm i redux-devtools
import { useEffect, useState } from "react";
import { createStore } from "redux"

const reducer = (state = {
  todos: ['test'],
}, action: any) => {
  console.log('action', action)
  switch(action.type) {
    case 'add':
      return { todos: [...state.todos, action.payload] }
    case 'reduce':
      return { todos: state.todos.filter((_,index) => index !== action.payload)}
    default:
      return state;
  }
}

const store = createStore(reducer)

function App() {

  const [title, setTitle] = useState('')
  const [todos, setTodos] = useState<string[]>([])

  useEffect(() => {
    console.log('start subscribe')
    const cancel = store.subscribe(() => {
      return setTodos(store.getState().todos)
    })
    return () => {
      cancel();
      console.log('end subscribe')
    }
  }, [])

  return (
    <>
      <div>
        <input type="text" onChange={e => setTitle(e.target.value) } />

        <button onClick={() => { store.dispatch({ type: 'add', payload: title }) }}>add</button>

        <ol>
          {todos.map(item => <li key={item}>{item}</li>)}
        </ol>
      </div>
    </>
  )


}

export default App

redux-saga 
组件 dispatch 一个异步行为，根据 saga 中定义好的类型，拿到异步结果，dispatch plain object，从而覆盖 store 中的 state。

react-window 虚拟列表
react-virtualized 长列表优化
react-transition-group 过渡动画
react-sortable-hoc 拖拽库
react-selecto 可以在拖拽元素中，多选元素。
react-selecable-fast 用来在大数据表格或列表中多选元素
react-scroll 监听滚动事件
react-resizable 改变元素的尺寸
react-lazyload 延迟加载组件，优化性能
react-fast-marquee 跑马灯，滚动元素
react-dnd 拖拽
react-app-polyfill 在 react 项目中使用 polyfill，很方便。
import 'react-app-polyfill/stable' 根据 package.json 中配置的浏览器列表来设置 polyfill

渲染组件的写法

```ts
<Test3 title="123" />

      <hr />

      {Test3({title: '456'})}
组件内部渲染元素的写法
export default function Test3(props: {title: string}) {

    console.log('render')

    const h1 = <h1>h1</h1>

    const h2 = () => <h2>h2</h2>

    return (
        <div>
            {h1}
            {h2()}
            test is {props.title}
        </div>
    );
}


import { useEffect } from "react";

export function useTest(title: string) {
    useEffect(() => {
        console.log('title changed', title)
    }, [title])
}

import { useRef, useState } from 'react'
import { useTest } from './useTest'

function App() {

  const [title, setTitle] = useState('111')

  useTest(title)

  function change() {
    setTitle(Math.random().toString())
  }

  return (
    <>
      <button onClick={change}>app</button> 
    </>
  )
}

export default App
```

ahooks
const clear = useInterval(() => {
        setCount(count + 1)
    }, 1000)

useSetState 自动合并对象类型的状态，
比如状态有a、b两个属性，你可以 setState({b: 100}); a 属性还是原样。

const [user, setUser] = useSetState({
        name: '',
        age: 0,
    })

    return (
        <div>

            <button onClick={() => setUser({name: 'liqi'})} >change name {user.name}</button>

            <button onClick={() => {
                setUser(prev => ({age: prev.age + 1}))
                setUser(prev => ({age: prev.age + 1}))
                setUser(prev => ({age: prev.age + 1}))
            }}>  age is {user.age}</button>
          
        </div>
    );




ref
const input = useRef<HTMLInputElement | null>(null)

  const handleClick = () => {
    input.current?.focus();
  }

  return (
    <>
      <input type="text" ref={input} />
      <button onClick={handleClick}>focus</button>
不需要在视图中渲染的数据，不应该保存为 state，而应该保存为 ref
const count = useRef<number>(0)

  return (
    <>
      <button onClick={() => { count.current = count.current + 10; console.log(count.current) }}>add</button>
而且修改了state，也不能立刻使用，但修改了 ref，可以立刻使用，修改了state，会触发组件函数重新执行，修改了 ref 不会。

获取子组件的 dom 元素，也就是把 ref 传递给子组件，让子组件去赋值

const input = useRef<HTMLInputElement | null>(null)

  return (
    <>
      <Test2 title='姓名' ref={input} />
      <button onClick={() => input.current?.focus()}>focus</button>

import { forwardRef, type Ref } from "react"


const Test2 = forwardRef(( {title}: {title: string}, ref: Ref<HTMLInputElement> ) => {
    return (
        <div>
            <input type="text" placeholder={'请输入' + title} ref={ref} />

        </div>
    )
})

export default Test2;
但这样父组件就可以拿着子组件胡作非为，比如可以可以获取子组件中 input 的 value，如果想限制父组件只能 focus，可以
const input = useRef<HTMLInputElement | null>(null)

  return (
    <>
      <Test2 title='姓名' ref={input} />
      <button onClick={() => input.current?.focus()}>focus</button> 
      <button onClick={() => alert(input.current?.value)}>get input value</button>

import { forwardRef, useImperativeHandle, useRef, type Ref } from "react"
import { Control } from "./App"


const Test2 = forwardRef(( {title}: {title: string}, ref: Ref<Control | undefined> ) => {

    const input = useRef<HTMLInputElement | null>(null)

    useImperativeHandle(ref, () => {
        return {
            focus() {
                input.current?.focus();
            }
        }
    }, [])
    return (
        <div>
            <input type="text" placeholder={'请输入' + title} ref={input} />

        </div>
    )
})

export default Test2;
import { useRef } from 'react'
import './App.css'
import Test2 from './Test2'

export type Control = {
  focus: () => void
}


function App() {

  const ref = useRef<Control>()

  return (
    <>
      <Test2 title='姓名' ref={ref} />
      <button onClick={() => { ref.current?.focus() }}>only focus</button>
    </>
  )
}

export default App
useImperativeHandle 如果子组件不想把内部的 dom 暴露给父组件，那就只暴露几个方法或属性，这样就实现了权限隔离。比如只暴露 input 的 focus，不暴露 input 的 dom，那父组件就不可以改子组件中 input 的样式。让父组件访问受限。
当然也可以暴露修改子组件的状态的方法，但这种不是最佳实践。

在 react 中，父组件如何拿到子组件里面某一个 dom 元素？
如果父组件想控制子组件里面10个input 的 focus，怎么办？forwardRef 只能接受一个 ref，那就用 useImperativeHandle

useMemo 缓存数据，不至于每次组件函数执行（渲染时）都执行

import { useMemo, useState } from "react"

type Todo = {
  title: string;
  isDone: boolean;
}

function App() {

  const [list, setList] = useState<Todo[]>([])

  // const length = list.filter(item => item.isDone).length;

  const length = useMemo(() => list.filter(item => item.isDone).length, [list])

  return (
    <div>

      <button onClick={() => setList([...list, {title: Math.random().toString(), isDone: false}])}>add</button>
      
      <ul>

        {
          list.map((item, index) => <li key={item.title}>{item.title}
            <button onClick={() => { setList(list.map((todo, idx) =>  index === idx ? {...todo, isDone: true} : todo )) }}>done</button>
          </li>)
        }
        
      </ul>

      <h1>done count is {length}</h1>

    </div>
  )
}

export default App

jsx return 后面可以加函数声明
function App() {

  const getData = () => {
    test();
  }

  return (
    <>
      <button onClick={getData}>get</button> 
    </>
  )

  function test() {
    console.log('test')
  }
}

export default App


ahooks useRequest 处理异步



如何封装一个阻止冒泡的组件，用 React.createElement(OldElement)

react-is 判断是否是 react 的数据类型