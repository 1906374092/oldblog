---
layout:     post
title:      nginx配置SSL证书
category: blog
description: 网站变成https
---

最近在做微信小程序开发，由于微信公众平台的强制性要求，服务器域名必须是https开头的，所以配置了一下，做个记录备忘。

首先打开终端，cd 到保存证书的文件夹，输入命令 openssl req -new -newkey rsa:2048 -sha256 -nodes -out yourname.csr -keyout yourname.key -subj "/C=CN/ST=Beijing/L=Beijing/O=zyf Inc./OU=Web Security/CN=*.iloverun.top" 引号内为自己的域名信息

![](https://ooo.0o0.ooo/2017/06/12/593e439be750f.png)

执行命令成功后会在文件夹内生成两个文件.csr和.key。

.csr用于向证书颁发机构申请.crt文件。

SSL证书有好多种，对于个人开发者来说当然使用免费的证书最划算。颁发免费SSL证书的机构有好多，例如百度云，阿里云等提供云服务的机构。但是云服务提供的免费证书不支持下载，自动部署也有很多限制，所以我选择了国外的免费证书申请网站，亲测没有被墙。[https://www.startcomca.com/](https://www.startcomca.com/)

网站以邮箱验证码的方式注册，申请免费证书需要两步操作。

1.验证域名主机  
2.上传csr  
<img src="https://ooo.0o0.ooo/2017/06/12/593e495a9a854.png" width=90%>  

网站提示粘贴csr，把我们的csr拖动到浏览器内用浏览器打开，可以看到csr内容，拷贝粘贴至文本框内，点击submit按照提示下载crt压缩包。
解压后找到nginx目录下的文件，就是我们需要的。  
![](https://ooo.0o0.ooo/2017/06/12/593e4b397c8fc.png)

**配置nginx.conf，证书路径为绝对路径**

``` 
server {  
    listen 80;#也可以不监听80端口 看需要
    listen 443 ssl;
    server_name www.iloverun.top;
    ssl on;
    ssl_certificate /etc/ssl/private/loverun_loc.crt;
    ssl_certificate_key /etc/ssl/private/loverun_loc.key;
}
```
把.csr .crt .key放倒nginx配置的路径下，执行nginx -s reload重新加载配置文件。

到这里SSL配置完成，打开浏览器输入你的https网址，可以看到打开成功，大功告成！！！

