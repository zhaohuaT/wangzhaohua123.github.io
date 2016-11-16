---
layout: post
title:  "什么是javascript"
date:   2015-02-15 22:14:54
categories: javascript
tags: javascript
excerpt: 1、什么是javascript

             JavaScript 是网景（Netscape）公司开发的一种基于客户端浏览器、面向（基于）对象、事件驱动式的网页脚本语言。
         	客户端浏览器：
---


一、简介

1、什么是javascript
 
    JavaScript 是网景（Netscape）公司开发的一种基于客户端浏览器、面向（基于）对象、事件驱动式的网页脚本语言。
	客户端浏览器：
	HTML、CSS、JavaScript客户端语言
	PHP/Java/.NET服务器语言
	判断一种语言是客户端还是服务器端，主要看“它”在哪里运行
	JavaScript是一种面向对象的脚本语言
	事件驱动式
	
2、javascript的用途

	1）网页特效
	2）表单验证
	3）网页小游戏（超级玛丽、五子棋、坦克大战）
	4）服务器端编程（Ajax、Node.js）

二、javascript的基本语法

1、基本格式 

	1）JavaScript区分大小写
	2）JavaScript脚本程序须嵌入在HTML文件中
	3）JavaScript脚本程序中不能包含HTML标记代码,如果想在javascript中使用html代码，可以采用document.write(“<h1>hello world</h1>”);
	4）每行写一条脚本语句:document.write(‘hello’);alert(‘world’);
	5）语句末尾可以加分号（建议都加分号）document.write(‘hello world’);alert(‘hello javascript’);
	6）JavaScript脚本程序可以独立保存为一个外部文件public.js文件中，并通过script中的src属性直接引入

2、变量

	变量是用来临时存储数值的容器，变量存储的数值是可以变化的。
	变量必须要先声明才能使用，使用var声明变量。

	错误的：
	alert(i);
	正确的：
	var i=10;
	alert(i);
	正确的：（只限全局，不推荐使用）
	i=10;
	alert(i)
	变量的命名规则：第一个字符必须是英文字母，或者是下划线(_)，还可以是$美元符号，其后的字符，可以是英文字母，数字，下划线；变量名不能是JavaScript的保留字
	变量的作用域:全局变量、局部变量

3、变量的数据类型：

	1）String 字符类型 
	2）Number 整型、浮点型
	3）、boolean  true、false  
	4）Undefined（变量声明，但未赋值）
	5）Null 空类型
	6）Object 对象类型（数组）

4、parseInt、parseFloat、isNaN函数

	parseInt()：将字符串转化为整数
	parseFloat()：将字符串转化为浮点数
	isNaN()：判断给定的值是否不为数值，如果不是数值则返回true，是数值返返回false

5、运算符

	1）算术运算符
	+ - * / %
	++ ：自加
	--  ：自减

	2）比较运算符
	> < == === >= <= != !==
	==：值相等，类型可以不同
	===：全等于，要求值相等且类型要一致

	3）逻辑运算符
	&&  ||  !

	4）赋值运算符
	= += -= *= /= %=

	5）字符串运算符
	+

三、通过id找对象

	基本语法：document.getElementById(id值)