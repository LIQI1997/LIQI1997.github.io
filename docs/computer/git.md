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



分支命名规范

查看当前分支跟其他某个分支之间的 diff，可以用 GUI 工具看，比如阿里云的云效平台。


git branch -d branch_name
git branch -D branch_name 强制删除
git push origin -d branch_name 删除远程分支


master commit 1
checkout -b dev ，commit 2
如果 master 直接 merge forwards，可以成功，本质上就是将 master 指针指向了 commit 2 而已。
如果 master 也修改了，指向了 commit 3，这时候再合并，如果 commit 3 和 commit 2 有冲突（修改了同一文件同一行），这时候 master 就没办法直接指向 commit 2 了，冲突状态下，就是未 commit 的情况，需要手动处理。


master 1
dev 2
master 3
dev merge master，就是把在 master 上提交的 commit 3 的代码拉进自己的工作目录，从而 commit 新的版本。

阿里云持续集成 CI，流水线