---
layout: post
title: "djangotinymce"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#django+tinymce
给django加一个在线编辑器，暂时先用tinymce来做，因为着急要用一下，所以没花太多时间去看configration,以下就是简单的介绍一下吧！
<br />
&emsp;&emsp;在tinymce[官网](http://www.tinymce.com/index.php)下载对应的zip，然后解压。我在django中设置了一个静态文静目录static，然后该目录下建立一个js目录，然后把解压缩的代码扔进去，代码目录是这样 
> *mysite/static/js/tinymce*
关于静态目录设定，是这样的
<br />
{% highlight python %}
STATIC_URL = '/static/'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),       
)
{% endhighlight %}
然后在模板文件里面引用tinymce就可以
{% highlight javascript %}
<script type="text/javascript" src="{{static}}js/tinymce/tinymce.min.js"></script>
<script type="text/javascript">
tinyMCE.init({
    mode : "textareas",
    theme : "modern",
    width: '780px',
    language: 'zh_cn',
    menubar: false,
    plugins: [
    "advlist autolink autosave link image lists charmap print preview hr anchor pagebreak",
    "contextmenu directionality emoticons template textcolor paste fullpage textcolor"
    ],
    toolbar_items_size: 'small',
    toolbar1: "undo redo | bold italic underline strikethrough forecolor | alignleft aligncenter alignright | bullist blockquote link unlink code preview",
});
</script>
{% endhighlight %}
这样就把tinymce引入进来了，然后在html代码里编写的textarea就会自动转换成tinymce。



