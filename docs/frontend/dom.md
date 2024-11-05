# DOM


DOM Document Object Model 文档对象模型，也就是 JavaScript 控制网页的 API

浏览器把网页转换成了一个 javascript 对象，就是 DOM，允许 js 控制网页元素，增删改查

DOM 是一个树形结构的对象，顶层是 document，有很多种分类，比如 Document 类型，
DocumentType 类型
Element
Attr
Text
Comment
DocumentFragment

这些类型都继承于 Node 接口类型，拥有一些共同的属性和方法


## storage

监听 storage 改变，新开 tab 页，修改了 storage，另一个页面能监听到更新。

```html
    <input type="text" id="name">
    <button id="btn">set storage</button>

    <script>
        window.addEventListener('storage', event => {
            console.log(event.key, event.newValue, event.oldValue, event.url)
        })

        document.getElementById('btn').addEventListener('click', () => {
            localStorage.setItem('name', document.getElementById('name').value)
        })
    </script>
```


btoa 创建一个 base64 编码的字符串
atob 解码一个 base64 编码的字符串


## url
判断 url 是否合法

```js

       try {
        const url = new URL('http://localhost:3000/test.html?name=liqi')
    //    const url = new URL('localhost:3000/test.html?name=liqi')
       console.log('protocol', url.protocol)
       console.log('href', url.href)
       console.log('origin', url.origin)
       console.log('port', url.port)
       console.log('pathname', url.pathname)
       console.log('search', url.search)
       console.log('params', url.searchParams.get('name'))
        url.searchParams.set('name', 'liqi')
        console.log( url.toString());

       } catch (error) {
        alert(error.message)
       }

```

## 处理 searchParams

```js
    const params = new URLSearchParams('?name=liqi&age=18')
        console.log( params.get('name'));
        console.log( params.get('age'));
```

给一个url拼接 search params，一定要 new URL，再追加，最后 url.tostring()


## history

history.replace()


## 刷新当前页面

location.load()
history.go(-1)
location.href = location.href


referrer
window.opener

