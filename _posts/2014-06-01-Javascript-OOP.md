---
layout: post
category : blog
title: "深入浅出理解JavaScript面向对象 "
tagline: "深入浅出理解JavaScript面向对象 "
tags : [blog, javascript]
---

忘记原文链接了

JavaScript 是面向对象的。但是不少人对这一点理解得并不全面。

在 JavaScript 中，对象分为两种。  
+ 一种可以称为`“普通对象”`，就是我们所普遍理解的那些：数字、日期、用户自定义的对象（如：{}）等等。
+ 还有一种，称为`“方法对象”`，就是我们通常定义的 function。
你可能觉得奇怪：方法就是方法，怎么成了对象了？但是在 JavaScript 中，方法的确是被当成对象来处理的。下面是一个简单的例子：

 

        JavaScript代码
        function func() {alert('Hello!');} 
        alert(func.toString()); 
在这个例子中，func 虽然是作为一个方法定义的，但它自身却包含一个 toString 方法，说明 func 在这里是被当成一个对象来处理的。更准确的说，func 是一个“方法对象”。下面是例子的继续：

    JavaScript代码
    func.name = “I am func.”; 
    alert(func.name); 
我们可以任意的为 func 设置属性，这更加证明了 func 就是一个对象。
#### 那么方法对象和普通对象的区别在哪里呢？
首先方法对象当然是可以执行的，在它后面加上一对括号，就是执行这个方法对象了。

 

    JavaScript代码
    func(); 
所以，方法对象具有二重性。一方面它可以被执行，另一方面它完全可以被当成一个普通对象来使用。这意味着什么呢？这意味着方法对象是可以完全独立于其他对象存在的。这一点我们可以同 Java 比较一下。在 Java 中，方法必须在某一个类中定义，而不能单独存在。而 JavaScript 中就不需要。

方法对象独立于其他方法，就意味着它能够被任意的引用和传递。下面是一个例子：

    JavaScript代码
    function invoke(f) { 
    f(); 
    } 
    invoke(func); 
将一个方法对象 func 传递给另一个方法对象 invoke，让后者在适当的时候执行 func。这就是所谓的“回调”了。另外，方法对象的这种特殊性，也使得 this 关键字不容易把握。这方面相关文章不少，这里不赘述了。

除了可以被执行以外，方法对象还有一个特殊的功用，就是它可以通过 new 关键字来创建普通对象。

话说每一个方法对象被创建时，都会自动的拥有一个叫 prototype 的属性。这个属性并无什么特别之处，它和其他的属性一样可以访问，可以赋值。不过当我们用 new 关键字来创建一个对象的时候，prototype 就起作用了：它的值（也是一个对象）所包含的所有属性，都会被复制到新创建的那个对象上去。下面是一个例子：

    JavaScript代码
    func.prototype.name=”prototype of func”; 
    var f = new func(); 
    alert(f.name); 
执行的过程中会弹出两个对话框，后一个对话框表示 f 这个新建的对象从 func.prototype 那里拷贝了 name 属性。而前一个对话框则表示 func 被作为方法执行了一遍。你可能会问了，为什么这个时候要还把 func 执行一遍呢？其实这个时候执行 func，就是起“构造函数”的作用。为了形象的说明，我们重新来一遍：

    JavaScript代码
    function func() { 
    this.name=”name has been changed.” 
    } 
    func.prototype.name=”prototype of func”; 
    var f = new func(); 
    alert(f.name); 
你就会发现 f 的 name 属性不再是"prototype of func"，而是被替换成了"name has been changed"。这就是 func 这个对象方法所起到的“构造函数”的作用。所以，在 JavaScript 中，用 new 关键字创建对象是执行了下面三个步骤的：

创建一个新的普通对象； 
将方法对象的 prototype 属性的所有属性复制到新的普通对象中去。 
以新的普通对象作为上下文来执行方法对象。 
对于“new func()”这样的语句，可以描述为“从 func 创建一个新对象”。总之，prototype 这个属性的唯一特殊之处，就是在创建新对象的时候了。


那么我们就可以利用这一点。比如有两个方法对象 A 和 B，既然从 A 创建的新对象包含了所有 A.prototype 的属性，那么我将它赋给 B.prototype，那么从 B 创建的新对象不也有同样的属性了？写成代码就是这样：

    XML/HTML代码
    A.prototype.hello = function(){alert('Hello!');} 
    B.prototype = new A(); 
    new B().hello(); 
这就是 JavaScript 的所谓“继承”了，其实质就是属性的拷贝，这里利用了 prototype 来实现。如果不用 prototype，那就用循环了，效果是一样的。所谓“多重继承”，自然就是到处拷贝了。

JavaScript 中面向对象的原理，就是上面这些了。自始至终我都没提到“类”的概念，因为 JavaScript 本来就没有“类”这个东西。面向对象可以没有类吗？当然可以。先有类，然后再有对象，这本来就不合理，因为类本来是从对象中归纳出来的，先有对象再有类，这才合理。像下面这样的：

    XML/HTML代码
    var o = {}; // 我发现了一个东西。 
    o.eat = function(){return "I am eating."} // 我发现它会吃； 
    o.sleep = function(){return "ZZZzzz..."} // 我发现它会睡； 
    o.talk = function(){return "Hi!"} // 我发现它会说话； 
    o.think = function(){return "Hmmm..."} // 我发现它还会思考。 

    var Human = new Function(); // 我决定给它起名叫“人”。 
    Human.prototype = o; // 这个东西就代表了所有“人”的概念。 

    var h = new Human(); // 当我发现其他同它一样的东西， 
    alert(h.talk()) // 我就知道它也是“人”了！
