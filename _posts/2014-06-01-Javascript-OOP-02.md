---
layout: post
category : blog
title: "理解JavaScript面向对象的思路 "
tagline: "理解JavaScript面向对象的思路 "
tags : [blog, javascript]
---


## 理解JavaScript面向对象的思路

一般来说大家比较熟悉的面向对象方式是基于类的面向对象，声明一个类，然后在根据类声明的描述去创建对象，通过类与类之间的继承和组合关系来复用代码。大多数情况下，基于类的面向对象语言（C++,C#,Java之类的）都把类整合进自己的类型系统，即每个类（Class）同时也是一个变量类型（Variable Type）,并允许子类类型的值被赋值给父类类型变量。

而JS的设计采用了一种完全不同的思路。首先JS的类型是不可扩展的（就是说，语言的使用者无法添加新的类型）这样就无法采用上述语言的做法。根据语言标准，JS设计了6种用户可以使用的数据类型（因为JS是弱类型的，所以变量没有类型，只有数据有类型）：
Boolean Number String Null Undefined Object

为了实现面向对象，JS把所有的对象放到Object类型中,这样，JS就有6种用户可使用的数据类型。除了Undefined,JS为所有的类型提供了字面值（literal）语法，现在来看，JS的Object字面值表示设计的相当成功，现在甚至成为了一种数据交换的格式，这就是大家所熟悉的JSON。A Sample:
    
    var aTShirt={color:"yellow",size:"big"} 
    
作为动态语言，JS允许使用者对一个已经创建的对象添加或者删除属性。对一个不存在的属性赋值即向其添加属性，delete关键字被用于删除属性。这个delete比较容易跟C++的delete运算符混淆，后者是用来释放不再使用的对象的。

本来有了这些语法，已经可以做基本的面向对象编程了，但是仅仅如此，JS代码复用性比其它语言弱太多。比如，你甚至无法为一组对象做一个统一的操作，必须通过循环遍历来实现，于是JS引入了原型（prototype）,具体的实现方式是为每个对象规定一个私有属性`[[prototype]]`，当读取一个对象的属性时，如果对象本身没有这个属性，会尝试访问[[prototype]]的相应属性。具体实现中，[[prototype]]所指向的对象仍然可以有[[prototype]]，实际的访问就是一个链式的操作，直到找到这个属性或者[[prototype]]为空为止，所以常常听到[[prototype]]链的说法。为了防止[[prototype]]出现循环，JS引擎会在任何对象的[[prototype]]属性被修改时检查。

按照标准，这个[[prototype]]语言使用者是无法访问的，不过FireFox的JS引擎把[[prototype]]暴露出来，作为公有属性"__proto__"，这样，我们就可以通过操作原型对象来控制一组对象的行为。我们可以借用FF提供的便利来了解一下[[prototype]]的工作原理：
    var proto={a:1};
    var m={__proto__:proto};
    var n={__proto__:proto};
    alert([m.a,n.a]);
    proto.a=2;
    alert([m.a,n.a]);

JS规定了一个内建对象作为所有对象的最终[[prototype]]，也就是说即使用{}创建的对象，也会有[[prototype]]指向这个内建对象。

通过这个机制，我们完全可以得到跟基于类的语言相当程度的对象复用能力——但是当然我们还需要函数。在JS中，函数仅仅是一种特殊的对象，JS设计了()运算符和function关键字让JS的函数看起来更像是传统的语言。只要实现了私有方法[[call]]的对象都被认为是函数对象(这个[[call]]跟大家比较熟悉的Function.prototype.call完全是两回事)，类似[[prototype]]，[[call]]也是语言使用者完全无法访问的，这一次FF也没有为我们提供公有属性来替代。

本来到这里为止，JS的面向对象已经很完整了，但是JS为了让自己的语法看起来更像是Java之类的语言，又引入了new关键字，在上面大部分语言中new都是针对类来做的，而JS没有类，甚至没有声明域，所以这个new还是要在对象上做文章，new会调用私有方法[[contruct]]，任何实现了[[construct]]的对象都可以被new接受。然而如何才能让一个对象可以被new呢？JS并没有额外提供构造这种对象方法，所以所有通过function关键字构造的函数对象被设计成实现了[[construct]]方法。这也就是JS的new很奇怪地针对函数的原因。值得一提的是，并非只有函数可以被new,JS的宿主环境可能提供一些其它对象，典型的例子是IE中的ActiveXObject。

所有函数的[[construct]]方法都是类似的：创建一个新的对象，将它的[[prototype]]设为函数对象的共有属性prototype，以新对象做为this指针的值，执行函数对象

这样对同一函数的new运算实际上创建了相似的对象：拥有共同的原型[[prototype]]，被同一函数处理过。这样的new运算就很类似Class了，同时由于JS的动态性，所有的"类"在运行时任你宰割，想要模拟继承之类的行为也就很容易了，由于是弱类型且是动态函数，不存在需要多态的问题，JS完全可以做到基于类的面向对象。

最后提供几道题，大家茶余饭后写完程序不妨做做，都做对说明你已经理解了protype-based javascript

    (请用FF来看结果)
    Function.prototype.prop=1;
    alert(Object.prop)
    alert(Function.prop)
    Object.prototype.prop=1;
    alert(Object.prop)
    alert(Function.prop)
    Function.__proto__.prop=1;
    alert(Object.prop)
    alert(Function.prop)
    function Class(){
    }
    Class.prototype=Class;
    Class.__proto__.prop=1
    alert((new Class).prop);
    function Class(){}
    Class.prototype=Class.__proto__;
    Function.prototype.prop=1;
    alert((new Class()).prop)
    function Class(){
    }
    Class.prototype.__proto__.prop=1;
    Class.prototype=new Class;
    alert((new Class).prop);
