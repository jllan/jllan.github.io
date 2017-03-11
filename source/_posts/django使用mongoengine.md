---
title: django使用mongoengine
date: 2017-03-11 15:36:01
tags: django
---
## 数据库配置
```
DBNAME = 'Questions'
```

## models
```
# from django.db import models
from mongoengine import *
from QADoorWebDjango.settings import DBNAME

connect(DBNAME)

# Create your models here.
class Questions(Document):

    _id = IntField(primary_key=True)
    url = URLField()
    title = StringField()
    content = StringField()
    is_solved = BooleanField(default=0)
    answer_count = IntField()
    view_count = StringField()
    vote_count = StringField()
    tags = ListField()
    answers = ListField()
    source = StringField()

    meta = {'collection': 'questions'}  # 指明连接数据库的哪张表

for i in Questions.objects[:10]:  # 测试是否连接成功
    print(i._id)
```

## 分页
### 1. 使用mongoengine自带的分页功能
- 视图函数
```
page = request.GET.get('page', 1)
pagination = Questions.objects.paginate(page=page, per_page=PER_PAGE)
return render(request, 'app/index.html', {'questions': pagination.items, 'pagination': pagination})
```
**问题**：这里**Questions.objects**被当成了django orm的**QuerySet**类型，会提示**AttributeError: 'QuerySet' object has no attribute 'paginate'**，暂时还不知道改怎么解决。

- 模板
```
<div class=pagination>
        {% if pagination.has_prev %}
            <li class="waves-effect"><a href="./{{ pagination.prev_num }}" >上一页</a></li>
        {% endif %}

        <li class="active"><strong><a href="">当前页 {{ pagination.page }} of {{ pagination.pages }}.</a></strong></li>

        {% if pagination.has_next %}
            <li class="waves-effect"><a href="./{{ pagination.next_num }}">下一页</a></li>
        {% endif %}
   </div>
```

### 2. 使用django的分页功能
- 视图函数
```
from django.core.paginator import Paginator
question_list = Questions.objects
page = request.GET.get('page', 1)
paginator = Paginator(question_list, PER_PAGE)
questions = paginator.page(page)
context = {
    'questions': questions,
    'total_pages': paginator.page_range
}
return render(request, 'app/index.html', context)
```
- 模板
```
<ul class="pagination">
    {# 上一页 #}
    {% if questions.has_previous %}
        <li class="waves-effect"><a href="?page={{ questions.previous_page_number }}"><i class="material-icons">chevron_left</i></a></li>
    {% else %}
        <li class="disabled"><a href="#!"><i class="material-icons">chevron_left</i></a></li>
    {% endif %}
    {% for page in total_pages %}
        {% if page %}
            {% ifequal page questions.number %}
                {# 当前页页 #}
                <li class="active"><a>{{ page }}</a></li>
            {% else %}
                {# 指定页 #}
                <li class="waves-effect"><a href="?page={{ page }}">{{ page }}</a></li>
            {% endifequal %}
        {% else %}
            <li class="waves-effect"><a href="#">...</a></li>
        {% endif %}
    {% endfor %}
    {# 下一页 #}
    {% questions.has_next %}
        <li class="waves-effect"><a href="?page={{ questions.next_page_number }}"><i class="material-icons">chevron_right</i></a></li>
    {% else %}
        <li class="disabled"><a href="#!"><i class="material-icons">chevron_right</i></a></li>
    {% endif %}
</ul>
```

## 其他问题
### 1. object._id问题
- 问题描述
> 在django模板中使用mongodb对象的**_id**属性时会提示：**Variables and attributes may not begin with underscores: 'value._id'**
- 解决方法
建立模板标签
```
from django import template
register = template.Library()
@register.filter(name='private')
def private(obj, attribute):
    return getattr(obj, attribute)
```
在模板中使用
```
{{ value|private:'_id' }}
```
