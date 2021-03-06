---
title: Chrome也疯狂之Vimium插件
layout: post
date: 2015-07-07 20:10:57
tags: vimium
categories: Tools
comments: true
description: Chrome的一个插件，像在Vim里面一样在使用Chrome浏览器，完全脱离鼠标 操作
---

由于最近换上了Mac，深感外设的累赘，脱离了外接鼠标以及键盘之后发现操作更加的流畅了（可怜我入手不到一年的机械键盘）。当然脱离鼠标用触摸板来操作浏览器有时候还是有些许的不流畅，对于喜欢键盘操作的我来说只能寻求其它的方法，谁曾想竟然让我遇到了Vimium，原谅我读书少，至今才见到Vimium，其实它仅仅是Chrome上的一个超级强大的扩展程序，装上它之后，从此你就可以像在操作Vim一样来操作Chrome浏览器了，当然如果你喜欢Safari浏览器的话，也就与之对应的Vimri，下面具体说下在Vimium的快捷键，虽然说是基于Vim来做出的一个用在Chrome上的一个扩展程序，但是有个别的会有差异。感兴趣的可以去[这里](https://github.com/philc/vimium/blob/master/README.md)看下。

##Vimium快捷键介绍

	j: 向下移动。
	k：向上移动。
	h：向左移动。
	l：向右移动。
	zH：一直移动到左部。
	zL:一直移动到右部。
	gg：跳转到页面的顶部。
	G：跳转到页面的底部。
	d：向下翻页（相当于PageDown被按下了）
	u：向上翻页（相当于PageUp被按下了）
	r：重新载入该页（相当于F5刷新页面）
	gs：查看页面源代码
	yy：拷贝当前页面的URL到剪贴板
	yf：拷贝某一个URL到剪贴板
	p:在当前页面打开剪切板中的链接
	P:在新标签页打开剪切板中的链接
	o:在当前页打开URL，书签或者是历史记录
	O:在新的标签页打开URL、书签或者是历史记录
	T:从你打开的Tabs里面查询
	b:在当前页打开书签
	B:在新的标签页打开书签
	gu：跳转到父页面（比如http://www.douban.	com/group/vim/，输入后跳转到父页面即http://www.douban.	com/group/，所以不同于H的快捷键是回到上个历史页面）
	
	i：输入模式（如果发现命令不起作用，可能是进入输入模式了，此时按Esc回到命令模式）
	gi：将焦点集中到第一个输入框（输入gNi则焦点集中到第N个输入框）
	f：在当前的页面打开一个新的链接。
	F：在新的页面打开一个新的链接。
	<a-f>:在当前页面打开多个链接（没感觉使用到了多个标签，不过表示的是输入af）
	gf：循环到当前页面的下一个框层（可能跟页面制作有关，目前没用到）
	
	查找模式：（和Vim相似）
	/ : 查找
	n: 向下查找匹配内容
	N：向上查找匹配内容
	
	导航历史：
	H：回退上一个历史页面（相当于浏览器中的向左箭头）
	L：回到下一个历史页面（相当于浏览器的向右箭头）
	
	标签页操作：
	K，gt：跳转到右边的一个标签页
	J，gT：跳转到左边的一个标签页
	t：创建一个新的标签页
	x：关闭当前的标签页
	X：恢复刚刚关闭的标签页
	？：显示命令的帮助提示（再按一次关闭）

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**