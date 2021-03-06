---
layout: post
category : blog
title: "javascript编程风格 "
tagline: "javascript编程风格 "
tags : [blog, javascript]
---


+ 规则1：表示区块起首的大括号，不要另起一行。
+ 规则2：调用函数的时候，函数名与左括号之间没有空格。
+ 规则3：函数名与参数序列之间，没有空格。
+ 规则4：所有其他语法元素与左括号之间，都有一个空格。
+ 规则5：不要省略句末的分号。
+ 规则6：不要使用with语句。
+ 规则7：不要使用"相等"（==）运算符，只使用"严格相等"（===）运算符。
+ 规则8：不要将不同目的的语句，合并成一行。
+ 规则9：所有变量声明都放在函数的头部。
+ 规则10：所有函数都在使用之前定义。
+ 规则11：避免使用全局变量；如果不得不使用，用大写字母表示变量名，比如UPPER_CASE。
+ 规则12：不要使用new命令，改用`Object.create()`命令。 

    如果不得不使用new，为了防止出错，最好在视觉上把建构函数与其他函数区分开来。
    
+ 规则13：建构函数的函数名，采用首字母大写（InitialCap）；其他函数名，一律首字母小写。
+ 规则14：不要使用自增（++）和自减（--）运算符，用+=和-=代替。
+ 规则15：总是使用大括号表示区块。
