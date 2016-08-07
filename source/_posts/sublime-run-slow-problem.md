title: "sublime run slow problem"
date: 2015-04-17 14:49:18
categories: 工具篇
tags:

- sublime
- GitGutter

---
#解决sublime Text2运行缓慢的方法

今天打开`sublime`想写博客发现整个页面打开很慢,切换tab要等好几秒。
发现了一个帖子,说`GitGutter`这个插件在st2下会影响切换tab速度。

于是 `ctrl+shift+p`调出命令,remove package,选择`GitGutter`,回车。
重启sublime,世界都变得清静了。
