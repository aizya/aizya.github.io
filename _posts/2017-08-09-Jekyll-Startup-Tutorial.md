---
layout: post
title: 初识Jekyll
date: 2017-08-09
categories: Jekyll
tags: [Jekyll]
description: 简单了解一下Jekyll
published: true
---

### 什么是Jekyll?

简单点来说,就是一个博客生产机器. 咯, 现在你看到的这个所谓的博客就是使用Jekyll+GitHub Pages完成的.

当然啦,你可不要想着这是我一个人一行代码接着一行代码写出来的,那也不可能.

偷偷告诉你,超越CV大法的究级功法,只有一个字: fork. 叉它,往死里把它叉在GitHub上.

言归正传,随意在GitHub上搜索一个比较好看的xxx.github.io的项目,直接fork了,然后自己再修改一下,可不要原样照搬..

为了完成fork之后的简单修改,首先至少也要了解一下Jekyll这玩意吧...

### 关于Jekyll的目录结构

Jekyll的核心其实就是一个文本转换引擎,它的概念就是:使用你最喜欢的标记语言写文章,如Markdown,或者简单的HTML,然后Jekyll就会帮你套入一个或一系列的布局当中去.

不过为了让你的博客显得更加的好看,你需要简单的了解一下Jekyll的目录结构:

一个基本的 Jekyll 网站的目录结构一般是像这样的：

    .
    ├── _config.yml
    ├── _drafts
    |   ├── begin-with-the-crazy-ideas.textile
    |   └── on-simplicity-in-technology.markdown
    ├── _includes
    |   ├── footer.html
    |   └── header.html
    ├── _layouts
    |   ├── default.html
    |   └── post.html
    ├── _posts
    |   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
    |   └── 2009-04-26-barcamp-boston-4-roundup.textile
    ├── _site
    ├── .jekyll-metadata
    └── index.html

那么,上述的这些文件分别都有什么作用呢?

- _config.yml: 保存配置数据
- _drafts: 草稿,一些未发布的文章
- _includes: 你可以加载这部分到你的布局或者文章当中去,以便重用.**可以使用\{\% include file.ext \%\}**(注意吧\省略查看,防止出现symlink,通过不了编译).把_include/file.ext包含进来,一般都是写头文件,底部文件以及导航文件等
- _layouts: layouts（布局）是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。**标签  \{\{ content \}\} 可以将content插入页面中。**
- _posts: 这里放的就是你的文章了。文件格式很重要，必须要符合: **YEAR-MONTH-DAY-title.MARKUP**。如,2017-08-20-初识Jekyll.md.
- _data: 格式化好的网站数据应放在这里。jekyll 的引擎会自动加载在该目录下所有的 yaml 文件（后缀是 .yml, .yaml, .json 或者 .csv ）。
- _site: 一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中.将该目录放进.gitignore之后,git提交的时候就会自动忽略这个文件夹中的内容.
- .jekyll-metadata: 该文件帮助 Jekyll 跟踪哪些文件从上次建立站点开始到现在没有被修改，哪些文件需要在下一次站点建立时重新生成。该文件不会被包含在生成的站点中。
- index.html and other HTML, Markdown,Textile files: 如果这些文件中包含 YAML 头信息 部分，Jekyll 就会自动将它们进行转换。当然，其他的如 .html, .markdown, .md, 或者 .textile 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。
- Other Files/Folders: 其他一些未被提及的目录和文件如  css 还有 images 文件夹，  favicon.ico 等文件都将被完全拷贝到生成的 **site** 中。

以上...
