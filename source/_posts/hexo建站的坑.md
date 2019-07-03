---
layout: w
title: hexo建站的坑
date: 2017-10-18 23:14:05
tags: [hexo]
---
# Hexo的安装与使用 #
这是我的第一篇博客，刚开始想弄jekyll的，但是弄了很久也没配置好，干脆直接弄hexo了。
接下来讲讲我弄hexo的遇到的一些问题吧。
[这是一个我看到的一个比较好的教程](http://www.jianshu.com/p/e99ed60390a8 "20分钟教你使用hexo搭建github博客")
<!-- more -->
大家在配置好后，在本地运行一般没有问题，但是在推到Github上去的时候，

    $ hexo deploy
	Error：Deployer not found: git

我就遇到了这里出错的问题，在网上看了解决办法发现只需要在你的Blog的根目录下运行一下代码
	
	npm install hexo-deployer-git --save
然后再

	$ hexo generate
	$ hexo deploy
然后everything is ok！