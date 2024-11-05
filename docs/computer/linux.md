# Linux

## echo

echo print string or variable


## windows 安装 Linux 环境

windows subsystem for linux

open powershell by admin permission
typing wsl --install and waiting for it.



## shell config

设置环境变量，也就是在终端环境可以直接使用的全局变量
power shell 和 Linux shell
Windows
set NAME=liqi



修改 ~/.bashrc
export MYNAME="liqi"
source ~/.bashrc
echo $MYNAME

mac/linux
export NAME=liqi

临时加一个环境变量，直接在终端输入 export name=liqi 即可

Vim ~/.zprofile

Export path=“/test:$PATH”

Source  ~/.zprofile


echo $SHELL
echo $PATH


## glob


约定大于配置的框架
约定式路由怎么做，即在 pages 目录创建一个目录，里面有一个 page.js 文件，那就识别成路由，用 glob 去匹配即可 pages/**/page.js

```js
const glob = require('glob')
 
const paths = glob.sync('pages/**/page.{js,jsx,ts,tsx}')

paths.forEach(item => {
  console.log(item)
})
```

shell 环境也能直接用 glob

```bash
touch {a,b}{1..3}.js
```


Linux 测试磁盘空间 du（disk usage），查看指定目录的大小


df