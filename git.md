# git

## git初始化

在相对应的文件里输入以下命令进行初始化仓库创建`.git`文件进行git版本控制

```log
git init
```

全局定义github上的账号邮箱

```log
git config --global user.name "yang"
git config --global user.email "yang@XX.com"
```

## 克隆现有的仓库

在你想要创建项目的文件下执行下列命令即可从远程拉取项目下来

```log
git clone http://XXX.XXX/sq-group/XXX.git
```

你也可以通过最后添加额外的参数来指定项目名

```log
git clone http://XXX.XXX/sq-group/XXX.git mylibgit
```

## git分支管理

### git fetch

`git fetch`是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。而`git pull` 则是将远程主机的最新内容拉下来后直接合并，即：`git pull = git fetch + git merge`，这样可能会产生冲突，需要手动解决。

拉取远程最新内容到本地

```log
git fetch <远程主机名>
git fetch origin
```

拉取远程特定分支的更新

```log
git fetch <远程主机名> <分支名>
git fetch origin master
```

### 查看分支

```log
查看远程分支：git branch -r
查看所有分支：git branch -a
查看本地分支的跟踪分支（上游分支）命令：git branch -vv
```

### 切换分支

以远程的origin/develop分支为蓝本，在本地新建一个分支develop，并切换到新建的分支develop，并且建立develop与远程分支origin/develop的跟踪关系(use git pull)

```log
git checkout -b <本地分支名> <远程主机名>/<远程分支名>
git checkout -b develop origin/develop
```

远程分支已被删除可以使用以下命令进行删除本地分支，这个命令只会同步本地和远程有过关联关系的分支

```log
git remote prune origin
```

### 新建、删除分支

新建分支

```log
git branch <分支名>
```

推送分支至远程

```log
git checkout <分支名>    //切换分支
git push <远程主机名> <分支名>:<分支名>
```

删除分支

1.删除本地分支

```log
git branch -d <分支名>
```

2.删除远程分支

```log
git branch -d -r <分支名>  //删除远程分支，删除后还需推送到服务器
git push origin:<分支名>   //删除后推送至服务器
```



### git pull文件发生冲突

git pull文件时发生冲突但还没有commit时，可以通过以下操作进行处理

```log
1.git stash  2. git pull   3. git stash pop
```

### git stash

1.执行存储时，添加备注，方便查找，只有git stash 也要可以的，但查找时不方便识别。

```log
git stash save "save message"
```

2.查看stash了哪些存储

```log
git stash list
```

3.显示做了哪些改动，默认show第一个存储

```log
git stash show
如果要显示其他存贮，后面加stash@{$num},比如第二个
git stash show stash@{1}
```

4.显示第一个存储的改动

```log
git stash show -p
如果想显示其他存存储
git stash show stash@{$num}  -p
比如第二个
git stash show  stash@{1}  -p
```

5.应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储

```log
git stash apply
如果要使用其他个
git stash apply stash@{$num}
比如第二个
git stash apply stash@{1}
```

6.命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash

```log
git stash pop
如果要应用并删除其他stash
git stash pop stash@{$num}
比如应用并删除第二个
git stash pop stash@{1}
```

7.丢弃stash@{$num}存储，从列表中删除这个存储

```log
git stash drop stash@{$num}
```

8.删除所有缓存的stash

```log
git stash clear
```

### 版本回退

1.回退至上一个版本

```log
git reset --hard HEAD
```

2.回退至指定版本

```log
git reset --hard 版本号
```

3.查看以往版本号(本地的commit)

```log
git reflog
```

4.查看各版本号及信息(所有的commit：本地commit + 远程的commit)

```log
git log
```

已经push到远程的版本回退，执行以下命令

```log
// 本地回退到指定的版本
git reset --hard 版本号
//将远程的也回退到指定版本
git push -f <远程主机名> <分支名>
git push -f origin dev
```

> 对于master分支进行推送可能会报错，是因为远程主分支受到保护的分支，你需要获取Masters权限

1.git reset --soft 版本号 

```log
git reset --soft HEAD^  //回到上一个版本
不删除工作区改动的代码，撤销commit，不撤销git add .
```

2.git reset --mixed 版本号 

```log
git reset --mixed HEAD^  //回到上一个版本
不删除工作区改动的代码，撤销commit，撤销git add .
```

3.git reset --hard 版本号 

```log
git reset --hard HEAD^  //回到上一个版本
删除工作区的代码，撤销commit，撤销git add . 回到上一次commit的状态
```

