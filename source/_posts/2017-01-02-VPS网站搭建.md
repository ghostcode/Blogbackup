title: VPS网站搭建
date: 2017-01-02 21:16:46
categories:
tags:
---

在搬瓦工买了一个VPS只装了一个Shadowsocks用来翻墙，实在是浪费。凑巧我有几个域名，就把搭建网站的简单过程走一遍，学习一下服务端的知识。

刚开始的疑问：

    1.一个VPS(单IP)如何部署多个应用程序（不同的端口，且非80端口）？
    2.二级域名如何配置？
    
需要的东西：

    一个空间和多个域名
    
我首先在空间上跑了一个简单的NodeJS程序，端口3000，然后直接用IP + port 可以访问，然后我就到域名服务商的域名管理那里添加域名解析，将域名解析到对应的IP，然后成功访问。刚开始我以为可以直接在那里添加端口，然后映射到不同程序呢。然后搜了一些资料貌似不是这样的，基本是用Nginx、Apache做转发。
这就基本解决了第一个问题：一个VPS(单IP)如何部署多个应用程序（不同的端口，且非80端口）？

具体如何做呢？(centos)

   1.首先安装Nginx
   
    sudo yum install epel-release
    sudo yum install nginx
    
   2.启动Nginx:
    
    sudo /etc/init.d/nginx start
    
   3.配置Nginx:
   
   
    http{
        server {
            listen 80;
            server_name a.com;
            location / {
                proxy_pass http://localhost:8080;
            }
        }
        server {
            listen 80;
            server_name b.com;
            location / {
                proxy_pass http://localhost:8081;
            }
        }
    }

当请求过来的时候Nginx首先拿到请求，然后根据域名分发到不同的应用服务器（不同端口）

   4.重新加载配置
   
    nginx -s reload

二级域名如何玩呢？

本来以为会很难，其实也很简单。类似上面解析域名的操作，添加一个A记录，然后主机名填你想填的比如：blog、mail、bbs等，然后到Nginx配置一下server_name:

    server {
        listen 80;
        server_name blog.b.com;
        location / {
            proxy_pass http://localhost:8081;
        }
    }

然后就可以访问了！

还有一点如何让NodeJS程序一直跑着？就是我退出终端或程序异常中断等，这里我选择了:PM2，其实还有好多选择 nodemon 、forever 等没有做过对比，有时间在看看区别。

    pm2 start app.js

参考资料：

http://www.west.cn/cms/news/domain/2014-10-22/1404.html

https://segmentfault.com/q/1010000003756513

https://segmentfault.com/q/1010000004112942

http://httpd.apache.org/docs/current/vhosts/examples.html

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-6-with-yum

https://www.npmjs.com/package/pm2