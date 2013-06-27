---
layout: post
title: 如何将页脚固定在页面底部
description: "how to keep footers at the bottom of the page"
categories: CSS
tags: [sticky footer]
---
> 原文链接：[如何将页脚固定在页面底部](http://www.w3cplus.com/css/css-sticky-foot-at-bottom-of-the-page)

作为一个Web的前端攻城师，在制作页面效果时肯定有碰到下面这种现象：当一个HTML页面中含有较少的内容时，Web页面的“footer”部分随着飘上来，处在页面的半腰中间，给视觉效果带来极大的影响，让你的页面看上去很不好看，特别是现在宽屏越来越多，这种现象更是常见。那么如何将Web页面的“footer”部分永远固定在页面的底部呢？注意了这里所说的是**页脚footer永远固定在页面的底部，而不是永远固定在显示器屏幕的底部**，换句话说，就是当内容只有一点点时，Web页面显示在浏览器底部，当内容高度超过浏览器高度时，Web页面的footer部分在页面的底部，总而言之**Web页面的footer部分永远在页面的底部，换句说，Footer部分永远沉底**。如下图所示：![固定页脚](http://www.w3cplus.com/sites/default/files/stickyfooter.png)

那么今天主要和大家一起学习如何将页脚固定在页面的底部？

# 方法一
首先我们来看第一种方法，这种方法是来自于[Matthew James Taylor](http://matthewjamestaylor.com/about)的[《How to keep footers at the bottom of the page》](http://matthewjamestaylor.com/blog/keeping-footers-at-the-bottom-of-the-page)。下面我们就一起来看看[Matthew James Taylor](http://matthewjamestaylor.com/about)介绍的方法。
### 结构
{% highlight html %}
<div id="container">
	<div id="header">Header Section</div>
	<div id="page" class="clearfix">
		页面容容部分
	</div>
	<div id="footer">Footer Section</div>
</div>
{% endhighlight %}
其实我们可以在div#page增加所需的内容结构，如下所示：
{% highlight html %}
<div id="container">
	<div id="header">Header Section</div>
	<div id="page" class="clearfix">
		<div id="left">Left Sidebar</div>
		<div id="content">Main content</div>
		<div id="right">Right sidebar</div>
	</div>
	<div id="footer">Footer Section</div>
</div>
{% endhighlight %}
### 样式
真正来说，实现这页脚永远固定在页面的底部，我们只需要四个div，其中div#container是一个容器，在这个容器之中，我们包含了div#header（头部），div#page（页面主体部分，我们可以在这个div中增加更多的div结构，如上面的代码所示），div#footer（页脚部分）。
{% highlight css %}
html, body {
	margin: 0;
	padding: 0;
	height: 100%;
}

#container {
	min-height: 100%;
	height: auto !important;
	height: 100%; /* IE6不识别min-height */
	position: relative;
}

#header {
	background: #ff0;
	padding: 10px;
}

#page {
	width: 960px;
	margin: 0 auto;
	padding-bottom: 60px; /* 等于footer的高度 */
}

#footer {
	position: absolute;
	bottom: 0;
	width: 100%;
	height: 60px; /* 脚部的高度 */
	background: #6cf;
	clear: both;
}

/*=======主体内容部分=======*/
#left {
	width: 220px;
	float: left;
	margin-right: 20px;
	background: lime;
}

#content {
	background: orange;
	float: left;
	width: 480px;
	margin-right: 20px;
}

#right {
	background: green;
	float: right;
	width: 220px;
}
{% endhighlight %}
下面我们一起来看看这种方法的实现原理：

1. **&lt;html&gt;和&lt;body&gt;标签**：html和body标签中必须将高度(height)设置为“100%”，这样我们就可以在容器上设置百分比高度，同时需要将html,body的margin和padding都移除，也就是html,body的margin和padding都为0；
2. **div#container容器**：div#container容器必须设置一个最小高度(min-height)为100％；这主要使他在内容很少（或没有内容）情况下，能保持100％的高度，不过在IE6是不支持min-height的，所以为了兼容IE6，我们需要对min-height做一定的兼容处理，具体可以看前面的代码，或者阅读[Min-Height Fast Hack](http://www.dustindiaz.com/min-height-fast-hack/)了解如何解决min-height在Ie6下的bug问题。另外我们还需要在div#container容器中设置一个"position:relative;"以便于里面的元素进行绝对定位后不会跑了div#container容器；
3. **div#page容器**：div#page这个容器有一个很关键的设置，需要在这个容器上设置一个padding-bottom值，而且这个值要等于（或略大于）页脚div#footer的高度（height）值，当然你在div#page中可以使用border-bottom-width来替代padding-bottom，但有一点需要注意，此处你**千万不能使用margin-bottom来代替padding-bottom，不然会无法实现效果**；
4. **div#footer容器**：div#footer容器必须设置一个固定高度，单位可以是px(或em)。div#footer还需要进行绝对定位，并且设置bottom:0；让div#footer固定在容器div#container的底部，这样就可以实现我们前面所说的效果，当内容只有一点时，div#footer固定在屏幕的底部（因为div#container设置了一个min-height:100%），当内容高度超过屏幕的高度，div#footer也固定在div#container底部，也就是固定在页面的底部。你也可以给div#footer加设一个"width:100%;"，让他在整个页面上得到延伸；
5. **其他div**：至于其他容器可以根据自己需求进行设置，比如说前面的div#header，div#left，div#content，div#right等。
### 优点
结构简单清晰，无需js和任何hack能实现各浏览器下的兼容，并且也适应iphone。
### 缺点
不足之处就是需要给div#footer容器设置一个固定高度，这个高度可以根据你的需求进行设置，其单位可以是px也可以是em，而且还需要将div#page容器的padding-bottom（或border-bottom-width）设置大小等于（或略大于）div#footer的高度，才能正常运行。

上面就是[Matthew James Taylor](http://matthewjamestaylor.com/about)介绍的如何实现页脚永远固定在页面的底部，如果大家感兴趣可以阅读[原文](http://matthewjamestaylor.com/blog/keeping-footers-at-the-bottom-of-the-page)，也可以直接点击这里查看[Demo](http://matthewjamestaylor.com/blog/bottom-footer-demo.htm)。

---
# 方法二
这种方法是利用footer的margin-top负值来实现footer永远固定在页面的底部效果，下面我们具体看是如何实现的。
### 结构
{% highlight html %}
<div id="container">
	<div id="page">Main Content</div>
</div>
<div id="footer">footer</div>
{% endhighlight %}
上面的代码是最基本的HTML Code，同时你也发现了div#footer和div#container是同辈关系，不像方法一，div#footer在div#container容器内部。当然你也可以根据你的需要把内容增加在div#container容器中，如：一个三列布局，而且还带有header部分，请看下面的代码：
{% highlight html %}
<div id="container">
	<div id="header">Header Section</div>
	<div id="page" class="clearfix">
		<div id="left">Left sidebar</div>
		<div id="content">Main content</div>
		<div id="right">Right sidebar</div>
	</div>
</div>
<div id="footer">Footer section</div>
{% endhighlight %}
### 样式
{% highlight css %}
html, body {
	height: 100%;
	margin: 0;
	padding: 0;
}

#container {
	min-height: 100%;
	height: auto !important;
	height: 100%;
}

#page {
	padding-bottom: 60px; /* 高度等于footer的高度 */
}

#footer {
	position: relative;
	margin-top: -60px; /* 等于footer的高度 */
	height: 60px;
	clear: both;
	background: #c6f;
}

/*==========其他div==========*/
#header {
	padding: 10px;
	background: lime;
}

#left {
	width: 18%;
	float: left;
	margin-right: 2%;
	background: orange;
}

#content {
	width: 60%;
	float: left;
	margin-right: 2%;
	background: green;
}

#right {
	width: 18%;
	float: left;
	background: yellow;
}
{% endhighlight %}
方法一和方法二有几点是完全相同的，比如说方法一中的1-3三点，在方法二中都一样，换句话说，方法二中也需要**把html,body高度设置为100%,并重置margin,padding为0；其二div#container也需要设置min-height:100%,并处理好IE6下的min-height兼容问题；其三也需要在div#page容器上设置一个padding-bottom或border-bottom-width值等于div#footer高度值（或略大于）**。那么两种方法不同之处是：

1. **div#footer放在div#container容器外面，也就是说两者是同级关系，如果你有新元素需要放置在与div#container容器同级，那你需要将此元素进行绝对定位，不然将会破坏div#container容器的min-height值**；
2. **div#footer进行margin-top的负值设置，并且此值等于div#footer的高度值，而且也要和div#page容器的padding-bottom(或border-bottom-width)值相等**。
### 优点
结构简单清晰，无需js和任何hack能实现各浏览器下的兼容。
### 缺点
要给footer设置固定值，因此无法让footer部分自适应高度。

大家要是对这种方法感兴趣，你也可以浏览一下《[CSS Sticky Footer](http://www.cssstickyfooter.com/)》和《[Pure Css Sticky Footer](http://www.lwis.net/journal/2008/02/08/pure-css-sticky-footer/)》，或者直接点击[Demo](http://www.lwis.net/profile/CSS/sticky-footer.html)查看其源代码。

---
# 方法三
实现在页脚永远固定在页面底部的方法有很多，但是有很多方法是需要使用一些hack或借助javaScrip来实现，那么接下来要说的方法三，仅仅使用了15行的样式代码和一个简单明了的HTML结构实现了效果，并且兼容性强，别的不多说，先看代码。
### 结构
{% highlight html %}
<div id="container">
	<div id="page">Your Website content here.</div>
	<div class="push"><!-- not have any content --></div>
</div>
<div id="footer">Footer Section</div>
{% endhighlight %}
上面是最基本的HTML Markup，当然你也可以加上新的内容，不过有一点需要注意**如果你在div#container和div#footer容器外增加内容的话，新加进徕的元素需要进行绝对定位**。如如说你可以在div#container容器中加上你页面所需的元素。
{% highlight html %}
<div id="container">
	<div id="header">Header Section</div>
	<div id="page" class="clearfix">
		<div id="left">Left sidebar</div>
		<div id="content">Main Content</div>
		<div id="right">Right Content</div>
	</div>
	<div class="push"><!-- not put anything here --></div>
</div>
<div id="footer">Footer Section</div>
{% endhighlight %}
### 样式
{% highlight css %}
html, body {
	height: 100%;
	margin: 0;
	padding: 0;
}

#container {
	min-height: 100%;
	height: auto !important;
	height: 100%;
	margin: 0 auto -60px; /* margin-bottom的负值等于footer高度 */
}

.push, #footer {
	height: 60px;
	clear: both;
}

/*==========其他div效果==========*/
#header {
	padding: 10px;
	background: lime;
}

#left {
	width: 18%;
	float: left;
	margin-right: 2%;
	background: orange;
}
#content {
	width: 60%;
	float: left;
	margin-right: 2%;
	background: green;
}

#right {
	width: 18%;
	float: left;
	background: yellow;
}

#footer {
	background: #f6c;
}
{% endhighlight %}
跟前面两种方法相比较，方法三更类似于方法二，他们都将div#footer放在div#container容器之外，而且这个方法在div#container容器中还增加了一个div.push用来把div#footer推下去，下面我们就一起看看这种方法是怎么实现页脚永远固定在页面底部的。

1. **&lt;html&gt;和&lt;body&gt;标签**：html,body标签和前两种方法一样，需要设置“height:100%”并重置“margin”和“padding”为0；
2. **div#container**：方法三关键部分就在于div#container的设置，首先需要设置其最小高度（min-height）为100％,为了能兼容好IE6，需要对min-height进行兼容处理（具体处理方法看前面或代码）另外这里还有一个关键点**在div#container容器上需要设置一个margin-bottom，并且给其取负值，而且值的大小等于div#footer和div.push的高度，如果div#footer和div.push设置了padding和border值，那么div#container的margin-bottom负值需要加上div#footer和div.push的padding和border值。也就是说“div#container{margin-bottom:-[div#footer的height+padding+border]或者-[div.push的height+padding+border]}”。一句话来说：div#container的margin-bottom负值需要和div#footer以及div.push的高度一致（如果有padding或border时，高度值需要加上他们）**；
3. **div.push**：在div.push中我们不应该放置任何内容，而且这个div必须放置在div#container容器中，而且是最底部，并且需要设置其高度值等于div#footer的值，最好加上clear:both来清除浮动。div.push容器在此处所起的作用就是将footer往下推；
4. **div#footer容器**：div#footer容器和方法二一样，不能放到div#container内部，而和div#container容器同级，如果需要设置元素和footer之间的间距，最好使用padding来代替margin值。
### 优点
简单明了，易于理解，兼容所有浏览器。
### 缺点
较之前面的两种方法，多使用了一个div.push容器，同样此方法限制了footer部分高度，无法达到自适应高度效果。

如果大家对方法三想了解更多可以点击[这里](http://ryanfait.com/resources/footer-stick-to-bottom-of-page/)或者直接从[DEMO](http://ryanfait.com/sticky-footer/)中下载代码自己研究一下。

---
# 方法四
前面三种方法我们都不需要任何javaScript或jQuery的帮助，让我们实现了页脚永远固定在页面底部的效果，前面三种方法虽然没有使用jQuery等帮助，但我们都额外增加了HTML标签来实现效果。如果你省略了这些HTML标签，再要实现效果就比较困难了，那么此时使用jQuery或javaScript方法来帮助实现是一种很好的办法，下面我们就一起来看第四种方法，通过jQuery来实现页脚永远固定在页面底部的效果。
### 结构
{% highlight html %}
<div id="header">Header Section</div>
<div id="page" class="clearfix">
	<div id="left">Left sidebar</div>
	<div id="content">Main Content</div>
	<div id="right">Right Content</div>
</div>
<div id="footer">Footer Section</div>
{% endhighlight %}
这里我们没有增加没用的HTML标签，此时你也可以随时在body中增加内容，只要**确保div#footer是在body最后面**。
{% highlight html %}
.
.
.
<div id="footer">Footer Section</div>
{% endhighlight %}
### 样式
{% highlight css %}
* {
  margin: 0;
  padding: 0;
}

.clearfix:before, .clearfix:after {
  content: "";
  display: table;
}

.clearfix:after {
  clear: both;
}

.clearfix {
  zoom:1; /* IE &lt; 8 */
}

#footer {
	height: 60px;
	background: #fc6;
	width: 100%;
}

/*==========其他div==========*/
#header {
	padding: 10px;
	background: lime;
}
#left {
	width: 18%;
	float: left;
	margin-right: 2%;
	background: orange;
}

#content {
	width: 60%;
	float: left;
	margin-right: 2%;
	background: green;
}

#right {
	width: 18%;
	float: left;
	background: yellow;
}
{% endhighlight %}
这个方法不像前面三种方法靠CSS来实现效果，这里只需要按正常的样式需求写样式，不过有一点需要特别注意**在html,body中不可以设置高度height为100%，否则此方法无法正常运行**，如果你在div#footer中设置了一个高度并把宽度设置为100%将更是万无一失了。
### 行为
{% highlight javascript %}
// Window load event used just in case window height is dependant upon images
$(window).bind("load", function() {
	var footerHeight = 0,
		footerTop = 0,
		$footer = $("#footer");
	positionFooter();

	// 定义positionFooter function
	function positionFooter() {

		// 取到div#footer高度
		footerHeight = $footer.height();

		// div#footer离屏幕顶部的距离
		footerTop = ($(window).scrollTop()+$(window).height()-footerHeight)+"px";

		/* DEBUGGING STUFF
			console.log("Document height: ", $(document.body).height());
			console.log("Window height: ", $(window).height());
			console.log("Window scroll: ", $(window).scrollTop());
			console.log("Footer height: ", footerHeight);
			console.log("Footer top: ", footerTop);
			console.log("-----------")
		*/

		// 如果页面内容高度小于屏幕高度，div#footer将绝对定位到屏幕底部，否则div#footer保留它的正常静态定位
		if ( ($(document.body).height()+footerHeight) < $(window).height())  {
			$footer.css({
				position: "absolute"
			}).stop().animate({
				top: footerTop
			});
		} else {
			$footer.css({
				position: "static"
			});
		}
	}
	$(window).scroll(positionFooter).resize(positionFooter);
});
{% endhighlight %}
使用上面的jQuery代码，可以轻松的帮我们实现页脚永远固定在页面底部，使用这种方法有几个地方需要注意：

1. **确保正常引入了jQuery版本库，并正常调入了上面那段jQuery代码**；
2. **确保&lt;div id="footer"&gt;是在body中最后**；
3. **确保在html,body中没有设置高度为100%**。
### 优点
结构简单，无需外加无用标签，兼容所有浏览器，不用另外写特别样式。页脚可以不固定高度。
### 缺点
在不支持js的浏览器中无法正常显示，另外每次改变浏览器大小会闪动一下。

---
今天主要和大家一起探讨和学习了四种方法，用来实现Web页面脚部永远固定在页面的底，这里在着得说清楚一下，**是页脚永远固定在页面的底部，而不是永远固定在浏览器窗口的底部，换句话说，就说当页面主内容没有显示屏幕高时，页脚固定在显示器屏幕的底部，但当页面内容超过显示器屏幕高度时，页脚又会跟随内容往下沉，但页脚都永远固定在页的底部**。前面三种都是纯CSS实现，最后一种采用的是jQuery方法实现，四种方法各有各的利弊，大家使用时可以根据自己的需求来定夺，希望这篇文章能给大家带来一定的帮助。如果大家有更好的方法，希望能和我一起分享，如果您愿意，可以直接给我留言，我会一直和您在一起，一起学习和讨论这方面的知识。