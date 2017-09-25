title: 由一个表格需求引发的blog-DOM中的尺寸属性
date: 2017-09-20 16:37:00
categories: Young
tags: 
- DOM
- 跨浏览器属性

---
最近数据平台有一个需求，页面滚动的时候，表格的表头悬停在页面顶部，实现的时候遇到了不少问题，也尝试了几种方案，下一篇blog会尝试分享一下，由于涉及到较多的DOM操作，尤其是各种尺寸位置属性，所以对这些尺寸属性的含义做了一些总结。

<!--more-->

## 偏移量相关属性

偏移量相关属性包括偏移量尺寸和位置属性，其中偏移量尺寸指的是元素在屏幕上占用的所有可见空间，包括其内容区的空间，内边距，边框以及滚动条占用的空间（不包括外边距）。偏移量位置属性是一个相对的概念，其偏移量是参照最近的定位（display值非none或static）祖先元素计算的。以像素作为单位。

1. 偏移量尺寸
	* offsetWidth:元素在水平方向占用的空间大小，包括元素内容区的宽度，左右内边距的宽度，垂直滚动条的宽度，左右边框的宽度。
	* offsetHeight:元素在垂直方向占用的空间大小，包括元素内容区高度，上下内边距的高度，水平滚动条的高度，上下边框的高度。

2. 	偏移量位置
	* offsetParent:返回被引用元素最近的定位过的祖先元素。如果没有定位过的祖先元素，则返回body。
	* offsetTop:被引用元素上边框的外边缘与其offsetParent上边框的内边缘之间像素距离。
	* offsetLeft:被引用元素左边框的外边缘与其offsetParent左边框的内边缘之间的像素距离。

**用法：**通常，如果想要获取某个元素在页面上的偏移量，将这个元素的offsetLeft或offsetTop与其offsetParent的相同属性相加，递归的计算直到根元素，就可以得到一个比较准确的结果。需要注意上述属性都是只读的，每次访问都需要重新计算。


## 客户区尺寸

客户区尺寸只与元素自身所占区域大小有关，相关的属性如下。

* clientWidth:元素内容区宽度加上左右内边距的宽度。
* clientHeight:元素内容区高度加上上下内边距的高度。
* clientTop:元素上边框的宽度。
* clientLeft:元素左边框的宽度。

**用法：**最常见的用途之一是用来确定浏览器视口的尺寸，根据浏览器的支持情况，使用document.documentElement或者document.body的相应尺寸来获取视口宽高信息。

##	 滚动元素相关属性
滚动元素是包含滚动条的元素，相关属性如下。

1. 滚动尺寸
	* scrollWidth:元素内容的实际宽度（包含左右内边距），即在没有滚动条的情况下，元素内容的总宽度。
	* scrollHeight:元素内容的实际高度（包含上下内边距），即在没有滚动条的情况下，元素内容的总高度。

2. 滚动位置
	* scrollTop:被隐藏在内容区域上方的像素数，这个属性和scrollLeft均是可配置的，可以通过设置这个属性的值来改变元素滚动的位置。
	* scrollLeft:被隐藏在内容区域左侧的像素数。

**用法：**滚动尺寸主要用来确定元素内容的实际大小，滚动位置既可以用来确定当前滚动的状态也可以设置滚动位置。一个很常见的使用场景就是判断页面的滚动情况，不同的浏览器的获取scrollTop方法不同，document.documentElement返回的是文档的根节点即<html>，document.body返回的是body，在chrome中，获取页面scrollTop只能通过document.body.scrollTop来获取，document.documentElement获取相同属性始终返回0，而在FF下（只测试了chrome和FF）正好相反。

## 说了半天还是来个图
作为一个斗图新人，没有图没有底气，所以虽然手残，我还是画了一个图，首先我们就假装图中那个灰色的东西是滚动条，然后看图对应上述属性吧~

![](http://p1.meituan.net/xgfe/447fe588baa4cd81ed6e3cd6fc38b55d85970.png)

## 事件event位置

对于页面中发生的事件，获取点击事件鼠标的位置信息在应用中很常见，以垂直位置为例，常用相关属性有三个：pageY, clientY, screenY。

* pageY:鼠标在页面上的文档坐标，以文档为参照，距离文档左上角的垂直像素距离，所以这个属性和页面是否滚动无关。
* clientY:以视口为参照，鼠标距离视口左上角的垂直像素距离。
* screenY:以浏览器为参照，鼠标距离浏览器左上角的垂直像素距离，包含工具栏之类。

**Ps:**配图一目了然，写这么多字好啰嗦的好么。。待我搞几个图来再来补充。



