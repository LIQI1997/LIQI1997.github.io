# Git

版本控制工具


- `git init` 初始化当前目录为一个仓库
- `git add .` 将当前目录所有的文件提交到暂存区
- `git commit -m "feat: add test file"` 提交一个版本
- `git log` 查看所有版本

## 分支

每次 commit，git 都会把修改记录下来，分支就是指向某一次 commit 的指针。

HEAD 就是指向某一个分支的指针，表明当前分支，而效果就是用户磁盘的工作目录里面真实的文件，就会应用那个分支指向的那次 commit 的文件内容 加上文件之前的内容。

比如 HEAD -> main 表明当前分支是 main。

- `git branch` 查看本地已经有的分支列表

- `git clone -b dev http://test.git` 克隆指定分支 dev

- `git clone test.git --depth=1` 只克隆仓库默认分支指向的历史文件和最新的 commit，历史提交记录不克隆

- `git reset --hard HEAD^` 回到上一个已提交的版本，也就是当前分支指向的那个 commit 之前的那个 commit




## Config

```bash
git config --global user.name "liqi"
git config --global user.email "test@qq.com"
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```


Mac 克隆账号密码错误，看看钥匙串，删除掉。

git clone https://test.git —single-branch



分支命名规范

查看当前分支跟其他某个分支之间的 diff，可以用 GUI 工具看，比如阿里云的云效平台。


## 删除分支

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


git log --graph
