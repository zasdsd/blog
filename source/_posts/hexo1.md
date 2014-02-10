title: 使用Hexo搭建个人博客(1)
date: 2014-02-03 15:12:35
tags: [hexo]
categories: [hexo]
---

存放主题文件，默认主题为`light`
	<!--more-->
### **Hexo简介**

Hexo是一个由Node.js驱动的,简单、快速、强大的Blog框架。可以快速的生成静态网页托管在GitHub、BAE等平台上。对Markdown有很好的支持，并支持从Wordpress、Octopress迁移。
Hexo由台湾地区的开发者编写，他的blog地址请猛击此处，欢迎前去膜拜。<!--more-->

项目主页
https://github.com/tommy351/hexo

使用手册
http://zespia.tw/hexo/docs/

### ** 准备工作**


一颗不安于现状、充满好奇、爱折腾的心！

**安装Git**

	yum install git


**安装Node**

先介绍两个工具

1. nvm 是nodejs version manager的简称，即：nodejs版本管理
2. npm 是nodejs package manager的简称，即：nodejs模块管理

**安装nvm**

	curl https://raw.github.com/creationix/nvm/master/install.sh | sh
	echo ". ~/.nvm/nvm.sh" >> ~/.bashrc

断开终端，再次连接后执行

	nvm install 0.10.24
安装npm

	curl https://npmjs.org/install.sh | sh
使用npm

	npm use 0.10.24
配置默认版本

如果希望断开连接后继续使用，需要输入如下命令

	npm update
	
**配置GitHub**

基本的注册使用就不再赘述，这里所涉及的配置是为了给Hexo发布到GitHub所做准备。
配置ssh key
在服务器上生成ssh key

	cd ~/.ssh
	ssh-keygen -t rsa -C "your_email@example.com"

一路回车即可，执行完毕会生成id_rsa和id_rsa.pub两个文件，将id_rsa.pub中的内容全部复制；
在GitHub上添加Key
进入GitHub - Account settings - SSH Keys - Add SSH key，将刚刚复制的内容全部粘贴进即可；
测试

	ssh -T git@github.com

成功的话会有successfully的提示；
ssh key的详细配置可参考官方文档：
https://help.github.com/articles/generating-ssh-keys
配置自己的github.io页面
登入GitHub后选择Create New repository,Repository name需要符合username.github.io的命名规则。
github.io
github.io

### **基本安装**

Hexo的安装

	npm install -g hexo
	hexo init /data/www/wwwroot/hexo #指定hexo工作目录
	Hexo的使用
	hexo n/new 写文章
	hexo g/generate 生成
	hexo s/server 本地调试，localhost:4000
	hexo d/deploy 发布文章
	Hexo的基本配置
	
Hexo的整体配置是Hexo根目录中的_config.yml文件，大部分配置可以根据提示了解用处，不明白的可以参考手册、百度、Google，此处不再赘述。
如果要发布文章到GitHub，需要修改_config.yml中的Deployment段

	deploy:
	  type: github
	   repository: https://github.com/mayiwei/mayiwei.github.io.git
	   branch: master
	   
其余配置段请参考Hexo使用手册

系统扩展
更换主题
下载主题

	cd hexo/theme
	git clone https://github.com/hexojs/hexo-theme-	light.git light

更多主题可以在https://github.com/tommy351/hexo/wiki/Themes找到。

配置主题

	vim hexo/_config.yml
	theme light
	
### **生成并部署到GitHub**

	hexo g
	hexo d
	
### **安装评论框**

因为Hexo生成的是静态文件，所以评论等动态内容需要使用第三方扩展，这里推荐使用多说。

[注册多说帐号](http://duoshuo.com/)

**配置多说**

vim hexo/theme/light/layput/_partial/comment.ejs
<!-- Duoshuo Comment BEGIN -->
<div class="ds-thread"></div>
<script type="text/javascript">
var duoshuoQuery = {short_name:"mayiwei"};
(function() {
	var ds = document.createElement('script');
	ds.type = 'text/javascript';ds.async = true;
	ds.src = 'http://static.duoshuo.com/embed.js';
	ds.charset = 'UTF-8';
	(document.getElementsByTagName('head')[0] 
	|| document.getElementsByTagName('body')[0]).appendChild(ds);
})();
</script>
<!-- Duoshuo Comment END -->

**生成并部署**

	hexo g
	hexo d
	
**安装RSS插件**

安装

	npm install hexo-generator-feed

配置


	vim hexo/_config.yml
	plugins:
	- hexo-generator-feed
	
	vim hexo/theme/light/_config.yml
	rss: /atom.xml

**生成并部署**

	hexo g
	hexo d
	
**安装sitemap插件**

安装

	npm install hexo-generator-sitemap

配置

	vim hexo/_config.yml
	plugins:
	- hexo-generator-sitemap

生成并部署

	hexo g
	hexo d
	
**安装wordpress迁移插件**

安装插件


	npm install hexo-migrator-wordpress

登录wordpress-》工具-》导出xml
导入

	hexo migrate wordpress source.xml

生成并部署

	hexo g
	hexo d
	
### **安装微博挂件**

去app.weibo.com获取代码

配置

	vim hexo/_config.ynl
	widgets:
	- weibo

	vim hexo/themes/light/layout/_widget/weibo.js
	<iframe width="100%" height="550" class="share_self"  frameborder="0" scrolling="no" src="http://widget.weibo.com/weiboshow/index.php?language=&width=0&height=550&fansRow=2&ptype=1&speed=0&skin=1&isTitle=1&noborder=1&isWeibo=1&isFans=1&uid=1434492110&verifier=f479351c&dpc=1"></iframe>

生成并部署

	hexo g
	hexo d
	
### **安装分享按钮**
去百度分享获取分享代码

配置

	vim themes/light/layout/_partial/article.ejs

删除

<%-partial('post/share')%>

替换为刚刚获取的分享代码
生成并部署

	hexo g
	hexo d
	
**安装百度统计**

去百度统计获取统计代码

配置

	vim themes/light/layout/_partial/baidu_analytics.ejs
	<% if (theme.baidu_analytics){ %>
	<script type="text/javascript">
	#Your_Baidu_Analytics_Code
	</script>
	<% } %>

	vim hexo/themes/light/_config.yml
	baidu_analytics: true


	vim hexo/themes/light/layout/_partial/head.ejs
	
	在/head前添加
	<%- partial('baidu_analytics') %>

生成并部署

	hexo g
	hexo d
	
#### **添加favicon**

制作faciconhttp://www.faviconer.com/

配置
	
	vim hexo/themes/light/layout/_partial/head.ejs
删除
	
	link href="<%- config.root %>favicon.png" rel="icon">
替换为

	<link href="<%- config.root %>favicon.ico" rel="icon" type="image/x-ico">
将制作好的favicon放到hexo/source

生成并部署

	hexo g
	hexo d
添加分类、标签云widget

	vim hexo/themes/light/_config.yml
	widgets:
	- category
	- tagcloud
	
添加友情链接widget
添加配置文件

	vim hexo/themes/light/layout/_widget/blogroll.ejs
	<div class="widget tag">
	<h3 class="title">友情链接</h3>
	<ul class="entry">
	<li><a href="http://mayiwei.com/" title="OPS Notes">OPS Notes</a></li>
	</ul>
	</div>
配置

	vim hexo/theme/light/_config.yml
	widgets:
	- blogroll
	
### **自定义页面**
生成自定义页面

	hexo n page "about"
	
到source/about/index.md编辑内容

添加自定义页

	vim hexo/themes/light/_config.yml

	menu:
	 关于: /about
	 
**自定义404**
使用腾讯公益的404页面，详见：http://www.qq.com/404/
此自定义404页面只对顶级域名生效

	vim hexo/source/404.html
	<html>
	<head>
	<meta charset="UTF-8" />
	<title>404</title>                                                                                                                                       
	</head>
	<body>
	<h1>404 Page Not Found</h1>
	<script type="text/javascript" src="http://	www.qq.com/404/search_children.js" charset="utf-8"></	script>
	</body>
	</html>
	
### **系统优化**
使用自己的域名
建立CNAME文件

	vim hexo/source/CNAME
	mayiwei.com
	
修改你的域名A记录，指向204.232.175.78
如果需要其他二级域名，建立cname记录到usrname.github.io即可。
主页文章显示摘要
编辑md文件的时候，在要作为摘要的文字后面添加<!--more-->即可。

### **生成categories配置项**
在scaffolds/post.md中，添加一行categories:。
同理可应用在page.md和photo.md。

### **关于Node.js**
Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎。目的是为了提供撰写可扩充网络程序，如Web服务。[1]第一个版本由Ryan Dahl于2009年发布，后来，Joyent雇用了Dahl，并协助发展Node.js。与一般JavaScript不同的地方，Node.js并不是在Web浏览器上运行，而是一种在服务器上运行的Javascript服务端JavaScript。


**关于npm命令**

	npm install <name>         # 安装安装nodejs的依赖包
	npm install <name> -g      # 将包安装到全局环境中
	npm install <name> --save  # 安装的同时，将信息写入	package.json中
	npm init                   # 会引导你创建一个	package.json文件，包括名称、版本、作者这些信息等
	npm remove <name>          # 移除
	npm update <name>          # 更新
	npm ls                     # 列出当前安装的了所有包
	npm root                   # 查看当前包的安装路径
	npm root -g                # 查看全局的包的安装路径
	npm help                   # 帮助
	
	
#### **关于Markdown**

Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（John Gruber）和亚伦·斯沃茨（Aaron Swartz）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。[1]这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。

参考

http://bubbyroom.com/2013/08/11/migrate-my-blog-from-wordpress-to-hexo/
http://ibruce.info/2013/11/22/hexo-your-blog/
http://zipperary.com/categories/hexo/
http://yangjian.me/workspace/building-blog-with-hexo/
http://wowubuntu.com/markdown/