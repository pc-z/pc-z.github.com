---
layout: post
title: 解决IE中图片链接有边框的方法
description: "how to switch off image border in IE"
categories: CSS
tags: [image border]
---

之前的项目中，使用图片作为超链接，在IE浏览器（低版本）中图片会出现边框，严重影响页面的展示，网上查阅了一下，以下代码能解决该问题：

{% highlight css %}
img {
	border: 0 none;
}
{% endhighlight %}
`none`只是去`border-style`，严格来说还应该去`border-width`;

---

##### 参考资料
1. [a标签嵌套img标签IE下会有边框，我知道那是超链接默认的样式，求消除方法！](http://www.oschina.net/question/189375_35017)
2. [How switch off image border in IE](http://stackoverflow.com/questions/2958688/how-switch-off-image-border-in-ie)
