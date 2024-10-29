# Node.js

export NODE_OPTIONS=--openssl-legacy-provider

process.platform; //  'darwin'
process.arch; // 'arm64'
os.arch(); // 'arm64' 'x64'
os.hostname(); // 'liqideMacBook-Air.local'
os.release(); // 获取os 版本号
process.cwd() 当前项目的根路径

nodejs 以 utf-8 编码读取文件，读出来的是字符串，再通过 json.parse 反序列化成js对象。

nodejs 全局对象 global

console.log(__dirname) // 当前运行的文件所在的目录的绝对路径
console.log(process.cwd()) // 效果同上
console.log(__filename) // 跟上面一样，但包含当前运行的文件名

const si = require('systeminformation')
const systemData = await si.system();
const biosData = await si.bios();
const baseboardData = await si.baseboard();
const chassisData = await si.chassis();

os.release() // get os version, such as windows: 10.0.19045
parseInt(os.release()) // get main version: 10

console.log(process.env.love); 
// process 是全局变量，表示当前进程
// env 是包含用户的环境变量，比如在 macOS 上的 shell 里面 export name=liqi 就可以通过上面的语句获取到
// 环境变量都会被隐式转换成字符串

cross-env
一个 nodejs 包
由于在 Windows cmd/powershell，macOS 上的 zsh，设置环境变量的方法是不一样的，zsh 是用 export，cmd 是用？
所以为了跨平台，这个库可以让你可以用一种写法设置用户环境变量，在 package.json 中定义好运动脚本的 script，通过 npm run 执行，比较优雅

环境变量，就是操作系统的变量
"scripts": {
    "dev": "cross-env love=song node test.js"
  },

FFI
foreign function interface 
js 调用外部的 C/C++ 编写的库的技术，加载动态链接库：.dll .so .dylib，声明函数，调用函数
node-ffi 比较老
koffi 比较新
ffi-napi

const fn = ffi.Library(path, {
    test: [ 'string', [ 'string', 'string']]
})
const res = fn.test('1', '2');


拼接路径，避免硬编码路径，导致在其他操作系统上不支持，提高代码可移植性
const path = require("path");
console.log(path.join(__dirname, './test/build'))


流：在有限的内存里操作海量的数据，在Linux中广泛应用。
比如 cat 一个 2G 的 log file，会爆内存，可是通过管道符以流的形式，就没问题。

nodejs stream
readable 可以读取数据的流 fs.createReadStream
writable
duplex 可以读写数据的流，如 net.socket
transform 双工流的一种特殊模式，与 duplex 的区别是可以对数据进行加工， zlib streams，crypto streams


child_process
创建子进程的好处：不影响主进程事件循环，不影响程序响应性，因为如果一个任务计算密集，占据大量 cpu，很适合单独创建一个子进程
常用的场景：可以用来跟外部通信，比如网络服务，运行 shell 脚本

创建子进程有4种方式，用来执行不同的任务
exec
execFile
spawn
fork 适合用来执行 Node.js 的脚本




Buffer.allocUnsafe(100) 分配100个byte 的内存池
dns.lookup(host, 4, callback) 获取域名的 ip



JWT

JSON Web Token

跨域认证解决方案

用户认证流程









if (fs.existsSync(path)) {
        return fs.readFileSync(path, 'utf8')

npm-run-all

child_process

spawn 
fork 只能启动 nodejs 进程


compressing 将文件或文件夹压缩成 zip, gzip 等格式，解压缩。
zip 比较通用，gzip 不能压缩文件夹。


nodejs 如何判断端口被占用？通过 net.createServer 创建 http 服务，监听端口，如果被占用，那就会触发错误事件监听，如果没被占用，那就会开启成功，这时候直接停服即可。



node-machine-id 纯 js 实现的，根据 mac 地址和 cpu 序列号等信息生成的设备标识符。
const { machineIdSync, machineId } = require('node-machine-id')

const id = machineIdSync();

console.log('id',id)

machineId().then(res => console.log('then', res === id))

lodash
import { get, set } from 'lodash'

const user = {
    address: {
      city: 'shenzhen'
    }
  }

  const city = get(user, 'address.city')
  const name = get(user, ['name']) // undefined
  const city2 = get(user, ['address', 'city'])

  set(user, 'arr[0].location.title', 'test street')
  set(user, ['arr', '0', 'location', 'title'], 'test value')

同时启动两个程序，并行执行。
"scripts": {
    "dev": "concurrently 'node test.js' 'node index.js'",
  },

colors 在控制台打印日志时，可以用不同颜色区分。
const colors = require('colors')

console.log(colors.blue.underline('Song Xintong'))
console.log(colors.bgRed('宋新桐'))


npm i shelljs，执行 shell 脚本
const shell = require('shelljs')

shell.rm('-r', 'test/') // 删除 test 目录

systeminformation
const si = require('systeminformation')

// si.inetChecksite('https://liqi.space').then(res => {
//     console.log('res',res)
// })

si.networkInterfaces().then(res => {
    const net = res.filter(item => item.default)[0];
    console.log('res', net.type, net.mac)

    if (net.type === 'wireless') {
        si.wifiConnections().then(res => {
            console.log('wifi', res)
            console.log('id',  res[0].ssid);

            // 通过定时器去监听网络状态，监听net.type 是否变化，从以太网切换到WiFi，通过监听 wifi.ssid 是否变化来判断用户是否更换了WiFi

            // 通过 判断 net 的 operstate 是否为 'up' 或 'down' 来判断网络是否离线。
        })
    }
})



array-move 将数组元素移动到指定位置
import {arrayMoveImmutable} from 'array-move';

const input = ['a', 'b', 'c'];

const array1 = arrayMoveImmutable(input, 1, 2);
console.log(array1);
//=> ['a', 'c', 'b']

const array2 = arrayMoveImmutable(input, -1, 0);
console.log(array2);
//=> ['c', 'a', 'b']

const array3 = arrayMoveImmutable(input, -2, -3);
console.log(array3);
//=> ['b', 'a', 'c']

process 对象提供了 nodejs 进程的信息，可以控制进程
process.argv 返回一个数组，包含启动 nodejs 进程时传入的命令行参数。

nodejs 16 起，支持 ES6 Module，需要将 package.json type 改成 module：import test from 'test'

ffi-napi 是一个 nodejs 库，可以让 nodejs 调用 C/C++ 编写的动态链接库（dll）中的函数。


yargs 是一个处理 nodejs 命令行参数的库
const { argv } = require('yargs')

const { name, age } = argv;

console.log(name, typeof name) // string
console.log(age, typeof age) // number

node index.js --name=liqi --age=18



spark-md5 生成 md5 值
const SparkMD5 = require('spark-md5')

const hexHash = SparkMD5.hash('hello')

console.log(hexHash, hexHash.length)

cross-spawn 是一个跨平台执行命令的包，解决了不同平台使用不同的命令行工具，要写不一致的语法的问题，让开发人员可以写一致的脚本。
const spawn = require('cross-spawn')

const childProcess = spawn('ls', ['-l'])


childProcess.on('close', code => {
    console.log('code is', code)
})


jwt-decode
解码 token，不验证，只解码
import { jwtDecode } from 'jwt-decode'

const token = 'eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoibGlxaSIsImFkbWluIjp0cnVlLCJpc3MiOiJsaXFpIiwiZXhwIjoxNzI5MjY3MjAwLCJzdWIiOiJvayIsImF1ZCI6IjY2IiwibmJmIjoxNzI5MTgwODAwLCJpYXQiOjE3MjkwOTQ0MDAsImp0aSI6IjEyMyJ9.w1THveyEzs9CjuRF3YL4K66JqSCi1a-qBMopYj2by_Kzj43BwwxbQX4y5N1GlgZc7x2w3_4zeQJq7-JNmlSHww'

// const decoded = jwtDecode(token, {header: true})

const decoded = jwtDecode(token)

console.log(decoded)



一个命令行交互的库
import inquirer from "inquirer";

inquirer.prompt([{
    type: 'input',
    name: 'projectName',
    message: '项目名：'
},{
    type: 'list',
    name: 'language',
    message: '语言：',
    choices: [{
        name: 'JS',
        value: 'js'
    }, {
        name: 'TS',
        value: 'ts'
    }]
}, {
    type: 'password',
    name: "code",
    message: '密码：',
    mask: '*'
}, {
    type: "confirm",
    name: 'flag',
    message: '是否继续？',
}]).then(answers => {
    console.log('answers', answers)
}).catch(error => {
    console.log(error)
    if (error.isTtyError) {
        console.log('tty error')
    }
})


dns.lookup(host)

ping -t windows

自定义一个 event emitter 对象
本质上就是注册监听，当发送消息的时候，执行监听的回调函数

class EM {
  constructor() {
    this.events = {}
  }

  on(event, callback) {
    const listeners = this.events[event] || []
    listeners.push(callback)
    this.events[event] = listeners
  }

  emit(event, message) {
    const listeners = this.events[event] || []
    listeners.forEach(callback => {
      callback(message)
    });
  }
}

const em = new EM()

em.on('hello', (msg) => {
  console.log('get message', msg)
})

em.on('hello', (msg) => {
  console.log('son get message', msg)
})

em.emit('hello', 'you')
em.emit('hello', 'you2')

## nvm
node version manger

nvm list
nvm install 18
nvm use 18.20.4

node -v
npm -v