---
layout: post
category : blog
title: "Python碎碎念（二）"
tags : [blog, python]
---


+ 我们可以用`__getattr__(self, name)`来查询即时生成的属性。当我们查询一个属性时，如果通过`__dict__`方法无法找到该属性，那么Python会调用对象的`__getattr__`方法，来即时生成该属性

+ (Python中还有一个`__getattribute__`特殊方法，用于查询任意属性。
    `__getattr__`只能用来查询不在__dict__系统中的属性)
    `__setattr__(self, name, value)`和`__delattr__(self, name)`可用于修改和删除属性。它们的应用面更广，可用于任意属性。
    
+  ####即时生成属性的其他方式
    即时生成属性还可以使用其他的方式，比如**descriptor**(descriptor类实际上是property()函数的底层，property()实际上创建了一个该类的对象)。
    
    
    
## 描述符（Descriptor）

描述符是一个有**“监听行为”**的属性，它的控制被描述符的方法重写。这些方法是 `__get__()`, `__set__()`, 和 `__delete__()`.有这些方法的对象叫做描述符。

描述器是强大的，应用广泛的。描述器正是属性, **实例方法, 静态方法, 类方法,和 super** 的实现原理.描述器在Python被用于实现Python 2.2中引入的新式类。