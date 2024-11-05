# 错误监控

错误监控
自己本地部署的sentry监控平台


## 全局 js 错误拦截

```js

        // 拦截未 catch 的 promise 错误
        window.onunhandledrejection = function(e) {
            console.log('on unhandled rejection', e)
        }
 
        window.onerror = function(e) {
            console.log('onerror', e)
            // 按钮点击事件，会触发
            // 主线程错误会触发
        }
```