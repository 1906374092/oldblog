---
layout:     post
title:      nginx配置负载均衡
category: blog
description: nodejs监听多端口模拟多服务器环境
---
如果还没有nginx和node环境，在mac环境下可以使用homebrew包管理工具安装

```
 brew install homebrew/nginx
```

```
brew install node
```
linux及windows环境使用对应包管理工具或直接下载安装。

###编写nodejs服务端文件
分别编写server8881.js , server8882.js , server8883.js , 代码如下，修改对应端口号即可。

```
var http = require('http');
http.createServer(function (request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('server 8881');
}).listen(8881);
// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8881/');
```
打开三个终端窗口，cd到server.js保存目录，分别执行node server8881.js , node server8882.js , node server8883.js命令。
终端显示Server running at http://127.0.0.1:8881。

###配置nginx.conf
找到nginx.conf用编辑器打开，在http模块下加入如下代码。保存并在终端执行nginx -s reload。

```
    upstream localhost{
      server 127.0.0.1:8881;
      server 127.0.0.1:8882;
      server 127.0.0.1:8883;
    }
    server{ 
        listen 8088; 
        server_name localhost; 
        location / { 
            proxy_pass         http://localhost; 
            proxy_set_header   Host             $host; 
            proxy_set_header   X-Real-IP        $remote_addr; 
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for; 
        } 
    }
```
server模块为主服务器监听8088端口，proxy_pass将请求发送到node服务，upstream模块配置负载主机。

###测试
打开浏览器在地址栏输入http://localhost:8088/回车，浏览器显示 “server 端口号”，刷新几次发现端口号随机更改，nginx配置成功！！！！