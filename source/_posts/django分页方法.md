---
title: django分页方法
date: 2017-03-11 15:48:01
categories: IT技术
tags: django
---
## 1. 使用pure_pagination进行分页

### 1. pure_pagination介绍
[pure_pagination](https://github.com/jamespacileo/django-pure-pagination/)基于并且兼容django原生的pagination模块。

### 2. 安装pure_pagination
```
pip install django-pure-pagination
```

### 3. 配置
- 添加 pure_pagination 到 INSTALLED_APPS
```
INSTALLED_APPS = (
    ...
    'pure_pagination',
)
```
- 
A few settings can be set within settings.py
```
PAGINATION_SETTINGS = {
    'PAGE_RANGE_DISPLAYED': 10,
    'MARGIN_PAGES_DISPLAYED': 2,
    'SHOW_FIRST_PAGE_WHEN_INVALID': True,
}
```
### 4. 代码
- 视图函数
```
from pure_pagination import Paginator, EmptyPage, PageNotAnInteger
class IndexView(View):
    def get(self, request):
        # 文章列表
        article_list = Article.objects.filter()
        # 分页数据
        try:
            page = int(request.GET.get('page', 1))  # 页码
            paginator = Paginator(article_list, 10, request=request)  # 获取有多少页
            article_list = paginator.page(page)  # 获取指定页的数据
        except Exception as e:
            return HttpResponseRedirect('/')
       
        return render(request, 'index.html', {
            'article_list': article_list
        })
```
- 模板
```
<ul class="pagination">
 {# 上一页 #}
 {% if article_list.has_previous %}
    <li class="waves-effect"><a href="?page={{ article_list.previous_page_number.querystring }}"><i class="material-icons">chevron_left</i></a></li>
{% else %}
    <li class="disabled"><a href="#!"><i class="material-icons">chevron_left</i></a></li>
{% endif %}
{% for page in article_list.pages %}
     {% if page %}
         {% ifequal page article_list.number %}
              {# 当前页页 #}
              <li class="active"><a>{{ page }}</a></li>
         {% else %}
              {# 指定页 #}
              <li class="waves-effect"><a href="?{{ page.querystring }}">{{ page }}</a></li>
         {% endifequal %}
     {% else %}
         <li class="waves-effect"><a href="#">...</a></li>
     {% endif %}
{% endfor %}
{# 下一页 #}
{% if article_list.has_next %}
      <li class="waves-effect"><a href="?page={{ article_list.next_page_number.querystring }}"><i class="material-icons">chevron_right</i></a></li>
{% else %}
      <li class="disabled"><a href="#!"><i class="material-icons">chevron_right</i></a></li>
{% endif %}
```

--- 

## 2. 使用django原生的分页方法
使用原生的分页方法要想实现一个比较好的结果需要使用templatetags，有点复杂，下面我们一点一点来
### 1. 首先建立模板标签
```
app/
    templatetags/
        __init__.py
        paginate_tags.py
```
### 2. 编码
- paginate_tags
```
from django import template
from django.core.paginator import Paginator, PageNotAnInteger, EmptyPage
register = template.Library()
// 这是定义模板标签要用到的
@register.simple_tag(takes_context=True)
def paginate(context, object_list, page_count):
    // context是Context 对象，object_list是你要分页的对象，page_count表示每页的数量
    left = 3
    right = 3
    // 获取分页对象
    paginator = Paginator(object_list, page_count)
    // 从请求中获取页码号
    page = context['request'].GET.get('page')
   
    try:
        object_list = paginator.page(page) # 根据页码号获取数据页码对象
        context['current_page'] = int(page) # 将当前页码号封装进context中
        // 获取页码列表
        // pages = paginator.page_range
        pages = get_left(context['current_page'], left, paginator.num_pages) + get_right(context['current_page'], right,                                                                                  paginator.num_pages)
    except PageNotAnInteger:
        object_list = paginator.page(1) # 获取首页数据页码对象
        context['current_page'] = 1
        // pages = paginator.page_range
        pages = get_right(context['current_page'], right, paginator.num_pages)
    except EmptyPage:
        // 用户传递的是一个空值，则把最后一页返回给他
        object_list = paginator.page(paginator.num_pages)
       // num_pages为总分页数
        context['currten_page'] = paginator.num_pages
        // pages = paginator.page_range
        pages = get_left(context['current_page'], left, paginator.num_pages)
    
    context['questions'] = object_list
    context['pages'] = pages  # 页码列表
    context['last_page'] = paginator.num_pages
    context['first_page'] = 1
    // 用于判断是否加入省略号
    
    try:
        context['pages_first'] = pages[0]
        context['pages_last'] = pages[-1] + 1
    except IndexError:
        context['pages_first'] = 1
        context['pages_last'] = 2
    return ''
def get_left(current_page, left, num_pages):
    """
    辅助函数，获取当前页码的值得左边两个页码值，要注意一些细节，比如不够两个那么最左取到2
    ，为了方便处理，包含当前页码值，比如当前页码值为5，那么pages = [3,4,5]
    """
    if current_page == 1:
        return []
    elif current_page == num_pages:
        l = [i - 1 for i in range(current_page, current_page - left, -1) if i - 1 > 1]
        l.sort()
        return l
    l = [i for i in range(current_page, current_page - left, -1) if i > 1]
    l.sort()
    return l
def get_right(current_page, right, num_pages):
    """
    辅助函数，获取当前页码的值得右边两个页码值，要注意一些细节，
    比如不够两个那么最右取到最大页码值。不包含当前页码值。比如当前页码值为5，那么pages = [6,7]
    """
    if current_page == num_pages:
        return []
    return [i + 1 for i in range(current_page, current_page + right - 1) if i < num_pages - 1]
```

- 模板
```
<ul class="pagination">
    {# 判断是否还有上一页，确定是否激活上一页按钮 #}
    {% if questions.has_previous %}
        <li class="waves-effect"><a href="?page={{ questions.previous_page_number }}"><i class="material-icons">chevron_left</i></a></li>
    {% else %}
        <li class="disabled"><a href="#!"><i class="material-icons">chevron_left</i></a></li>
    {% endif %}

    { # 永远显示第一页 #}
    {% if first_page == current_page %}
        <li class="active"><a>1</a></li>
    {% else %}
        <li class="waves-effect"><a href="?page=1">1</a></li>
    {% endif %}

    {# 2以前的页码号要被显示成省略号 #}
    {% if pages_first > 2 %}
        <li class="waves-effect"><a href="#">...</a></li>
    {% endif %}

    {% for page in pages %}
        {% if page == current_page %}
            <li class="active"><a>{{ page }}</a></li>
        {% else %}
            <li class="waves-effect"><a href="?page={{ page }}">{{ page }}</a></li>
        {% endif %}
    {% endfor %}

    {# pages最后一个值+1的值小于最大页码号，说明有页码号需要被省略号替换 #}
    {% if pages_last < last_page %}
        <li class="waves-effect"><a href="#">...</a></li>
    {% endif %}
    
    {# 永远显示最后一页的页码号，如果只有一页则前面已经显示了1就不用再显示了 #}
    {% if last_page != 1 %}
        {% if last_page == current_page %}
            <li class="active"><a>{{ last_page }}</a></li>
        {% else %}
            <li class="waves-effect"><a href="?page={{ last_page }}">{{ last_page }}</a></li>
        {% endif %}
    {% endif %}

    {# 判断是否还有下一页，确定是否激活下一页按钮 #}
    {% if questions.has_next %}
        <li class="waves-effect"><a href="?page={{ questions.next_page_number }}"><i class="material-icons">chevron_right</i></a></li>
    {% else %}
        <li class="disabled"><a href="#!"><i class="material-icons">chevron_right</i></a></li>
    {% endif %}
</ul>
```

### 3. 使用
```
{% load paginate_tags %}
{% paginate article_list 3 %}   # 对应模板标签中的函数paginate(context, object_list, page_count)，context是默认传入的，article_list代表object_list，3代表每页的数量
{% for article in article_list %}
    <section class="post">
        <h1>{{ article.title }}</h1>
        <p>{{ article.content }}</p>
        ...
    </section>
{% include 'pagination.html' %}
```
