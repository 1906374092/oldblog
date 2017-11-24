---
layout:     post
title:      python爬虫初探
category: blog
description: bs4爬取电子书网站
---

**前言**

最近由于人工智能和机器学习的火热，python这门比java还要古老的语言又重新焕发了青春，这样说其实也不完全准确，因为在国外python的应用领域还是非常广泛的，只不过在国内一直不温不火。但是在网络数据采集方面python一直是主流工具。最近有好多朋友问我有没有电子书资源，我把电子书网站分享出去的时候，想到了用爬虫做批量下载。

**准备工作**

我是在mac环境下操作的，默认python2.7环境。其他平台对应安装python环境即可。然后下载python包管理工具pip。打开终端，在命令行中输入

```
sudo easy_install pip
```

提示安装成功后，使用pip安装这个爬虫库BeautifulSoup：

```
sudo pip install BeautifulSoup
```

提示安装成功后就可以开始爬虫的开发了。对于开发初级爬虫来说，pycharm这样的python IDE实在有些太过庞大，跟推荐使用编辑器开发，我这里是用的是mac平台的CodeRunner，可以直接运行看到结果。

**分析要爬取的网站**

这次我要爬的是一个界面比较简洁的电子书网站，idesks.me。我们可以在浏览器中输入域名看下网站的效果。

![idesks](http://oqedyhjhj.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-11-24%20%E4%B8%8A%E5%8D%8810.48.32.png)

我们看到，这个电子书网站书目编排非常简洁清晰。点击epub按钮即可下载电子书。这种风格简洁的网站对于初级爬虫来说是非常好的选择。下面我们来看下这个网站的源码，右键页面，菜单中选择查看页面源代码。看到的效果是这样的。

![](http://oqedyhjhj.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-11-24%20%E4%B8%8A%E5%8D%8811.03.32.png)

如你所愿这个网站的源码看起来非常整洁。每本书的详细信息包含在section标签内。包括书名，作者，封面url，下载url。结构非常清晰。接下来我们只要用python来拆解这个页面结构，提取出我们需要的信息就行了。

**爬虫编写**
先来看看我们需要做哪些工作。首先，我们需要把网站的完整页面请求下来，这需要用到urllib2,然后我们需要把请求回来的html源码解析出来，提取section标签内的相关信息，这要用到正则表达式，所以要引入re库。最后在需要的信息都提取出来后，我们要把电子书按照书名作建立文件夹，然后把书的封面和书的文件放到文件夹里。到这里就我们的所有逻辑就结束了。是不是很简单。实际操作也非常简洁，不到40行代码就可以完成。下面放代码。

```
#!/usr/bin/python
# -*- coding:utf-8 -*-
from bs4 import BeautifulSoup
import urllib
import urllib2
import os
import re

#爬虫方法
def spider(rooturl):
	response = urllib2.urlopen(rooturl)
	soup=BeautifulSoup(response.read(),'lxml')
#	result=[]
	
	for item in soup.find_all('section'):
		bookname = item.div.h3.string
		bookauthor = item.div.find('p',attrs={'class':'author'}).string
		style = item.article['style']
		imgurl = re.search('\'(.+)\'', style).group(1)
		dirname = bookname + '-' + bookauthor
		fileurl = 'http://idesks.me' + item.div.find('p',attrs={'class':'download'}).find('a',attrs={'class':'epub'})['href']
		
		os.makedirs(dirname)
		#进入书目文件夹
		os.chdir(dirname)
		imgname = dirname + '.jpg'
		filename = dirname + '.epub'
		try:
			urllib.urlretrieve(imgurl,imgname)
			urllib.urlretrieve(fileurl,filename)
		except:
			os.chdir('..')
			continue
		#退出书目文件夹
		os.chdir('..')
		
		print(fileurl)
		
		
#循环爬取前pagecount页		
def runpage(pagecount):
	page = 1
	while page < pagecount:
		print(page)
		rooturl = 'http://idesks.me/?page=%d' % (page)
		spider(rooturl)
		page = page+1


runpage(3)

```

ok，在命令行里cd到bs文件夹下，执行python spider.py。可以在终端中看到正在下载的下载地址。如果你是用CodeRunner直接运行，效果是这样的。
![](http://oqedyhjhj.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-11-24%20%E4%B8%8A%E5%8D%8811.52.36.png)

大吉大利，大功告成！
如过你想要爬取复杂的网站可以系统学习下python爬虫。有很多大型的爬虫框架如scrapy等。找个网站试试吧！