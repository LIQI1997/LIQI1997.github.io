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


        window.onunhandledrejection = function(e) {
            console.log('on unhandled rejection', e)
        }
 
        window.onerror = function(e) {
            console.log('onerror', e)
            // 按钮点击事件，会触发
            // 主线程错误会触发
        }
