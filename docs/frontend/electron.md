# Electron


win.restore() 从最小化窗口恢复，任务栏还是有的？
win.show() 从后台恢复，即这个窗口被隐藏起来了，任务栏默认都没有？还是说故意隐藏起来的？

自定义协议在 mac 端通过 open-url 事件打开，Windows 没有独立的事件，可以在 second-instance 中判断被打开，想拿参数，就通过 electron 的启动参数去拿

本地的网页想要调用 nodejs 的 api，需要开启 nodeIntegration: true，node 集成。
如果不开，preload.js 也是无法访问 nodejs 的 api 的，因为默认情况下，渲染进程在沙箱中，开 sandbox: false，关闭沙箱模式才能访问，其实这里是部分 api 是需要关闭沙箱模式的，有些 api，即使在沙箱模式下，也是可以用的。

context Isolation 上下文隔离，默认为 true，就是 preload 和 普通的 js 是否共享 window 和 document 对象

上下文是 v8 的全局作用域的概念，每个上下文都有一个独立的 window 对象，彼此隔离，有各自的全局作用域，全局变量，原型链。





electron/remote
很多方法只能在主进程中使用，不能在渲染进程中使用，只能发消息给主进程，这样很麻烦，用 electron/remote 可以方便的在渲染进程中调用主进程的方法。

很多nodejs库不能直接在 electron 环境中使用，所以需要重新编译，以适配 electron 环境，electron-rebuild 就是干这事的。
electron-notarize 对 app 用数字证书签名，是为了防止用户下载下来的 app 被篡改。
签名后的 app 用户不能直接安装使用，需要被 apple 公证（notarize），也就是加到 apple 的白名单中，这样才能被安装使用。


## electron-builder
msi
windows installer

nsis
nullsoft scriptable install system 

两种不同的应用程序打包工具，msi 是 Windows 独有的，nsis 不光可以打 Windows 的程序安装包，还能打 mac 的安装包。

一个 app 开发完之后，可以打包成可运行的程序，比如 Windows 下直接打一个 test.exe，mac 下就是一个 .app

这种也叫做绿色包，不需要安装。

如果开发完打成安装包，就可以通过操作系统来安装和卸载，可以控制安装路径，同意使用协议，否则不给安装，控制是否开启自动启动，控制是否创建桌面快捷方式，固定在任务栏，注册环境变量path，添加到右键菜单等等。
可以添加到开始菜单




运行，packer 打包，绿色包，自定义协议，单实例运行，scheme 唤起，加载远程网址，实现套壳

scheme 计划，规划，方案，体系，组合，配合，图表，计谋，草图

自定义协议，就是将应用起个名称，注册到 os 中，浏览器通过你定义的名称： 就可以唤起你的应用，这叫 scheme 唤起，可以传递参数，app 能监听唤起，拿到参数，mac 通过 open-url 事件监听，拿到参数

程序未启动，会启动，已启动，会激活，反正拿参数，都是在 open-url 监听

open-url 在 app ready 之前注册？

Windows 没有 open-url，直接在启动时会传递参数，通过 process.argv 取

如果程序支持多实例，那每次 scheme 都会打开新的实例，如果不支持，那就通过 second- instance 监听，通过 argv 参数取出参数


传统的网页在浏览器沙箱环境中执行，无法调用操作系统的 API，electron 环境内的网页，支持通过 nodejs 调用操作系统的 API，默认不开启，要手动开启，

nodeIntegration: true

generic 通用的，一般的，普通的，无商标的，无厂家商标的

forced 被迫的，强迫的，不得已的，不真诚的

怎么样清空浏览器缓存，加载新的 html 首页，首页里面的 js 链接都追求 ?t=123 时间戳

我没有时间研究那些 hook，我要把electron 研究透

const os = require('os')
os.platform()
os.release()

直接调用 nodejs，如果加载的还是第三方网页，危害极大，比如调用 fs 删除文件。所以建议不开启 node 集成，通过 preload.js，由 electron 将暴露的 nodejs API 注入到 html 中，preload.js 会最先执行，其他 html 引入的 js 都是后执行。
preload.js 可以使用 nodejs 的 API

electron 20 开始，渲染进程默认进入沙箱模式，需要指定 sandbox false 才能在 preload.js 中调用 nodejs api

preload 加载时，html dom还未创建，所以不可以直接获取 dom，可以在 document.addeventlistener( 'DOMContentLoaded' 事件触发再执行

electron 实例启动，默认使用 package.json 中的 main 字段寻找入口 js 文件

electron 实例启动，
会有 electron 主进程：负责子进程管理，os api 封装，可以操控 os
electron helper 网络进程
electron helper（GPU）
electron helper （Plugin）负责插件运行
electron helper（Renderer）渲染进程：负责网页排版布局渲染 blink 排版引擎，和 js 代码执行 v8 引擎

每创建一个 BrowserWindow 并加载文件 load file or load url，都会创建一个 Renderer 进程。

主进程调用的 API 比渲染进程要多，所以如果渲染进程想用，只能通过让主进程用，然后跟主进程通信，拿到结果，跟主进程通信叫做 IPC
写法是 invoke，handle
invoke 拿到异步结果，是通过 then 来获取。

preload.js 只能访问 nodejs 的API，以及 electron 提供的渲染进程 API，

contextIsolation: false, 上下文隔离，默认开启

开启了之后，preload 中的 window 和 document，跟加载的 html 页面的 window 和 document 不是一个对象，处在不同的 context 中，不共享全局变量，全局作用域，原型链。

所以如果 contextIsolation 如果开启，直接在 preload.js 中给 window 添加属性，html 中无法使用。得通过 context Bridge.exposeInMainWorld 暴露出去才能使用

使用主进程的 nativeTheme 拿到主题色，修改主题色

也就是说，electron 环境里面的页面js脚本，是集成了 nodejs 能力的，但默认不开启，需要 nodeInte 开启。
但开启了暴露的权限就太多了，建议不开，由 preload.js 注入，但默认 preload 和 html 属于不同上下文，所以不共享全局变量，可以 context Isolation 改掉，但也不建议，建议通过 context bridge.exposeInMainWorld 暴露。

https://juejin.cn/user/1961184474432830/posts


右键菜单

分清 electron 实例 和 窗口的关系。
单例模式，就是只允许用户在电脑上启动一个 electron 主进程。
可以一个窗口都没有，比如 macos，通过 command + Q 关闭应用。

window show 时，读取剪切板数据，判断是否是 http 或者 bt，从而可以创建下载任务

弹窗可以是原生，可以是 HTML 模态框，主进程给用户看的，才是原生，否则都用模态框即可。


process.versions.chrome
process.versions.node
process.versions.electron

主进程管理多个渲染进程

主进程负责调用操作系统的 API，而渲染进程中的网页要是想调用操作系统的 API，只能通过进程间通信（IPC interprocess communication），让主进程去调用

ipcMain ipcRenderer 都是 EventEmitter 对象


v8-inspect-profiler 监测 electron 主进程性能

v8-compile-cache：v8 执行 js 代码都是先解析，编译，再执行，使用这个库，可以减少下次的编译工作。

VLC 开源的 media player，可以集成进 electron 项目

import {app, shell } from electron
app.setPath('', '')
app.getGPUFeatureStatus()
app.disableHardwareAcceleration()

electron-log
electron-store 本质上是 json 文件
store.initRenderer(); 设置必要的ipc通道，要不然在渲染进程中无法使用 store？

设置 process.env.PATH = path; ${process.env.PATH}有什么用？能改用户电脑的环境变量？应该改不了。

autobind-decorator
boundClass

electron-updater

lock = app.requestSingleInstanceLock
!lock app.exit();

win.isMinized()
win.restore()
win.focus()

app.isPackaged()

path.isAbsolute(path);

app.getAppPath();

browserWindow
win.webContents.send();

app.setAsDefaultProtocolClient();
app.isDefaultProtocolClient();

app.commandLine.appendSwitch

app.on('open-url')

app.on('second'

app.setLoginItemSettings({
        openAtLogin: openVisible,
        openAsHidden: false,
    })

win.isMinimized()
win.minimize()

win.webContents.getURL()
win.getSize()
win.setSkipTaskbar(true);

监听 win 调整大小，不允许调整到预设最小
win.on('will-resize', (event, newBounds) => {

})

win.webContents.on('render-process-gone' ) 监听程序崩溃

dialog.showMessageBox();

win.reload()
app.exit()

win.webContents.on('did-fail-load')
win.webContents.on('did-finish-load'

win.show()

我现在有了学习的念头，而身材上那些被别人评价的东西，我已经不在乎了，不以物喜不以己悲，度过那个阶段了。
我也不认为忙碌的打工人，面容浮肿有什么打不了了，每个人都在用力的活着，而每个人都可以选择自己喜欢的方式。
我觉得那种获得的愉悦感，没什么大不了的，跟上班的痛苦相比，这点愉悦感微不足道。
所以我更想要足够的财富，让我能够过上轻松的富裕生活。
所以运动，应该是我闲暇之余，一种放松方式而已，我需要跑步带给我的内啡肽，也需要身体有肌肉，让我能够抵御疾病侵扰。
等我工作稳定，每日保持进步，优先享受这些愉悦感，满足感，以后再享受健身带来的好处即可。

  require("@electron/remote/main").enable(win.webContents)

win.on('close' event.preventDefault();
win.isFullScreen()
win.setFullScreen(false)
win.hide()

BrowserWindow.getAllWindows();


tray.destroy()

import { powerMonitor } from 'electron'

powerMonitor.on('lock-screen', () => {
})

import { shell } from 'electron'
shell.openExternal(url).then

electron-builder

autoUpdater

buffer 处理二进制数据，创建缓冲区，读写数据


    app.dock.setIcon('./liqi.jpg') 设置mac dock 栏上的图标

grep global regular expression Linux 中检索指定的字符，返回字符所在的行的所有数据

管道命令操作符，简称管道符，用 | 将多个命令之间隔开，上一个命令的输出作为下一个命令的结果

system_profiler SPHardwareDataType | grep Chip

nodejs 子进程 如果是 Node 进程，用 fork，不是，如果是非流式，用回调函数 exec，execFile，spawn

所以 nodejs 执行命令行，用 exec 即可，exec 和 execFile 的区别是，exec 可以方便执行 shell 命令。

再用 electron 套个壳，就是个很厉害的桌面工具应用了，虽然没有市场，但学习还是有趣的。

electron，桌面端可以做很多有意义的事情，我要好好学，好好积累。

sqlite3
better-sqlite3


swr
data fetching lib