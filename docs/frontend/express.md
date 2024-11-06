# express



使用 express 开发 Web API服务，或者搭配模板引擎，构建服务端渲染网站，MySQL 作为数据库

body-parser
cors
morgan 日志

## 路由
```js
app.get('/', (req, res) => {

})
app.post('/', (req, res) => {

})
app.put('/', (req, res) => {

})
app.delete('/', (req, res) => {

})
```

## 请求和响应
```js
app.get('/test', (req, res) => {
console.log(req.url)
console.log(req.method)
console.log(req.headers)

const { current, pageSize } = req.query;
res.json({
current,
pageSize
})
})

app.get('/user/:id/:name', (req, res) => {
const { id, name } = req.params;
res.json({
id,
name
})
})

app.get('/test', (req, res) => {
res.cookie('name', 'liqi')
res.status(500).json({
name: 'liqi'
})
})

```

## 中间件

```js
function logger(req, res, next) {
console.log(`${Date.now()} ${req.method} ${req.url}`)
req.name = 'liqi';
next();
}
// 不限制请求路径
app.use(logger);

// 限定请求路径
app.use('/user', (req, res, next) => {
console.log('logger')
next();
})
// 还可以限制请求方法
app.get('/user', (req, res, next) => {

})
```

## json 参数
```js
app.use(express.json())

app.post('/', (req, res) => {
const { id, name } = req.body;
res.json({
id,
name
})
})
```

## 错误处理中间件

```js

```
