---
title: Flask介绍
date: 2016-12-08 14:45:58
categories: IT技术
tags: flask
---

# 1. 简介
Flask是一个使用Python编写的轻量级Web应用框架。基于Werkzeug WSGI(*PythonWeb服务器[网关](http://baike.baidu.com/view/807.htm)接口（Python Web Server Gateway Interface，缩写为WSGI*)是Python应用程序或框架和Web服务器之间的一种接口，已经被广泛接受, 它已基本达成它的可移植性方面的目标)工具箱和Jinja2 模板引擎。 Flask使用BSD授权。 Flask也被称为“microframework”，因为它使用简单的核心，用extension增加其他功能。Flask没有默认使用的数据库、窗体验证工具。然而，Flask保留了扩增的弹性，可以用Flask-extension加入这些功能：ORM、窗体验证工具、文件上传、各种开放式身份验证技术。

**小插曲**
Flask 的作者是 Armin Ronacher（他也是 Werkzeug 及 Jinja2 的作者。）。本来只是作者的一个愚人节玩笑，不过后来大受欢迎，进而成为一个正式的项目。

***

# 2. Flask的安装
和大多数的Python第三方模块的安装方法一样，Flask可以直接通过pip来安装
```
pip install flask
```

***

# 3. 一个最小的Flask应用

 - **代码**

```
from flask import Flask
app = Flask(__name__)
 
@app.route("/")
def hello():
    return "Hello World!"
 
if __name__ == "__main__":
    app.run()
```

- **运行**

```
python hello.py
```

![深度截图20161208135148.png](http://upload-images.jianshu.io/upload_images/1713353-62f8d07213068e6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![深度截图20161208135255.png](http://upload-images.jianshu.io/upload_images/1713353-6c8a2410a5ef26f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



- **代码解释**

> 首先我们导入了 [Flask](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 类。
接着我们创建了这个类的实例。第一个参数是应用模块或者包的名称。
然后我们使用 [route()](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask.route) 装饰器来告诉 Flask 触发函数的 URL 。
函数名称可用于生成相关联的 URL ，并返回需要在用户浏览器中显示的信息。
最后，使用 [run()](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask.run) 函数来运行本地服务器和我们的应用。 if __name__ =='__main__':
 确保服务器只会在使用 Python 解释器运行代码的 情况下运行，而不会在作为模块导入时运行。
按 control-C 可以停止服务器

***

# 4. Flask 框架分析
为了理解 Flask 框架是如何抽象出Web开发中的共同部分，我们先来看看Web应用程序的一般流程。对于Web应用来说，当客户端想要获取 **动态资源** 时，就会发起一个HTTP请求（比如用浏览器访问一个 URL），Web应用程序会在后台进行相应的业务处理，（从数据库或者进行一些计算操作等）取出用户需要的数据，生成相应的HTTP响应（当然，如果访问静态资源，则直接返回资源即可，不需要进行业务处理）。整个处理过程如下图所示：
![](http://upload-images.jianshu.io/upload_images/1713353-dbf7be5e3c31ffda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
实际应用中， **不同的请求可能会调用相同的处理逻辑** 。这里有着相同业务处理逻辑的 HTTP 请求可以用一类 URL 来标识。比如论坛站点中，对于所有的获取Topic内容的请求而言，可以用 topic/<topic_id>/ 这类URL来表示，这里的 topic_id 用以区分不同的topic。接着在后台定义一个 get_topic(topic_id) 的函数，用来获取topic相应的数据，此外还需要建立URL和函数之间的一一对应关系。这就是Web开发中所谓的 **路由分发** ，如下图所示：
![](http://upload-images.jianshu.io/upload_images/1713353-dfb1a47e078dbd20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Flask底层使用 werkzeug 来做路由分发，代码写起来十分简单，如下：
```
@app.route('/topic/<int:topic_id>/') 
def get_topic(topic_id):
  # Do some cal or read from database
  # Get the data we need.
```

通过业务逻辑函数拿到数据后，接下来需要根据这些数据生成HTTP响应（对于Web应用来说，HTTP响应一般是一个HTML文件）。Web开发中的一般做法是提供一个HTML模板文件，然后将数据传入模板，经过渲染后得到最终需要的HTML响应文件。
一种比较常见的场景是， **请求虽然不同，但响应中数据的展示方式是相同的** 。仍以论坛为例，对不同topic而言，其具体topic content虽然不同，但页面展示的方式是一样的，都有标题拦，内容栏等。也就是说，对于 topic 来说，我们只需提供一个HTML模板，然后传入不同topic数据，即得到不同的HTTP响应。这就是所谓的 **模板渲染** ，如下图所示：
![](http://upload-images.jianshu.io/upload_images/1713353-07146b9e837d26cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Flask 使用 Jinja2 模板渲染引擎来做模板渲染，代码如下：
```
@app.route('/topic/<int:topic_id>/')
def get_topic(topic_id):
  # Do some cal or read from database
  return render_template('path/to/template.html', data)
```

总结一下，Flask处理一个请求的流程就是，首先根据 URL 决定由那个函数来处理，然后在函数中进行操作，取得所需的数据。再将数据传给相应的模板文件中，由Jinja2 负责渲染得到 HTTP 响应内容，然后由Flask返回响应内容。

***

# 5. Flask的扩展
Flask是一个微型框架，自身没有提供数据库管理，表单验证，cookie处理等功能，很多功能需要通过扩展才能实现
>- **Flask-sqlalchemy**   # 数据库管理
- **Flask-script**              # Flask-Script 为Flask程序添加了一个命令行解析， 自带了一组常用选项，而且还支持自定义命令。
- **Flask-wtform**           # 表单验证
- **Flask-login**              # 登陆管理
......

可以在这个网站上寻找更多Flask的扩展的安装和使用方法
[http://flask.pocoo.org/extensions/](http://flask.pocoo.org/extensions/)

***

# 6. 主页
更多关于Flask的内容请到Flask的官网
[http://flask.pocoo.org/](http://flask.pocoo.org/)
