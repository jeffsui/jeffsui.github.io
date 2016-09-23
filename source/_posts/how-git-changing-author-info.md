title: how git changing author info
date: 2016-09-22 16:39:05
category: git
tags: [git,github]
---
# how git changing author info

## 背景
1. `gitlab`中的统计视图是根据用户的信息统计工作量 
2. 迁移git repo中经常会遇到用户和邮箱不一致的情况

## 解决思路
那么如何修改已经推送到远程的author信息呢?

 `github` 官方提供的建议[如何变更用户信息](https://help.github.com/articles/changing-author-info/)

同时也有类似的项托管在`github`上，[git-tips-blame-someone-else](https://github.com/jayphelps/git-blame-someone-else)

思路基本一致,就是替换提交记录、分支、标签里的author信息。

## 方案 
### 1.打开终端或命令行(`git bash`)
### 2.创建一个你项目的全新裸库
```
git clone --bare https://github.com/user/repo.git
cd repo.git
```
### 3.复制粘贴脚本,并根据你的信息修改下面的变量:
```
OLD_EMAIL
CORRECT_NAME
CORRECT_EMAIL
```
脚本`replace.sh`
```
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

### 4.执行这个脚本
`sh replace.sh`
### 5.察看Git历史有没有错误
`git log `
### 6.强制推送到远程
`git push --force --tags origin 'refs/heads/*'`
### 7.清除repo.git仓库 
```
cd ..
rm -rf repo.git
```
