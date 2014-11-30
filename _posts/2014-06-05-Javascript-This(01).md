---
layout: post
category : blog
title: "JavaScript 中的 this "
tagline: "JavaScript 中的 this "
tags : [blog, javascript]
---



`this`是一个关键字，不是变量，也不是属性名。javascript的语法不允许给this赋值。和变量不同，关键字this没有作用于的限制，嵌套的函数不会从调用它的函数中（即上一层函数）继承this。

　　1、如果嵌套函数作为方法调用，其this的值指向调用它的对象；

　　2、如果嵌套函数作为函数调用，其this值不是全局对象（非严格模式下）就是undefind（严格模式下）；

很多人误以为，调用嵌套函数时this会指向调用外层函数的上下文。

如果你想访问这个外部函数的this值，可将this的值保存在一个变量里，这个变量和内部函数都同在一个作用域内，通常使用变量self来保存this，比如：

例子1：

     var o={　　　　　　　　　　　　　　　　//对象o

    　　m:function(){　　　　　　　　　　　//对象中的方法m

    　　　　var self=this;　　　　　　　　　//将this值保存在一个变量中

    　　　　console.log(this===o);　　　　//输出true，this就是指对象o

    　　　　f();　　　　　　　　　　　　　　

    　　　　function f(){　　　　　　　　　//在方法m内部定义一个嵌套函数f

    　　　　　　console.log(this===o);　　//"false"：this的值是指向全局对象或者undefined

    　　　　　　console.log(self===o);　　//"true"：self指向外部函数的this值

    　　　　}

    　　}

    };

例子2：

    var name = "The Window";   
    　　var object = {   
    　　　　name : "My Object",   
    　　　　getNameFunc : function(){   
    　　　　　　return function(){   
    　　　　　　　　return this.name;   
    　　　　　};   
    　　　　}   
    };   
    alert(object.getNameFunc()());  //输出结果为：The Window
