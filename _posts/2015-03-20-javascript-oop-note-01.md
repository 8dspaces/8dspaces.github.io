---
layout: post
category : blog
title: "[转]面向对象书写Javascript"
tags : [javascript]
---

[引爆你的Javascript代码进化](http://www.hicss.net/evolve-your-javascript-code/)

面向对象书写Javascript想必你一定不会陌生，但我敢说，大多数前端一般不会通过面向对象来书写JS代码。第一是Javascript本身的语言机制原因造成面向对象的书写困难，由于Javascript是原型式继承又是一个类C的语言，他的面向对象的东西相对于C++/Java比较奇怪。第二是Javascript作为一种语法极其灵活的语言，直接导致了面向对象书写JS又有多种写法，让许多初学者分不清到底哪个才是正确的写法。基于上述和个人的经验推荐如下俩种书写方式：

### 第一类：

```
(function(){
    function Circle(nRadius){
        this.nR = nRadius;
    }
    Circle.prototype = {
        PI : 3.14,
        fnGetArea : function(){
            return this.PI * this.nR * this.nR;
        }
    }
 
    var c1 = new Circle(5);
    alert(c1.fnGetArea()); //78.5
})();
```
上面这种可以说是很标准的面向对象JS书写方式了我们又称之为工厂模式，优点就是简单容易上手，新手常常喜欢这么写。以上代码略微做些改动，会有如下这个变种：


```
(function(){
    function Circle(nRadius, sMessage){
        this.init.apply(this, arguments);
    }
    Circle.prototype = {
        init : function(nRadius, sMessage){
            this.nR = nRadius;
            this.sMessage = sMessage;
        },
        PI : 3.14,
        fnGetArea : function(){
            return this.sMessage + ": " + this.PI * this.nR * this.nR;
        }
    }
 
    var c = new Circle(5, "构造初始化 面积");
    alert(c.fnGetArea()); //构造初始化 面积: 78.5
})();
```

上面这个变种，就比较有意思了，this.init.apply(this, arguments);这行代码把初始化的任务交接给了init()方法，这么做的好处就是可以把所有初始化的东西都放在一个地方进行，增加可阅读性，需要理解一定的Javascript运行机制原理。


###第二类：

```
(function(){
    function Circle(){
    }
    Circle.prototype = {
        init : function(nRadius, sMessage){
            this.nR = nRadius;
            this.sMessage = sMessage;
        },
        PI : 3.14,
        fnGetArea : function(){
            return this.sMessage + ": " + this.PI * this.nR * this.nR;
        }
    }
 
    var c = new Circle();
    c.init(5, "手动构造初始化 面积");
    alert(c.fnGetArea()); //手动构造初始化 面积: 78.5
})();
```

这类写法是我现在比较喜欢的写法，简洁高效。从书写角度来看省去了构造函数初始化属性，改用其init()中初始化。另一个好处在于不在new Circle()构造之初传入参数，个人认为当new构造对象的时候，最好不要掺杂参数，这样做很“危险”，改为“手动型”初始化更易于代码排查修改。当然这种写法还有一个原因就是他可以很好的转换成一般前端接受的“封装型”代码，我们把上面的代码也略微改动一下：


```
(function(){
    var Circle = {
        init : function(nRadius, sMessage){
            this.nR = nRadius;
            this.sMessage = sMessage;
        },
        PI : 3.14,
        fnGetArea : function(){
            return this.sMessage + ": " + this.PI * this.nR * this.nR;
        }
    }
 
    Circle.init(5, "封装型 面积");
    alert(Circle.fnGetArea()); //封装型 面积: 78.5
})();
```


是不是对上面这类代码很熟悉，很多网站上的例子都是使用这类封装型代码书写的，优点是代码的封装性良好，可以有效的重用，多用于页面功能性效果实现，封装一个Tab控件、封装一个跑马灯效果等等。缺点就是不能很好的用作继承，这是和上面三种格式最大区别。可话又说回来一般JS代码很少会用到继承的地方，除非是写一个大型库（类似YUI）会用到继承外，一般写一个功能模块用到封装型代码就够用了，所以大多数前端喜欢使用这类封装型的书写风格。
上面介绍了2类4种面向对象的写法，一般面向对象书写格式基本都在上面了，熟悉面向对象书写可以有效的增加你对JS的理解。熟练使用上面4中写法也能够很好的在工作中给代码维护修改带来便利。最后我们再来谈一个技巧，让你的Javascript代码在技巧上进化。
用对象字面量构造对象
一个对象字面量就是包含在一对花括号中的0个或多个“名/值”对。上文在面向对象书写格式的时候我们就大量的使用了对象字面量的书写格式。
对象字面量书写Javascript可以很好的简化代码，又能极大的增加代码可读性，尤其作为参数使用可以有化腐朽为神奇的表现。我们看下面代码：

```
(function(){
    function Person(sName, nAge, nWeight, bSingle){
        this.sName = sName;
        this.nAge = nAge;
        this.nWeight = nWeight;
        this.bSingle = bSingle;
    }
    Person.prototype.showInfo = function(){
        return this.sName + " " + this.nAge + " " + this.nWeight + " " + this.bSingle;
    }
    var p = new Person("海玉", 25, 75, true);
    alert(p.showInfo()); //海玉 25 75 true
})();
```

上面是一个很标准的工厂模式，一般而言这类写法属于那种规规矩矩没有大错也没有亮点的代码，而且参数不少，一个不小心还会传入错误的参数，而应用对象字面量技巧可以很好的规避此类问题，我们来看改动过后的代码：

```
(function(){
    function Person(){
    }
    Person.prototype = {
        init : function(option){
            if(typeof option == "undefined"){
                option = {};
            }
            this.sName = option.sName || "海玉";
            this.nAge = option.nAge || 25;
            this.nWeight = option.nWeight || 75;
            this.bSingle = (typeof option.bSingle != "undefined") ? option.bSingle : true;
        },
        showInfo : function(){
            return this.sName + " " + this.nAge + " " + this.nWeight + " " + this.bSingle;
        }
    }
    var p = new Person();
    p.init({
        nWeight : 80,
        sName : "Hank"
    })
    alert(p.showInfo()); //Hank 25 80 true
})();
```

改成封装型的 

```
(function(){
    var Person = {
        init: function(option){
            if(typeof option == "undefined"){
                option = {};
            }
            this.sName = option.sName || "Mick";
            this.nAge = option.nAge || 25;
            this.nWeight = option.nWeight || 68; 
        
        },
        showInfo: function(){
            return this.sName + " " + this.nAge + " " + this.nWeight;
        }
    
    }
    Person.init({
        sName: "Micky",
        nAge: 28
    });
    alert(Person.showInfo());

})();
```

这里使用第三种面向对象写法，有兴趣的朋友可以自行尝试改成封装型写法。上面的改写看出哪里改动最大吗？对的，传入参数改成了一个对象字面量，而且传入参数可以是随意设置，位置颠倒也不会有任何问题。这里充分利用对象字面量优点，利用键值对代替原始的传参方式大大提升了可读性和容错性。还有一个改进就是默认值的处理，如果没有传入任何参数，此代码也能很好的运行下去，不会有任何问题。


注1：这里形参命名为option，没有遵守上面的变量命名规范是为了方便书写与阅读，具体情况具体分析。
注2：由于||运算符对于布尔值默认赋值会出现赋值问题，所以要先进行判断是否为undefined，再利用三元运算符可以很好的完成布尔值的默认值赋值。

### 总结
总的来说上面阐述的代码进化只能算的上是“修身”层面，想要真正的让代码“修心”还是得多写，多看，多练。只有这样你的代码才会更精练，更有扩展性，更好的维护性。
林林总总写了这些个总结，一来是对自己学习的记录，二来也是想同大家探讨JS还有哪些地方有潜力可挖，脑子里还有许多零零碎碎的片段，待日后再次整理验证再与大家一起分享吧。
