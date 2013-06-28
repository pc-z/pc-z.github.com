---
layout: post
title: Window环境中使Jekyll支持Pygments语法高亮
description: "pygments highlight for jekyll in windows"
categories: 博客建设
tags: [jekyll, pygments]
---

默认情况下Jekyll不支持代码块的语法高亮，这对程序员来说是无法容忍的，好在有几种解决方案，下面我就来介绍下在Windows环境中使Jekyll支持[Pygments](http://pygments.org/)语法高亮的方法（蛋疼的说）。

### 安装Python
Pygments是[Python](http://www.python.org/)下的一个工具，所以首先要安装 Python，在[这里](http://www.python.org/download/)可以下载Windows下的Python安装包，我下的版本是Python 2.7.5（Pygments目前不支持Python 3，若选择Python 3会报`Failed to get header`错误），然后直接双击安装即可。安装完成后把Python的安装路径，默认是` C:\Python27`，添加到环境变量Path中，在命令行中运行
{% highlight python %}
C:\Users\pc>python
Python 2.7.5 (default, May 15 2013, 22:44:16) [MSC v.1500 64 bit (AMD64)] on win 32
Type "help", "copyright", "credits" or "license" for more information.
>>>
{% endhighlight %}
如果看到输出`Python 2.7.5`，则表明安装成功。

### 安装easy_install
[easy_install](http://peak.telecommunity.com/DevCenter/EasyInstall)是 Python里的一个模块安装工具，类似nodejs的npm和ruby的gem，需要使用[easy_install](http://peak.telecommunity.com/DevCenter/EasyInstall)来安装Pygments，先在[这里](http://pypi.python.org/pypi/setuptools)下载setuptools，我下载的是[setuptools 0.7.4](https://pypi.python.org/pypi/setuptools/0.7.4)，将压缩包解压到特定的目录（我的是`S:\Python\setuptools-0.7.4`），打开命令行，进入该目录，运行命令：
{% highlight python %}
python ez_setup.py
{% endhighlight %}
运行之后，在`C:\Python27\Scripts`文件夹中就能看到easy_install了，接着也将路径`C:\Python27\Scripts`添加到环境变量Path中。

### 安装Pygments
通过easy_install安装Pygments，打开命令提示符，运行命令：
{% highlight python %}
easy_install Pygments
{% endhighlight %}
运行之后，Pygments就会被安装到刚才的`C:\Python27\Scripts`目录下。

### 配置Jekyll
Pygments已经安装好之后，我们需要配置Jekyll让Pygments生效。有两种方法可以实现，第一种，打开`_config.yml`文件，加入配置：
{% highlight python %}
pygments: true
{% endhighlight %}
第二种，在Jekyll的启动命令参数中加入`--pygments`。这里我们选择第一种。

### 生成样式
由于Pygments是通过css样式来实现代码高亮的，我们还需要在页面中引入Pygments的css文件。进入我们的版本库目录，运行命令：
{% highlight python %}
pygmentize -S default -f html > media/css/pygments.css
{% endhighlight %}
可以在[Pygments demo](http://pygments.org/demo/)上根据不同语言找到自己喜欢的高亮方案，这里我们使用的是默认的`default`，`media/css/pygments.css`路径需要根据你的实际情况更改，运行之后，会在`media/css/`路径下生成`pygments.css`文件，在`_layouts/default.html`模板页中引入该文件（需要根据你的实际情况选择模板页）。

### 使用方法
Pygments的使用非常简单，把代码用highlight tag包住，highlight后面跟上[语言的种类](http://pygments.org/languages/)，下面是个例子：

	{{ "{% highlight javascript " }}%}
		function helloWorld() {
			alert("Hello World!");
		}
	{{ "{% endhighlight " }}%}

如果一切顺利，运行`jekyll serve`就能看到如下效果了：
{% highlight javascript %}
function helloWorld() {
	alert("Hello World!");
}
{% endhighlight %}
有不满意的话还可以自己修改那个样式文件。

----------

##### 参考资料
1. [在Jekyll中使用Pygments(Windows) - Face of Time](http://fuqcool.github.io/2012/08/11/jekyll-pygments-install-usage/)
2. [Windows下让Jekyll支持Pygments语法高亮 - 小前端](http://ppxu.net/blog/2013/01/18/support-pygments-in-jekyll-on-windows/)
3. [run jekyll --server failed in win7](http://stackoverflow.com/questions/14253116/run-jekyll-server-failed-in-win7)
4. [MentosError header errors](https://github.com/tmm1/pygments.rb/issues/45)
