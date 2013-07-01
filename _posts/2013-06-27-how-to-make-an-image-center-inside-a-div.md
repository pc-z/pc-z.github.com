---
layout: post
title: DIV中的图片垂直居中
description: "make an image center inside a div"
categories: CSS
tags: [image, center]
---

> 原文链接：[未知高度的图片垂直居中](http://stylechen.com/img-middle.html)

# 前提
图片的宽度和高度是未知的，没有固定的尺寸，在这个前提下要使图片在一个固定了宽度和高度的容器中垂直居中。下图是理想中的效果图，外部容器的宽度和高度固定，中间的图片宽度和高度未知，但是图片要始终要相对于外部的容器垂直居中。![图片垂直居中](http://stylechen.com/wp-content/uploads/2010/12/img_middle.jpg)
但是实际在浏览器中实现起来的效果并不是很完美，由于各浏览器的解析都各不相同，所以在各浏览器都会有1px-3px的偏差。
# 方法一
该方法是将外部容器的显示模式设置成`display: table;`，img标签外部再嵌套一个span标签，并设置span的显示模式为`display: table-cell;`，这样就可以很方便的使用`vertical-align`象表格元素那样对齐了，当然这只是在标准浏览器下，IE6/IE7还得使用定位。
### 结构
{% highlight html %}
<div id="box">
	<span><img src="images/demo.jpg" alt="" /></span>
</div>
{% endhighlight %}
### 样式
{% highlight html %}
<style type="text/css">﻿
	#box {
		width: 500px;
		height: 400px;
		display: table;
		text-align: center;
		border: 1px solid #d3d3d3;
		background: #fff;
	}

	#box span {
		display: table-cell;
		vertical-align: middle;
	}

	#box img {
		border: 1px solid #ccc;
	}
</style>
<!--[if lte IE 7]>
<style type="text/css">﻿
	#box {
		position: relative;
		overflow: hidden;
	}

	#box span {
		position: absolute;
		left: 50%;
		top: 50%;
	}

	#box img {
		position: relative;
		left: -50%;
		top: -50%;
	}
</style>
<![endif]-->
{% endhighlight %}

# 方法二
方法二和方法一的实现的原理大同小异，结构也是相同的，方法一用的是条件注释，方法二就用的CSS Hack。
### 样式
{% highlight html %}
<style type="text/css">﻿
	#box {
		width: 500px;
		height: 400px;
		overflow: hidden;
		position: relative;
		display: table-cell;
		text-align: center;
		vertical-align: middle;
		border: 1px solid #d3d3d3;
		background: #fff;
	}

	#box span {
		position: static;
		*position: absolute; /* 针对IE6/7的Hack */
		*top: 50%; /* 针对IE6/7的Hack */
	}

	#box img {
		position: static;
		*position: relative; /* 针对IE6/7的Hack */
		*top: -50%;
		*left: -50%; /* 针对IE6/7的Hack */
		border: 1px solid #ccc;
	}
</style>
{% endhighlight %}
该方法有个弊端，在标准浏览器下由于外部容器#box的显示模式为`display: table-cell;`，所以导致#box无法使用`margin`属性，并且在IE8下设置边框也无效。

# 方法三
标准浏览器还是将外部容器#box的显示模式设置为`display: table-cell;`，IE6/IE7是利用在img标签的前面插入一对空标签的办法。
### 结构
{% highlight html %}
<div id="box">
    <i></i><img src="images/demo.jpg" alt="" />
</div>﻿
{% endhighlight %}
### 样式
{% highlight html %}
<style type="text/css">﻿
	#box {
		width: 500px;
		height: 400px;
		display: table-cell;
		text-align: center;
		vertical-align: middle;
		border: 1px solid #d3d3d3;
		background: #fff;
	}

	#box img {
		border: 1px solid #ccc;
	}
</style>
<!--[if IE]>
<style type="text/css">﻿
	#box i {
		display: inline-block;
		height :100%;
		vertical-align: middle;
	}

	#box img {
		vertical-align: middle;
	}
</style>
<![endif]-->﻿
{% endhighlight %}

# 方法四
在img标签外包裹一个p标签，标准浏览器利用p标签的伪类属性`:before`来实现，IE6/IE7使用了CSS表达式来实现兼容。
### 结构
{% highlight html %}
<div id="box">
    <p><img src="images/demo.jpg" alt="" /></p>
</div>﻿
{% endhighlight %}
### 样式
{% highlight css %}
#box {
	width: 500px;
	height: 400px;
	text-align: center;
	border: 1px solid #d3d3d3;
	background: #fff;
}

#box p {
	width: 500px;
	height: 400px;
	line-height: 400px; /* 行高等于高度 */
}

/* 兼容标准浏览器 */
#box p:before {
	content: "."; /* <a href="http://casinogreece.gr/">????????????</a> 具体的值与垂直居中无关，尽可能的节省字符 */
	margin-left: -5px;
	font-size: 10px; /* 修复居中的小BUG */
	visibility: hidden; /* 设置成隐藏元素 */
}

#box p img {
	*margin-top: expression((400 - this.height )/2); /* CSS表达式用来兼容IE6/IE7 */
	vertical-align: middle;
	border: 1px solid #ccc;
}
{% endhighlight %}
使用`:beforr`这个方法对于标准浏览器来说比较给力，也没发现有副作用，但是对于IE6/IE7，如果对性能要求较高的话CSS表达式的方法要慎用。

# 方法五
该方法针对IE6/IE7，将图片外部容器的字体大小设置成高度的0.873倍就可以实现居中，标准浏览器还是使用上面的方法来实现兼容，并且结构也是比较优雅。
### 结构
{% highlight html %}
<div id="box">
    <img src="images/demo.jpg" alt="" />
</div>﻿
{% endhighlight %}
### 样式
{% highlight css %}
#box {
	width: 500px;
	height: 400px;
	text-align:center;
	border: 1px solid #d3d3d3;
	background: #fff;
	
	/* 兼容标准浏览器 */
	display: table-cell;
	vertical-align: middle;
	
	/* 兼容IE6/IE7 */
	*display: block;
	*font-size: 349px; /* 字体大小约为容器高度的0.873倍 400*0.873 = 349 */
	*font-family: Arial; /* 防止非utf-8引起的hack失效问题，如gbk编码 */
}

#box img {
	vertical-align: middle;
}﻿
{% endhighlight %}
设置字体大小的方法感觉比较怪异，也没有看到一个合理的解释，只知道图片元素有一些不同于其他元素的特性，但是对于IE6/IE7来说，这个方法还是比较给力的。

思考：很多方法都是依赖于将外部容器的显示模式设置成table才能实现垂直居中，也就是div来模拟table，如果CSS有一个属性来实现这种效果那该多好。如果你也有好的方法，欢迎你来分享。部分方法来自于互联网，由[雨夜带刀](http://stylechen.com/)测试整理。

---
**另外补充[两个方法](http://stackoverflow.com/questions/388180/how-to-make-an-image-center-vertically-horizontally-inside-a-bigger-div)**

# 方法六
用div的背景图片代替&lt;img&gt;标签。
### 结构
{% highlight html %}
<div id="box"></div>﻿
{% endhighlight %}
### 样式
{% highlight css %}
#box {
	background: url(bg_apple_little.gif) no-repeat center center;
	height: 200px;
	width: 200px;
}﻿
{% endhighlight %}
### 优点
代码最简洁，易懂。
### 缺点
不能添加ALT等属性。


# 方法七
创建&lt;table&gt;元素包含&lt;img&gt;标签，使用&lt;table&gt;标签的align和valign属性使图片居中。
### 结构，样式
{% highlight html %}
<div>
	<table width="100%" height="100%" align="center" valign="center">
		<tr>
			<td>
      			<img src="foo.jpg" alt="foo" />
			</td>
		</tr>
	</table>
</div>﻿
{% endhighlight %}
