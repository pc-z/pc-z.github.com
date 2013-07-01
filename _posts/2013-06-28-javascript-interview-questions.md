---
layout: post
title: Javascript面试题（持续更新）
description: "javascript interview questions"
categories: Javascript
tags: [javascript interview questions]
---

**问1：优化以下代码性能，并给a添加事件，要求点击弹出相应的index值？**
{% highlight javascript %}
var aLinks = document.getElementsByTagName('a');
for ( i = 0; i < aLinks.length; i++ ) {
	...
}
{% endhighlight %}
**答**：优化后的代码：
{% highlight javascript %}
var aLinks = document.getElementsByTagName('a');
for ( var i = 0, l = aLinks.length; i < l; i++ ) {
	...
}
{% endhighlight %}
添加事件，第一种方法（加索引）：
{% highlight javascript %}
var aLinks = document.getElementsByTagName('a');
for (var i = 0, l = aLinks.length; i < l ; i++ ) {
    aLinks[i].Index = I;
    aLinks[i].onclick = function(){
		alert(this.Index);
	};
}
{% endhighlight %}
第二种方法（闭包）：
{% highlight javascript %}
var aLinks = document.getElementsByTagName('a');
for ( var i = 0, l = aLinks.length; i < l; i++ ) {
    aLinks[i].onclick = (function(a) {
    	return function() {
			alert(a);
		};
    })(i);
}
{% endhighlight %}
第一种方法需要给元素添加属性，推荐第二种方法。

---
**问2：Javascript对象的深度克隆？**

**答**：方法1：
{% highlight javascript %}
Object.prototype.deepClone = function() {
	function cloneObj() {}
	cloneObj.prototype = this;
	var obj = new cloneObj();
	for ( var o in obj ) {
		if ( typeof(obj[o]) == "object" ) 
			obj[o] = obj[o].deepClone();
	}
	return obj;
}

// 近视的解法
Object.prototype.clone = function(){
    var o = this.constructor === Array ? [] : {};
    for ( var e in this ) {
        o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
    }
    return o;
} 
{% endhighlight %}
（原文：[javascript深度克隆一个对象](http://www.2cto.com/kf/201201/116033.html)）

方法2：
{% highlight javascript %}
var cloneObj = function(obj) {
	var newobj = obj.constructor === Object ? {} : [];
	if ( typeof JSON === 'object' ) {
		var s = JSON.stringify(obj); // 系列化对象
		newobj = JSON.parse(s); // 反系列化（还原）
	} else {
		if ( newobj.constructor === Array ) {
			newobj.concat(obj);
		} else {
			for ( var i in obj ) {
				if ( obj.hasOwnProperty(i) ) {
					newobj[i] = typeof obj[i] === "object" ? cloneObject(obj[i]) : obj[i];
				}
			}
		}
	}
	return newobj;
};

// 测试
var obj = {a: 0, b: 1, c: 2};
var arr = [0, 1, 2];

// 执行深度克隆
var newobj = cloneObj(obj);
var newarr = cloneObj(arr);

// 对克隆后的新对象进行成员删除
delete newobj.a;
newarr.splice(0,1);
console.log(obj, arr, newobj, newarr); // 结果： {a: 0, b: 1, c: 2}, [0, 1, 2], {b: 1, c: 2}, [1, 2];
{% endhighlight %}
代码中的JSON对象及其成员方法stringify和parse属于ECMAScript5规范，它们分别负责将一个对象转换成字符串和还原，从而实现对象的深拷贝。我们知道，数组是特殊的对象，其内置的concat方法可直接返回一个新数组。但是对于Object构造函数（constructor）实例下的对象，则需要通过枚举给新的对象属性赋值。（原文：[也谈JavaScript对象之深度克隆](http://sentsin.com/web/21.html)）

方法三：
{% highlight javascript %}
/**
 * 克隆一个对象
 * @param Obj
 * @returns
 */ 
function clone(Obj) {   
    var buf;   
    if ( Obj instanceof Array ) {   
        buf = [];  // 创建一个空的数组 
        var i = Obj.length;   
        while ( i-- ) {   
            buf[i] = clone( Obj[i] );   
        }   
        return buf;
    } else if ( Obj instanceof Object ) {   
        buf = {};  // 创建一个空对象 
        for ( var k in Obj ) { // 为这个对象添加新的属性 
            buf[k] = clone( Obj[k] );   
        }
        return buf;   
    } else {   
        return Obj;   
    }   
}
{% endhighlight %}
纯粹是个递归的思路，如果是数组，就一个的放到一个数组对象中，如果是对象，则复制他的对象中的属性，如果是普通变量，就直接赋值之。

方法四（方法三的严谨版）：
{% highlight javascript %}
function clone(obj) {

    // Handle the 3 simple types, and null or undefined
    if ( null == obj || "object" != typeof obj ) return obj;

    // Handle Date
    if ( obj instanceof Date ) {
        var copy = new Date();
        copy.setTime( obj.getTime() );
        return copy;
    }

    // Handle Array
    if ( obj instanceof Array ) {
        var copy = [];
        for ( var i = 0, var len = obj.length; i < len; ++i ) {
            copy[i] = clone( obj[i] );
        }
        return copy;
    }

    // Handle Object
    if ( obj instanceof Object ) {
        var copy = {};
        for ( var attr in obj ) {
            if ( obj.hasOwnProperty(attr) ) copy[attr] = clone( obj[attr] );
        }
        return copy;
    }

    throw new Error("Unable to copy obj! Its type isn't supported.");
}
{% endhighlight %}
（原文：[JavaScript中的对象复制(Object Clone)](http://noyesno.net/page/javascript/20111212-331)）

---
**问3：动态打印时间，格式为yyyy-MM-dd hh:mm:ss？**

**答**：
{% highlight javascript %}
function printTime() {
	var timer1 = new Date();
	console.log(timer1);
	var timer = timer1.toLocaleString();
	console.log(timer);
	timer = timer.replace(/[年月]/g, "-");
	timer = timer.replace(/日/, "");
	console.log(timer);
	time.innerHTML = timer;
}
setInterval("printTime()", 1000);
{% endhighlight %}
在Chrome浏览器中，这种方法会在dd和hh之间产生不必要的中文（如“上午”，“下午”）。