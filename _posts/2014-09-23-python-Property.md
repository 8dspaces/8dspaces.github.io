---
layout: post
category : blog
title: "python中property的用法"
tags : [blog, python]
---

pthon中使用property有两种方法  

###使用`property()`函数

property函数原型为`property(fget=None,fset=None,fdel=None,doc=None)`，所以根据自己需要定义相应的函数即可。

假设定义了一个类:C，该类必须继承自object类，有一私有变量_x
现在介绍第一种使用属性的方法：
在该类中定义三个函数，分别用作赋值、取值和删除变量（此处表达也许不很清晰，请看示例）

    class C(object):
    
        def __init__(self):
            self._x=None

        def getx(self):
            return self._x
                        
        def setx(self,value):
            self._x=value
            
        def delx(self):
            del self._x
            
        x=property(getx,setx,delx,'')


现在这个类中的x属性便已经定义好了，我们可以先定义一个C的实例`c=C()`，然后赋值`c.x=100`，取值`y=c.x`，删除：`del c.x`。
是不是很简单呢？

###使用@property装饰符 （原理同上）

    class C:
    
            def __init__(self):
                self._x=None
            
            #下面就开始定义属性了
            @property
            def x(self):
                return self._x
        
            @x.setter
            def x(self,value):
                self._x=value
            
            @x.deleter
            def x(self):
                del self._x

　　
同一属性的**三个函数名要相同**哦。。