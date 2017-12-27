---
layout: post
category : blog
title: "斐波那契数列优化"
tags : [blog, python]
---

总结几种对斐波那契数列的优化方式  

传统的，利用未优化的递归求斐波那契数列第n项， 对于讲解递归来说是个好例子，但效率却非常糟糕

    def fab_1(n):
        if 0< n <=2:
            return 1
        else:
            return fab_1(n-1) + fab_1(n-2)
 
    print fab_1(30)


+ **利用字典优化递归** 
 
传统的方式有问题是因为有大量的重复计算，利用字典把重复的值缓存下来是常见的优化方式


    previous = {1: 1L, 2: 1L}
    def fab_2(n):
        if previous.has_key(n):
            return previous.get(n)
        else:
            newvalue = fab_2(n-1) + fab_2(n-2)
            previous[n] = newvalue
            return newvalue
     
    print fab_2(30) # 832040




+ **利用迭代得到斐波那契数列第n项** 

递归往往是可以转化为迭代的，这样免去了缓存的麻烦。当然不是所有递归都能很容易转为迭代


    def diedai_fab(n):
        x, y = 1, 1
        while n > 1:
            x, y = y, x + y
            n = n - 1
        return x
    print diedai_fab(30)

+ **迭代器itertools** 

生成器绝对是一个强大的工具，利用Python自带的迭代器库可以实现许多常见的功能。 


    def fab_3():
        x, y = 1, 1
        while True:
            yield x
            x, y = y, x+y
        
    from itertools import islice
    for i in islice(fab_3(), 10):
        print i 


当然，这只是一个简单的例子，重要的是从简单的例子中的一些思想

+ 字典缓存
+ 递归迭代互转

