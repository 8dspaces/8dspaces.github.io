---
layout: post
category : blog
title: "javascript 模块的写法 "
tags : [javascript]
---


全部参考：

[Javascript模块化编程（一）：模块的写法 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)


最近在学javascript，当作记笔记，列一下javascript模块的写法

### 模块的基本写法

        　var module1 = (function(){
        　　　　var _count = 0;
        　　　　var m1 = function(){
        　　　　　　//...
        　　　　};
        　　　　var m2 = function(){
        　　　　　　//...
        　　　　};
        　　　　return {
        　　　　　　m1 : m1,
        　　　　　　m2 : m2
        　　　　};
        　　})();
        
        module.m1()
        module.m2()

### 基本写法的放大模式

如果一个模块很大，就必须分成几个部分，或者一个模块继承另一个模块

        var module1 = (function(mod){
            mod.m3 = function(){
            
            };
            return mod

        })(module1);
        //为模块加上了m3方法，然后返回新的 module1模块，在原有模块基础上加了新方法 

### 宽放大模式 

在浏览器环境中，模块的各个部分通常都是从网上获取的，有时无法知道哪个部分会先加载。如果采用上一节的写法，  
第一个执行的部分有可能加载一个不存在空对象，这时就要采用**"宽放大模式"**。

        var module1 = ( function (mod){
        　　　　//...
        　　　　return mod;
        　　})(window.module1 || {});
        //与"放大模式"相比，＂宽放大模式＂就是"立即执行函数"的参数可以是空对象。

### 输入全局变量 

独立性是模块的重要特点，模块内部最好不与程序的其他部分直接交互。
为了在模块内部调用全局变量，必须显式地将其他变量输入模块。

        var module1 = (function($,YAHOO){
            //
        })(jQuery, YAHOO);

上面的module1模块需要使用jQuery库和YUI库，就把这两个库（其实是两个模块）当作参数输入module1。这样做除了保证模块的独立性，
还使得模块之间的依赖关系变得明显
