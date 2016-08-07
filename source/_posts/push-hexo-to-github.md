title: "push-hexo-to-github"
date: 2015-04-17 16:35:31
categories: hexo
tags:

- hexo
- 搭建
- 笔记

---

#如何使用hexo在github上建立静态博客

##环境搭建

1. hexo环境搭建

    请参考 hexo.io [官方站点](http://hexo.io/ "官方站点"),
    **强烈建议**给*基本操作*下的内容快速浏览一遍,下面的操作是我一个一个命令敲出来的,遇到的坑也会记录下来,希望大家能少走弯路。
2. github上建立静态博客
	1. github账号申请（略）
	2. 建立一个github项目
	3. `git clone 项目地址` 到本地
	4. 项目初始化
	<code><pre>
	   cd 项目名
       echo # hexo 实例站点 >> README.md
       git init 
       git add README.md
       git commit -m "first blood"
       git remote add origin 项目地址
       git push -u origin master
	</code></pre>
    5. github免费站点建立规则,请仔细阅读这个规则 
    [https://help.github.com/articles/user-organization-and-project-pages/](https://help.github.com/articles/user-organization-and-project-pages/)
   
    6. 默认github域名
   
    默认github 分配了 一个name.github.io的域名,
    还有一个name.github.io/project_name的二级域名,
    请参照github的提示设置URL。
 
华丽的分割线
---

1. 站点配置流程
	1. 建立站点文件夹,并站点初始化
	2. 安装hexo依赖
	3. 修改node_module/hexo-server/index.js,用于本地调试
	4. 安装hexo-git-deployer插件
	5. 修改全局配置文件_config.yml,配置`deploy`
	<code><pre>
	\# Site 站点配置
	title: 凭海临风的测试江湖
	subtitle:
	description: 凭海临风的博客
	author: Jeff Sui
	language: zh-CN
	timezone
 	</code></pre>

	<code><pre>
	\# URL 站点链接
	\## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
	url: http://jeffsui.github.io
	root: /pinghailinfeng_blog/
	permalink: :year/:month/:day/:title/
	permalink_defaults:
 	</code></pre>
	
	<code><pre>
	\# Deployment
	\## Docs: http://hexo.io/docs/deployment.html
	deploy:
  	type: git
  	repo: https://github.com/jeffsui/pinghailinfeng_blog.git
  	branch : master
  	message :
 	</code></pre>

    *以上配置仅供参考*
	
2. 创建文章
	- 执行`hexo new` 命令
	
	例如:hexo new post my-first-blog

	*将会自动在source/_posts下创建一个my-first-blog.md的文件,使用的是scaffolds下的post.md模板*

    - 修改并保存my-first-blog.md文件

	- 站点生成`hexo g`
	
	- 站点部署 `hexo d`

    如果没有报错,恭喜你已经成功推送到 项目的master分支。

3. 创建gh-pages分支并推送到远程
	<code><pre>
	git fetch origin master
    git checkout gh-pages
    git merge master 
    git push origin gh-pages
	</code></pre>
4. 访问[http://jeffsui.github.io/pinghailinfeng_blog/](http://jeffsui.github.io/pinghailinfeng_blog/ "站点")

##遇到的问题

1. github站点的规则不熟悉,url配置浪费我2个小时。项目建立的是二级域名,所以必须要按照我说的那样配置。
2. 本地预览有可能不加载样式,重新删除`node_module`下所有,执行`npm install`,再`hexo g`,`hexo server -i 127.0.0.1 -s -o`即可。
3. hexo3.0版本的git插件必须要独立安装。
4. 其他坑,自己填吧。

##总结

*大坑各种有,github特别多,还有伟大的墙,兄弟们,github好上,填坑需谨慎！*