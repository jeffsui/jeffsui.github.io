title: "how git version rollback"
date: 2015-05-16 21:58:30
category: git
tags: [git,gitcafe,版本管理]

---

>如何进行远程版本回退......

#引言

> 一切都要从一个蛋疼的需求开始,老板说,能给远程仓库的版本回退吗？我说为毛？他说我就是试试看git好使不,我......

#咋搞

* 背景 

gitcafe 国内知名的源码托管平台

* 分析

```

1、git checkout the_branch

2、git pull

3、git branch the_branch_backup //备份一下这个分支当前的情况

4、git reset --hard the_commit_id //把the_branch本地回滚到the_commit_id

5、git push origin :the_branch //删除远程 the_branch

6、git push origin the_branch //用回滚后的本地分支重新建立远程分支

7、git push origin :the_branch_backup //如果前面都成功了，删除这个备份分支
```

* 删除远程分支

首先,任何一个git源码托管平台都会告诉你,别删除远程master分支,因为它是默认的分支......,请移步[这里](https://gitcafe.com/GitCafe/Help/wiki/%E5%A6%82%E4%BD%95%E5%88%A0%E9%99%A4-Master-%E5%88%86%E6%94%AF) 

* 操作步骤 

如果远程只有一个master分支,请你创建一个非master分支,然后推送到远程。
有人会问我为什么？打个比方,你见过上旱厕的时候,给自己脚下站着的板子抽走吗？

脚本类似下面这样

```

git branch the_master_backup

git push origin the_master_backup


```

此时你查看远程分支应该有两个:master和the_master_backup

设置默认的分支为 the_master_backup




```

git branch -D branch_name //删除本地master分支

git push :master //推送一个空分支,相当于删除远程master分支

```

然后你在the_master_backup分支上 回滚到你要回滚的commit_id,然后重建master分支并推送到远程,顺便删除the_master_backup分支(包括远程the_master_backup分支)。

```

git checkout the_master_backup

git reset --hard commit_id

git branch master //重新创建master分支

git push origin master //重新推送master分支

git branch -D the_master_backup //删除本地the_master_backup分支

git push origin :the_master_backup//删除远程the_master_backup分支

```

#遇到的问题

1. 忘记设置默认分支为非master分支

```

remote: error: By default, deleting the current branch is denied, because the ne
xt
remote: error: 'git clone' won't result in any file checked out, causing confusi
on.
remote: error:
remote: error: You can set 'receive.denyDeleteCurrent' configuration variable to

remote: error: 'warn' or 'ignore' in the remote repository to allow deleting the

remote: error: current branch, with or without a warning message.
remote: error:
remote: error: To squelch this message, you can set it to 'refuse'.
...

```

#总结

如果你遇到的是所有提交只有master分支,那么希望我这个博文能帮到你。当然git强大的分支功能你基本也用不到了。