---
title: 使用ReactNative开发记账app（一）
comments: true
date: 2017-03-17 13:48:15
tags: ReactNative
---

现在网上有很多的记账app，优点是功能特别多，缺点也是功能太多，好多都用不到，而且对于记账的类别分类也是特别的细，用着也不是很习惯。

之前在看一本理财的书，书中提供的有一个记账的excel，用下来挺不错，但是每次都需要打开这个文件去记录，这就很不方便了，因此想到去写一个app。

之所以弄一个app，也是觉得作为一个前端，能使用js写app还是很酷炫的，而且对ReactNative挺感兴趣，感觉这个东西很神奇，就想用下试一试。

使用过程中也是遇到了一些问题，因此就记录下来，用以巩固加深印象。

<img src="http://omy4g53nh.bkt.clouddn.com/1.png" width = "30%" alt="记账app" />

这个就是该app的主页面了，下方的另外3个tab bar可无视。

在这个页面中，主要包括了日期选择的一个控件和一个list view。选择日期后，下方的列表会加载出所选日期对应的数据。

<img src="http://omy4g53nh.bkt.clouddn.com/2.png" width = "30%" alt="删除数据" />

对于每一条数据，可以左滑删除或者点击进入编辑页面(即新增页面)。

<img src="http://omy4g53nh.bkt.clouddn.com/3.png" width = "30%" alt="新增" />
<img src="http://omy4g53nh.bkt.clouddn.com/4.png" width = "30%" alt="新增" />

点击NavBar上的加号(+)，可以进入新增页面。填写完成后点击保存即可回到主页面。

在整个的开发过程中，主要用到了以下技术：

* react native
* react
* redux，单向数据流，状态容器
* express，基于node的web框架，作为后端提供接口
* mongodb，一个基于分布式文件存储的数据库
* mongoose，将数据库中的数据转换为JavaScript对象以供使用，与ORM映射类似

以上的每个框架或库都包括了特别多的内容，这次只使用了一部分，还有更多的内容等待着去学习。

当然，在开发过程中，也学到了很多，加深了对redux、react等的理解，遇到的问题也都得到了解决。


** 如果是独立开发，Google是最好的老师 **

所以作为一名合格的程序员，翻墙是必须要会滴！

[Shadowsocks搭建教程][Shadowsocks搭建教程]

[Shadowsocks搭建教程]:http://lam0114.github.io/2016/05/20/Shadowsocks%E6%90%AD%E5%BB%BA%E6%A0%B8%E5%BF%83%E6%AD%A5%E9%AA%A4/
