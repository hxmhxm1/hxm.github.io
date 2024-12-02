---
title: nginx
tags:
categories:
  - 网络代理
---

# nginx配置

### 一、是什么

    高性能的开源web服务器软件，也可以用作反向代理服务器、负载均衡器、HTTP缓存以及作为邮件代理服务器，有以下性能
    1、高性能
    2、事件驱动架构
    3、模块化设计
    4、反向代理和负载均衡
    5、静态文件服务
    6、HTTPS支持

### 二、如何设置代理

    1、安装brew install nginx
    2、默认配置文件路径 /usr/local/etc/nginx/nginx.conf，/usr/local/var/www放置静态资源文件，root html就是指向这个文件夹，当需要静态代理时，就将静态资源放到这个文件下；再在nginx.conf文件中配置location 代理即可
    3、location /rms {
        root html
        index index.html index.htm
        try_files $uri $uri/ /index.html  (如果不是hashRouter的话需要配置这一行)
    }, 这里nginx会去查找/usr/local/var/www/html/rms中找到index.html文件

### 三、常用命令

    1、启动nginx： sudo nginx
    2、停止nginx： nginx -s quit/ nginx -s stop
    3、重启nginx： nginx -s reload
    3、如何在mac系统中打开/usr/文件夹，左上角，“前往” -> “前往文件夹”直接查找/usr/文件打开即可
    4、启动vim文本编辑工具
        vim /usr/local/etc/nginx/nginx.conf
        按i 进入插入文本编辑模式
        按esc 退出编辑模式
        :wq退出

### 四、nginx.conf文件内容介绍

    1、event
    2、http 
