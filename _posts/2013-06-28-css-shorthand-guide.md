---
layout: post
title: CSS简写指南
description: "css shorthand guide"
categories: CSS
tags: [css shorthand]
---

> 原文链接：[CSS简写指南](http://www.qianduan.net/css-font-shorthand-attribute-handbook.html)

高效的CSS写法中的一条就是使用简写。通过简写可以让你的CSS文件更小，更易读。而了解CSS属性简写也是前端开发工程师的基本功之一。今天我们系统地总结一下CSS属性的缩写。

### 色彩缩写
色彩的缩写最简单，在色彩值用16进制的时候，如果每种颜色的值相同，就可以写成一个：
{% highlight css %}
color: #113366;
{% endhighlight %}
可以简写为：
{% highlight css %}
color: #136;
{% endhighlight %}
所有用到16进制色彩值的地方都可以使用简写，比如`background-color`、`border-color`、`text-shadow`、`box-shadow`等。

### 盒子大小
这里主要用于两个属性：`margin`和`padding`，我们以`margin`为例，`padding`与之相同。盒子有上下左右四个方向，每个方向都有个外边距：
{% highlight css %}
margin-top: 1px;
margin-right: 1px;
margin-botton: 1px;
margin-left: 1px;
{% endhighlight %}
这四个值可以缩写到一起：
{% highlight css %}
margin: 1px 1px 1px 1px;
{% endhighlight %}
缩写的顺序是上->右->下->左。顺时针的方向。相对的边的值相同，则可以省掉：
{% highlight css %}
margin: 1px; // 四个方向的边距相同，等同于margin: 1px 1px 1px 1px;
margin: 1px 2px; // 上下边距都为1px，左右边距均为2px，等同于margin: 1px 2px 1px 2px
margin: 1px 2px 3px; // 右边距和左边距相同，等同于margin: 1px 2px 3px 2px;
margin: 1px 2px 1px 3px; // 注意，这里虽然上下边距都为1px，但是这里不能缩写。
{% endhighlight %}

### 边框(border)
`border`是个比较灵活的属性，它有`border-width`、`border-style`、`border-color`三个子属性。
{% highlight css %}
border-width: 数字 + 单位;
border-style: none || hidden || dashed || dotted || double || groove || inset || outset || ridge || solid;
border-color: 颜色;
{% endhighlight %}
它可以按照`width`、`style`和`color`的顺序简写：
{% highlight css %}
border: 5px solid #369;
{% endhighlight %}
有的时候，`border`可以写的更简单些，有些值可以省掉，但是请注意哪些是必须的，你也可以测试一下：
{% highlight css %}
border: groove red; // 大家猜猜这个边框的宽度是多少？
border: solid; // 这会是什么样子？
border: 5px; // 这样可以吗？
border: 5px red; // 这样可以吗？？
border: red; // 这样可以吗？？？
{% endhighlight %}
通过上面的代码可以了解到，`border`默认的宽度是3px，默认的颜色是该规则中的`color`属性的值，而`color`默认是黑色的(多谢[@birdstudio ](http://www.qianduan.net/css-font-shorthand-attribute-handbook.html#comment-5367)的提醒 )。`border`的缩写中`border-style`是必须的。

同时，还可以对每条边采用缩写：
{% highlight css %}
border-top: 4px solid #333;
border-right: 3px solid #666;
border-bottom: 3px solid #666;
border-left: 4px solid #333;
{% endhighlight %}
还可以对每个属性采用缩写：
{% highlight css %}
border-width: 1px 2px 3px; // 最多可用四个值，缩写规则类似盒子大小的缩写，下同
border-style: solid dashed dotted groove;
border-color: red blue white black;
{% endhighlight %}

### outline
`outline`类似`border`，不同的是`border`会影响盒模型，而`outline`不会。
{% highlight css %}
outline-width: 数字 + 单位;
outline-style: none || dashed || dotted || double || groove || inset || outset || ridge || solid;
outline-color: 颜色;
{% endhighlight %}
可以缩写为：
{% highlight css %}
outline: 1px solid red;
{% endhighlight %}
同样，`outline`的简写中，`outline-style`也是必须的，另外两个值则可选，默认值和`border`相同。

### 背景(background)
`background`是最常用的简写之一，它包含以下属性：
{% highlight css %}
background-color: color || #hex || RGB(% || 0-255) || RGBa;
background-image: url();
background-repeat: repeat || repeat-x || repeat-y || no-repeat;
background-position: X Y || (top||bottom||center) (left||right||center);
background-attachment: scroll || fixed;
{% endhighlight %}
`background`的简写可以大大的提高css的效率：
{% highlight css %}
background: #fff url(img.png) no-repeat 0 0;
{% endhighlight %}
`background`的简写也有些默认值：
{% highlight css %}
background: transparent none repeat scroll top left;
{% endhighlight %}
`background`属性的值不会继承，你可以只声明其中的一个，其它的值会被应用默认的。

### font
`font`简写也是使用最多的一个，它也是书写高效的CSS的方法之一。

`font`包含以下属性：
{% highlight css %}
font-style: normal || italic || oblique;
font-variant: normal || small-caps;
font-weight: normal || bold || bolder || || lighter || (100-900);
font-size: (number+unit) || (xx-small - xx-large);
line-height: normal || (number+unit);
font-family: name,"more names";
{% endhighlight %}
`font`的各个属性也都有默认值，记住这些默认值相对来说**比较重要**：
{% highlight css %}
font-style: normal;
font-variant: normal;
font-weight: normal;
font-size: inherit;
line-height: normal;
font-family: inherit;
{% endhighlight %}
事实上，`font`的简写是这些简写中最需要小心的一个，稍有疏忽就会造成一些意想不到的后果，所以，**很多人并不赞成使用`font`缩写**。

不过这里正好有个小手册，相信会让你更好的理解`font`的简写：![CSS Font简写属性手册](http://www.qianduan.net/wp-content/uploads/2010/04/030336t77.jpg)

### 列表样式
可能大家用的最多的一条关于列表的属性就是：
{% highlight css %}
list-style: none;
{% endhighlight %}
它会清除所有默认的列表样式，比如数字或者圆点。

list-style也有三个属性：
{% highlight css %}
list-style-type: none || disc || circle || square || decimal || lower-alpha || upper-alpha || lower-roman || upper-roman;
list-style-position: inside || outside || inherit;
list-style-image: (url) || none || inherit;
{% endhighlight %}
list-style的默认属性如下：
{% highlight css %}
list-style: disc outside none;
{% endhighlight %}
需要注意的是，如果`list-tyle`中定义了图片，那么图片的优先级要比`list-style-type`高，比如：
{% highlight css %}
list-style: circle inside url(../img.gif);
{% endhighlight %}
这个例子中，如果`img.gif`存在，则不会显示前面设置的`circle`符号。

PS：其实`list-style-type`有很多种很有用的样式，感兴趣的同学可以参考一下：[https://developer.mozilla.org/en/CSS/list-style-type](https://developer.mozilla.org/en/CSS/list-style-type)

### border-radius(圆角半径)
`border-radius`是CSS3中新加入的属性，用来实现圆角边框。这个属性目前不好的一点儿是，各个浏览器对它的支持不同，IE尚不支持，Gecko(firefox)和webkit(safari/chrome)等需分别使用私有前缀`-moz-`和`-webkit-`。更让人纠结的是，如果单个角的`border-radius`属性的写法在这两个浏览器的差异更大，你要书写大量的私有属性：
{% highlight css %}
-moz-border-radius-bottomleft: 6px;
-moz-border-radius-topleft: 6px;
-moz-border-radius-topright: 6px;
-webkit-border-bottom-left-radius: 6px;
-webkit-border-top-left-radius: 6px;
-webkit-border-top-right-radius: 6px;
border-bottom-left-radius: 6px;
border-top-left-radius: 6px;
border-top-right-radius: 6px;
{% endhighlight %}
呃，是不是你已经看的眼花了？这只是要实现左上角不是圆角，其它三个角都是圆角的情况。所以对于`border-radius`，神飞强烈建议使用缩写：
{% highlight css %}
-moz-border-radius: 0 6px 6px;
-webkit-border-radius: 0 6px 6px;
border-radius: 0 6px 6px;
{% endhighlight %}
这样就简单了很多。PS：不幸的是，最新的Safari(4.0.5)还不支持这种缩写… (thanks [@fireyy](http://fireyy.com/))

就总结这么多，还有其它的可以缩写的属性吗？欢迎大家提出一起讨论。

----------

##### 参考资料
1. [常用CSS缩写语法总结](http://www.w3cn.org/article/tips/2005/103.html)
2. [CSS Shorthand Guide](http://www.dustindiaz.com/css-shorthand/)
3. [Efficient CSS with shorthand properties](http://www.456bereastreet.com/archive/200502/efficient_css_with_shorthand_properties/)
4. [Mozilla Developer Center:CSS Reference](https://developer.mozilla.org/en/CSS_Reference)
5. [CSS Font Shorthand Property Cheat Sheet](http://www.impressivewebs.com/css-font-shorthand-property-cheat-sheet/)
