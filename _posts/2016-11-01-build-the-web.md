---
layout: post
title:  建立网站过程中遇到的问题
categories: document
tag: 教程
---

* content
{:toc}

问题
------------------------------------

1、执行jekyll server后运行不起来，提示rouge错误，其实是环境没有配好，重新安装了jekyll后OK

2、提示未配置分页，解决方式为在_config.yml中配置

```
# Gems
gems: [jekyll-paginate]
```

3、markdown解释器，由于本模版使用了kramdown解释器（解释数学公式比较快），故而需要安装该解释器，还有一个解释器为rdiscount，可以一并安装上

```
$ gem install jekyll rdiscount
$ gem install kramdown
```

参考网址
------------------------------------

[搭建运行配置全教程](http://alfred-sun.github.io/blog/2014/12/05/github-pages/)


[为了更改logo图片使用的ps抠图教程](http://www.uisdc.com/photoshop-matting-techniques)