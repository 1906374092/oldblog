---
layout:     post
title:      从“一”开始撸一套系统（一）
category: blog
description: 从技术选型开始
---

**前言**

笔者本身是从事iOS开发的程序猿，众所周知移动端也属于前端的范畴，所以在没有团队，只有自己的情况下就没有办法进行下去了，因为当今这个移动互联时代，几乎没有哪个app不是网络应用，既然是网络应用就一定会用到数据接口，然而就iOS开发来说Objective-C和swift都没办法很好的开发数据接口。于是笔者开始逐步走上了后台接口开发的不归路。


**一个iOS程序员选哪种语言写接口比较好**

站在前端的角度数据就是接口，这其实非常的狭隘。因为一个数据接口公布出来需要的是一整套后台服务的支撑。那么这一整套后台服务我们用什么来开发呢。别急，我们先来看下目前主流的服务都是怎么开发起来的。

1.java

java后台服务目前来说可能是最主流的了，江湖上见得最多的大概就是java程序员了吧，java web的技能图谱如下：

![](http://www.admin10000.com/UploadFiles/Document/201605/13/20160513110847549063.JPG)

对于开发服务来说，我们只需要看后台和数据库两条线就行了。由于数据库不因编程语言而异，所以我们后续再进行讨论。我们直接来看后台，首先就是编程基础，java本身就是面向对象的语言，计算机专业必修课之一，所以相对来说java语言基础比较好上手，尤其对于android开发的童鞋来说，可以直接跳过这一步。serverlet有助于理解java的网络程序的大致运行原理。JDBC是数据库操作，不做过多解释。接下来就是三大网络框架，也就是工业级的开发框架，现在企业多数使用的是spring mvc或spring boot。与之配套的IDE叫STS，是从eclipse针对spring框架修改而来的，对于用惯eclipse的童鞋来说并不难。但是对于iOS的童鞋来说，Xcode已经给惯出毛病来了。所以会觉得不太容易上手。而且框架本身难度相对较大，对于非java系的童鞋相对不容易上手。

2.python

python开发的后台服务目前并不主流，但也并不是没有，据说知乎的整个架构就是python开发的。python语言本身上手很容易，发展也比较成熟，语言本身的历史比java还要古老，近几年由于人工智能和机器学习的火爆，让python也变得火爆起来，我们来看下python的技能图谱：

![](http://oqedyhjhj.bkt.clouddn.com/pythonweb.png)

python网络框架种类繁多，主流的有三个，其中tornado支持异步，据说知乎就是这个框架开发的，flask 和 django 比较flask较轻量化，django不太适合初学python的童鞋，但是功能相对完善，而且会默认带有数据管理后台，但对于企业级应用服务来说，数据管理系统一般都会单独开发一个。python爬虫可以用来抓取数据，对建站有很大帮助。综合来说python上手难度比java容易，但使用规模不大，在网络编程方面属小众。

3.php

php是世界上最好的语言，不解释😅

4.nodejs

javascript本身是动态网页的脚本语言，在谷歌v8引擎之后出现了nodejs。近几年nodejs越来越繁荣，nodejs社区也越来越完善，不断涌现各种优秀的框架。由于nodejs的异步特性，所以node服务对高并发支持比较好。据说天猫双11的服务支持有很大一部分是由node提供的。
我们来看下nodejs的技能图谱：

![](http://oqedyhjhj.bkt.clouddn.com/nodejs.png)

js对于大多数程序员来说都不陌生，无论是java，前端，还是移动端，对于javascript都会有一个基础的掌握。所以从语言角度出发，js是最容易上手的。另外node的异步编程主要基于ES6和ES7标准，所以要对js新语法有一些了解。node的网络框架也相对简单，express属于第一代框架，目前只有一些老项目还在使用，但是对于node入门来说学习express是第一步，koa／koa2是第二代node网络框架，目前为止使用的规模较大，主要是koa2。eggjs是阿里开源的node网络框架，非常的标准化，类似于java的spring，由于推出的时间不长目前使用的规模并不大。

站在前端的角度综合考虑上述几种方案，最终选择使用nodejs的egg框架开发后台服务，学习成本和开发成本以及系统稳定性都可以达到相对满意。

**移动端框架选哪家**

移动端自然不用说，iOS和android肯定各有一个，但是对于一个iOS程序猿来说，写安卓程序学习成本太高，而且有麻烦，要写两个一模一样的业务逻辑，所以最佳方案是选择混合开发的模式。目前来看混合开发有以下几种方案：

1.react native

这是相对来说比较成熟的混合开发方案，发展时间较长，但是上手并不特别容易，用到了JSX，JSX在其他方面用到的地方并不多。

2.Cordova

Cordova改名之前就是phonegap，基于angularjs，嵌入手机浏览器运行，效率不高，实际运行时有卡顿情况。

3.Weex

Weex是由阿里开源的框架，原理是用js调用系统原生控件，所以运行效率较高，支持前端的vue.js框架，熟悉前端的童鞋上手很快。

根据实际运行效果，开发技术的通用性，综合考虑决定使用Weex来开发移动端。Weex开发文档在此 [http://weex.apache.org/cn/guide/](http://weex.apache.org/cn/guide/)


**前端框架二选一**

前端框架主要有两种选择，angularjs和vue.js。这两种框架都支持数据双向绑定，由于移动端使用Weex开发所以前端使用Vue开发，这里主要是用一个集成程度很高的脚手架来搭建后台管理系统页面。vue开发文档 [https://cn.vuejs.org/v2/guide/](https://cn.vuejs.org/v2/guide/), 管理系统脚手架地址 [https://github.com/PanJiaChen/vueAdmin-template](https://github.com/PanJiaChen/vueAdmin-template)
