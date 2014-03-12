---
layout: post
title: "django pagination实践+总结"
description: ""
category: 
tags: []
---
{% include JB/setup %}
简单记录一下django分页的用法，django自带分页功能pagination，不过用起来稍微有点小复杂，所以个人建议用django-pagination扩展，还是很方便的！<br />
其[官方](https://pypi.python.org/pypi/django-pagination)有一个很清晰的使用例子，以下简单的记录以下：<br />
1. 将源文件下载后，解压，然后在目录中python setup.py install,然后将解压目录中的pagination目录拷贝到项目根目录下
2. 修改项目setting.py文件，需要添加或修改的内容如下
{% highlight python %}
INSTALLED_APPS = (
	'pagination',
)
MIDDLEWARE_CLASSES = (
	'pagination.middleware.PaginationMiddleware',
)
TEMPLATE_CONTEXT_PROCESSORS = (
    "django.contrib.auth.context_processors.auth", 
    "django.core.context_processors.debug", 
    "django.core.context_processors.i18n",
    "django.core.context_processors.media",
    "django.core.context_processors.request"
)
{% endhighlight %}
3. 模板中添加如下内容（以下内容中，请把$$换成%，因为{和%凑在一起和markdown语法冲突）
- 模板顶部添加<br />
{$ load pagination_tags $}；
- 处理需要循环的数据变量<br />
{$ autopaginate 变量名称 $} or {$ autopaginate 变量名称 10 $}<br />    
注：这里的10代表每页显示条目,这句写好后，后面就可以开始循环输出变量了<br />
- 显示分页页码<br />
{$ paginate $} 注：如果页数小于2的话，这个不会显示
4. 其他的选项，可以在setting.py的配置最后一想中设置
> PAGINATION_DEFAULT_PAGINATION         	每页显示数量
>
> PAGINATION_DEFAULT_WINDOW			分页显示在当前页左右两边的页数
>
> PAGINATION_DEFAULT_ORPHANS		最后一页显示的最小页数，默认为0
>
> PAGINATION_INVALID_PAGE_RAISES_404		当页数不存在时，是否显示404页面
<br />
至此，简单的分页就做完了。