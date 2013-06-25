---
layout: post
title: 解决IE6不支持hover的方法
description: ":hover in IE6"
categories: CSS
tags: [：hover, IE6]
---

大多数现代浏览器（FF、Chrome、Safari、IE10等）对伪类选择器`:hover`能提供很好的支持，不管是应用到`<a>`标签还是其他标签，然而IE6及更低版本对`：hover`的支持很不理想，如果一定要在IE6及更低版本中使用`:hover`，可以采用以下方法，下载“csshover.htc”[[压缩版]](http://peterned.home.xs4all.nl/htc/csshover3.htc) [[源码版]](http://peterned.home.xs4all.nl/htc/csshover3-source.htc) [[包含以上两种版本]](http://www.xs4all.nl/~peterned/htc/csshover3.zip)文件，将下载后的文件放在与你想要使用`:hover`选择器的页面的同一级目录中（如果不放在同一级目录则要在样式中注意文件的路径问题）：

	————
	 | --csshover.htc
	 | --index.html

将“csshover.htc”定义在body样式内：

{% highlight css %}
body { behavior:url("csshover.htc"); }
{% endhighlight %}

这样你就能在代码中使用`:hover`了，完整代码如下所示：

{% highlight html %}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
<title>解决IE6不支持hover的方法</title>
<style type="text/css">
	body { behavior:url("csshover.htc"); }
	p.test{width:100px; height:100px; background:#333;}
	p.test:hover{width:200px; height:200px;}
</style>
</head>
<body>
	<p>对class="test"的p元素应用伪类选择器':hover'/p>
	<p class="test"></p>
</body>
</html>
{% endhighlight %}

----------

##### 参考资料
1. [完美解决IE6不支持hover的方法](http://www.w3cfuns.com/thread-347-1-1.html)
2. [Whatever:hover](http://peterned.home.xs4all.nl/csshover.html)
