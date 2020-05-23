github简易使用备忘
===
### 将在线代码库拉取到本地
在需要拉取的位置打开终端，使用如下命令可以将代码库拉取到本地：
```
git clone https://XXX
```
如果需要拉取特定分支的特定版本，可以通过相应命令指定：
```
git clone -b [branch name] https://XXX --depth [num]
```
depth决定了拉取最新的N次commit版本。

参考链接：<br>
https://www.cnblogs.com/chenxi188/p/12503468.html<br>
https://segmentfault.com/q/1010000000171057

### 分支操作
通过下面命令可以查看分支情况，加上后缀`-a`或`-r`可以分别查看本地分支情况和远程分支情况：
```
git branch
```
如果需要从当前分支中新建一个分支，可以使用如下命令：
```
git branch [name]
```
之后通过下面命令切换当前分支，即可继续修改：
```
git checkout [name]
```

### 提交和拉取
通过下面命令可以对远程的代码仓库进行提交：
```
git push [远程主机名，origin] [本地分支名] [远程分支名]
```
当本地分支与远程分支存在追踪关系，则可以省略两个分支名。而若当前只有一个远程分支，则主机名也可以省略。<br>
在本地仓库的文件夹中可以通过下面命令从远程代码仓中获取。
```
git pull
```

参考链接：<br>
https://www.jianshu.com/p/2e1d551b8261

### 版本回溯
需要将本地代码回溯到之前的版本，可以先通过下面命令查看所有版本：
```
git log
```
之后可以通过返回的commit id来对代码进行回溯
```
git reset --hard [commit id]  
```

参考链接：<br>
https://blog.csdn.net/qq_28093585/article/details/101465845

### 合并提交
需要将多次提交合并为一次，来缩减commit信息，还是现查询目前的版本：
```
git log
```
之后，找到需要合并提交的前一次提交的commit id，利用如下语句进行合并：
```
git rebase -i [commit id]
```
在输入命令之后，会进入下面vi界面，使用`i`进入插入模式，可以对内容进行修改，将需要合并的commit的前缀改为s(squash，保留commit信息)，或f(fixup，不保留commit信息)。
```
pick e7ba81d Commit-1
pick 5756e15 Commit-2
pick b1b8189 Commit-3
 
# Rebase 5d39ff2..b1b8189 onto 5d39ff2 (3 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
之后按`esc`退出插入模式，输入`:wq`保存并退出，即可完成合并。注意，这一合并一般在提交远程分支之前进行。

参考链接：<br>
https://blog.csdn.net/dietime1943/article/details/85113142