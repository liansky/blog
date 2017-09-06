---
title: 常用git命令
date: 2017-01-21 14:13:55
tags: git
categories: 版本管理
---

### 创建仓库

``` bash
  git init
```

### 添加更改和查看日志

``` bash
  git status        // 查看修改

  git add file/.    // 添加到缓存区

  git commit -m  "message"  // 提交更改

  git log           // 查看版本

  git log --pretty=oneline   // 版本一行输出

```
### 版本回退

>首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

``` bash 
  git reset --hard HEAD^      // 回退上一版本

  git reset --hard 3628164    // 回退到具体的版本  

  git reflog                  // 用来记录你的每一次命令

```

#### 撤销修改

git checkout -- file可以丢弃工作区的修改

``` bash
  git checkout -- readme.txt

  git checkout . // 撤销工作区所有修改

  git reset HEAD readme.txt  // 可以把暂存区的修改撤销掉（unstage），重新放回工作区

```

#### 删除文件

``` bash
  git rm readme.txt   //  从版本库中删除该文件

```

### 远程仓库
可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

``` bash  
  git clone 你的仓库地址                // 克隆仓库到本地

  git remote add origin 你的仓库地址    // 本地仓库与远程仓库管理

  git push -u origin master   // 关联之后第一次推送，加参数 -u

```

### 分支管理

``` bash
  git branch   // 查看工作区所有分支

  git branch <name> // 创建分支

  git checkout <name>  //切换分支

  git checkout -b <name>    // 创建+切换分支

  git merge <name>    // 合并某分支到当前分支(默认是快进模式)

  git branch -d <name>    // 删除分支

  git branch -D <name>      // 强行删除没有合并的分支，工作修改的东西会丢掉
```

#### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下--no-ff方式的git merge

准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：


``` bash 
  git merge --no-ff -m "merge with no-ff" dev
  Merge made by the 'recursive' strategy.
   readme.txt |    1 +
   1 file changed, 1 insertion(+)
```
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

合并后，我们用git log看看分支历史：


``` bash 
git log --graph --pretty=oneline --abbrev-commit
*   7825a50 merge with no-ff
|\
| * 6224937 add merge
|/
*   59bc1cb conflict fixed
```

### “储藏”工作区

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

``` bash 
  git stash         // “储藏”工作区

  git stash list    // 查看 “储藏”
  
  git stash apply    // 恢复“储藏”工作区

  git stash drop     // 删除“储藏”工作区

  git stash pop      // 恢复并删除“储藏”工作区

```

>工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：


### 多人合作

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是orign。

要查看远程库的信息，用git remote
或者，用git remote -v显示更详细的信息

``` bash 
  git remote

  git remote -v

```

#### 推送分支

``` bash
  git push origin <branch-name>
```

#### 关联本地分支与远程分支

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建
用命令git branch --set-upstream branch-name origin/branch-name。

``` bash
  git branch --set-upstream branch-name origin/branch-name
```

### 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

#### 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上

然后，敲命令git tag <name>就可以打一个新标签：

``` bash
  git tag v1.0
```
可以用命令git tag查看所有标签：

``` bash
  git tag
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了

``` bash
   git log --pretty=oneline --abbrev-commit
```
比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：

``` bash
   git tag v0.9 6224937
```
可以用git show <tagname>查看标签信息

``` bash
git show v0.9
commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 11:22:08 2013 +0800

    add merge
...

```
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

``` bash
 git tag -a v0.1 -m "version 0.1 released" 3628164
...
```
#### 操作标签

``` bash
  git tag -d <tagname>   // 删除标签

  git push origin <tagname>    // 推送标签

  git push origin --tags      //一次性推送全部尚未推送到远程的本地标签

```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除,
然后，从远程删除。删除命令也是push，但是格式如下：

``` bash
  git push origin :refs/tags/<tagname>

```
### 更多信息
[史上最浅显易懂的Git教程！](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)























