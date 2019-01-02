---
layout:     post
title:      python小脚本
category: project
description: 用微信好友头像生成壁纸
---

**说明**

脚本使用python3编写，仅供娱乐。

**源码如下**

```
import itchat
import time
import urllib
import io
from PIL import Image, ImageFilter

WIDTH = 1920
HEIGHT = 1080
CELLSIZE = 120
	

def mixImage():
	friendList = itchat.get_friends(update=True)[1:]
	target = Image.new('RGB', (WIDTH,HEIGHT))
	left = 0
	top = 0
	for friend in friendList:
		try:
			headImgBytes = itchat.get_head_img(friend['UserName'])
			img = Image.open(io.BytesIO(headImgBytes))
			img.thumbnail((120,120))	
			target.paste(img,(left,top))
			left += CELLSIZE
		except:
			print('exception')
			
		if left == WIDTH:
			left = 0
			top += CELLSIZE
			
		if left == WIDTH and top == HEIGHT:
			break;
		
	target.save('./backgroundzzz.png')
					
		
		
if __name__ == '__main__':
	itchat.auto_login()
	mixImage()
```

运行前使用pip3安装相关依赖，安装完成直接运行脚本即可，会在脚本同级目录生成图片，效果如下：

![](http://bucket-zyf.oss-cn-beijing.aliyuncs.com/background3.png)

图片使用PIL调整过高斯模糊，有兴趣可以自行修改

