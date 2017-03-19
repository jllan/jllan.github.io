---
title: 使用nginx+uwsgi来部署django服务
date: 2017-03-19 14:11:53
tags: 
- django
- nginx
---
大体的流程是：nginx作为服务器最前端，负责接收client的所有请求，统一管理。静态请求由Nginx自己处理。非静态请求通过uwsgi传递给Django，由Django来进行处理，从而完成一次WEB请求。
通信原理是：
```
the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django
```

## 1. 安装nginx
```
sudo apt-get install nginx
```

## 2. 安装uwsgi
```
pip install uwsgi
```

## 3. 测试uwsgi
进入django项目根目录
```
uwsgi --http :8001 --plugin python --module blog.wsgi
```
然后访问127.0.0.1:8001检验uwsgi是否正常运行，注意这时项目的静态文件是不会被加载的，需要用nginx做静态文件代理。

## 4. 配置nginx
`vim /etc/nginx/site-enabled/default`
添加以下代码
```
server {
    listen         80;
    server_name    域名;   # 如果没有域名就写ip地址
    charset UTF-8;
    access_log      /var/log/nginx/myweb_access.log;
    error_log       /var/log/nginx/myweb_error.log;

    location / {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8003;
        proxy_read_timeout 600;
    }

    #location /static {
    #    alias static路径;
    #}
}
```
因为我的静态文件直接放在七牛云了，所以这里就不用配置了。

## 5. 启动服务
```
sudo /etc/init.d/nginx restart  # 重启nginx
uwsgi --socket :8003 --plugin python3 --module blog.wsgi  # 启动uwsgi，注意这里是socket而不是http
```

## 6. 出现问题
1. 504 Gateway Time-out 
刚开始客户端请求时总是出现`504 Gateway Time-out`，查看日志发现`connect() failed (111: Connection refused) while connecting to upstream`。网上搜了很多说配置`proxy_read_timeout`，试了都不行，最后终于找到了原因<sup>[3]</sup>，把uwsgi启动时的参数http改成socket就好了，意思是如果nginx配置如果使用了uwsgi_pass指令，则uwsgi协议就只能用socket，具体也不懂。

## 7. 参考
- [1][http://www.cnblogs.com/jhao/p/6071790.html](http://www.cnblogs.com/jhao/p/6071790.html)
- [2][http://www.cnblogs.com/fnng/p/5268633.html](http://www.cnblogs.com/fnng/p/5268633.html)
- [3][http://stackoverflow.com/questions/32492425/trouble-with-using-nginx-with-django-and-uwsgi](http://stackoverflow.com/questions/32492425/trouble-with-using-nginx-with-django-and-uwsgi)
