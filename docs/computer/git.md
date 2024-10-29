# Git

版本控制工具

git init
git add .
git commit -m 'feat: add test function'

git log

HEAD -> main 当前分支是 main，也就是 HEAD 指针指向了 main，而 main 也是个指针

git branch 查看本地已经有的分支列表

分支是指向 commit 的指针

git log --graph

克隆指定分支：git clone -b dev http://test.git

git reset --hard HEAD^ 回到上一个已提交的版本


git clone test.git -b dev --depth=1

## Config

```bash
git config --global user.name "liqi"
git config --global user.email "test@qq.com"
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```


Mac 克隆账号密码错误，看看钥匙串，删除掉。
git clone -b feat/fix-bug https://test.git --depth=1 —single-branch
