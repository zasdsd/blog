title: hexo学习
date: 2014-02-04 21:14:01
tags: [hexo]
categories: [hexo]
---

# themes

存放主题文件，默认主题为`light`
<!--more-->



**languages:** 语言配置文件

**layout:** 布局文件，该文件加内容有待研究，

_`_partial`_ 为网页每个部分的布局，如`header`博客的头部进行配置；`comment`用于设置评论配置；`google_analytics`用于配置google分析工具；`after_footer`貌似用于每篇文章打开的配置，这里可以将mathjax加载到其中。

_`_widget`_ 为页面添加模块，里面包括`category`（文章分类）、 `recent_posts`（最近文章） `search`（搜索） `tag（标签）` `tagcloud`（标签云） ，而加载这些widget需要在theme的`_config`中进行配置。

其他文件还不大明白

**source:** 存放资源，有待研究。不过可以吧一些图片文件放在该文件夹下，如博客的大小图标，
<a id="more"></a>

# source

**_dtafts:** 保存草稿位置，生成命令：`hexo new draft "draftName"`
                            发布草稿命令：`hexo publish "draftName"`

**_posts:** 保存发布的文章，生成命令：`hexo new post "postName"` 或者`hexo new "postNam"`

**about:** 用户生成的关于page ，生成命令：`hexo new page "newPageName"`
                在生成about目录后about中会自动生成一个文件`index.md`
                为了使生成的page可以显示、使用，需要在主题的配置文件即`themes\light\_config.yml`中在`menu`中添加`About: /about`前面的`About`为显示名称，后面的存储名称，

# script

不清楚有啥用，从名字上来看应该是存放脚本文件的

# scaffolds

创建`draft` `post` `page` `photo`时用于生成头文件
