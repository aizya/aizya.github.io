---
layout: post
title: Jekyll无法正确解析上传文档
date: 2017-08-21
categories: Jekyll
description: 记录一个Jekyll无法正确显示提交文章的小陷阱
published: true
---

#### 问题描述

之前不是写了一个关于Jekyll目录的简单介绍嘛,因为我使用的是VS Code在本地写好文档,然后直接将写好的文件提上去.

但是我发现,无论当我提交了之后,博客上并没有文章增加! 正常情况下,应该更新完之后的一两分钟之内,文章就应该正常显示出来.

也就是说: **Jekyll无法正确解析并显示提交的文章**

更加奇怪的是,无论我如何修改,甚至于在**已经提交的文章**上进行修改,博客上的文章没有任何变化(包括直接在_post目录中删掉已经上传的文章...)

这就让人有点恶心了...

本来这个博客就是直接fork别人的,现在还莫名其妙的出了这样的问题...

让我一度想要重新fork一遍,但是，苦于实在是要修改太多配置的东西了...

#### 问题经过

在我实在不知道怎么办的时候,我就开始玩手机.. 这时候,我才发现一点小情况...

每个GitHub帐号不是自动绑定一个邮箱号嘛? 我发现绑定的邮箱上面突然多了很多邮件（其实一直都会发的，只是我没有注意过）,修改pressmore.github.io里面的内容也会来一封,出现了错误也会来一封...

所以我就看到了下面这种情况:

![邮箱中出现的GitHub提示邮件:]({{ site.url }}/assets/Selection_229.png)

邮件中提示我，我提交的post文档里面存在问题，无法进行解析，不仅该篇文章不能解析，在此之后的所有文章都不能解析，只有你解决好了所有的情况：

![邮件提示文档的时间格式不正确:]({{ site.url }}/assets/Selection_230.png)

![邮件提示文档的中出现了不规范的include调用]({{ site.url }}/assets/Selection_231.png)

之所以出现上面的提示，是因为我在文档中，无意中使用了Jekyll的语法，但是却没有正确的使用，所以出现了错误．．

![]({{ site.url }}/assets/Selection_232.png)

当然，除此之外，还有一个地方给我们提示了相应的错误，那就是GitHub对应的仓库Setting中．如果没有问题会是下面这种情况，但是出了问题，会提示具体问题出在哪里．．

![]({{ site.url }}/assets/Selection_233.png)

#### 问题总结

Jekyll未正常解析Post文档的原因有很多:

1. 该文档没有放在_post目录下

2. 该文档的标题不正确，应该严格使用YEAR-MONTH-DAY-title.MARKUP格式．

3. 文档的使用的日期比当前的日期早,应该要在_config.xml文件中配置future:true.

4. 该文档的published设置为false,默认不发表出来.将其修改为true.

5. 标题含有其他特殊字符,如冒号,应该使用&#58进行替代.

6. 文档包含其他格式错误(这个很广泛).

#### 参考链接

1. <a href="http://jekyllcn.com/docs/posts/"> Jekyll中文社区</a>

2. <a href="https://stackoverflow.com/questions/30625044/jekyll-post-not-generated"> Jekyll Post未正常生成原因</a>

3. <a href="http://jekyllrb.com/docs/templates/#linking-to-posts"> Jekyll如何链接其他文档</a>

4. <a href="https://www.digitalocean.com/community/tutorials/controlling-urls-and-links-in-jekyll">如何控制Jekyll的链接</a>
