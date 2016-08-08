---
title: how to understand git detached HEAD
date: 2016-08-08 13:30:53
tags: [git,版本管理]
---
# 场景

远程有一个develop分支，我想获取到本地,但是clone该项目的时候这个远程分支还没有创建,于是执行 `git checkout commit_id(develop)` 提示如下

```
$ git checkout f7c774b
Checking out files: 100% (357/357), done.
Note: checking out 'f7c774b'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at f7c774b... update jeffsui.github.io
```

## 出现 `detached from ...`
此时用`git branch -av`察看分支
```
$ git branch -av
* (detached from f7c774b) f7c774b update jeffsui.github.io
  master                  6ce1857 Site updated: 2016-08-07 22:09:10
  remotes/origin/HEAD     -> origin/master
  remotes/origin/develop  f7c774b update jeffsui.github.io
  remotes/origin/gh-pages 1eee93f Site updated: 2016-02-13 21:03:46
  remotes/origin/master   6ce1857 Site updated: 2016-08-07 22:09:10
```

所谓的 `detached HEAD` 其实就是`HEAD`指向的是一个`commit`而不指向任何一个`branch`的临时分支,翻译过来就是`游离`.

众所周知,每一个分支都对应了一个commit,`git checkout`其实就是修改`HEAD`文件内容,让它指向不同的分支.

## 如何让`detached HEAD`所处分支指向远程分支

此时的分支你可以执行`commit`操作,但是无法`push`到远程分支。
那么我们如何把游离状态的分支指向我们指定的远程分支呢。
```
$ git fetch origin develop:develop
From https://github.com/jeffsui/jeffsui.github.io
 * [new branch]      develop    -> develop
```

继续执行`git branch -av` 命令查看分支

```
$ git branch -av
* (detached from f7c774b) f7c774b update jeffsui.github.io
  develop                 f7c774b update jeffsui.github.io
  master                  6ce1857 Site updated: 2016-08-07 22:09:10
  remotes/origin/HEAD     -> origin/master
  remotes/origin/develop  f7c774b update jeffsui.github.io
  remotes/origin/gh-pages 1eee93f Site updated: 2016-02-13 21:03:46
  remotes/origin/master   6ce1857 Site updated: 2016-08-07 22:09:10
```

此时我们发现多了一个`develop`分支指向了远程`develop` 分支，这样我们就可以通过命令`git push origin develop:develop`到远程分支了。

## 更简洁的方法

`git fetch origin develop:develop` 

or 

`git checkount -b origin develop:develop` 这样可以直接获取远程分支并创建一个本地分支。


