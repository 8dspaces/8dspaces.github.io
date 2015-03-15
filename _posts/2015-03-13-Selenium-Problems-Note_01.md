---
layout: post
category : blog
title: "Selenium 遇到的问题(一) "
tags : [test, selenium, python]
---

Selenium 遇到的问题  

最近一个项目使用selenium和python的unittest模块对一个web application做自动化测试，从头开始积累
把遇到的一些问题记下来

参考   

+ [Selenium with Python — Selenium Python Bindings 2 documentation](http://selenium-python.readthedocs.org/en/latest/index.html)
+ [python - Selenium and iframe in html - Stack Overflow](http://stackoverflow.com/questions/18924146/selenium-and-iframe-in-html)
+ [unit testing - How to get selenium to wait for ajax response? - Stack Overflow](http://stackoverflow.com/questions/2835179/how-to-get-selenium-to-wait-for-ajax-response)
+ [Shadow DOM — WebComponents.org](http://webcomponents.org/polyfills/shadow-dom/)

1.Select options 

        from selenium.webdriver.support.ui import Select
        drop_list = Select(browser.find_element_by_id("#testid"))

        drop_list.select_by_value(value)
        # drop_list.select_by_index(index)
        # drop_list.select_by_visible_text(text)
        
        # it's web element object, need use x.text to get the text 
        options = drop_list.options
        
    对于支持多选的选择列表，可以调用`select_by_x`多次来实现  

        lists.select_by_value("549")
        lists.select_by_value("548")
        lists.select_by_value("547") 
        
2.等待Ajax控件  

    在测试Ajax控件的时候经常需要等控件加载完毕才能获取和操作

        from selenium.webdriver.support import expected_conditions as EC
        from selenium.webdriver.common.by import By
        from selenium.webdriver.support.ui import WebDriverWait
        
        wait = WebDriverWait(browser, 10)
        # Expected Conditions(EC) 可以支持很多方式
        menu = wait.until(EC.element_to_be_selected((By.XPATH,r"/html/body/core-scaffold/core-header-panel/core-menu/core-submenu[2]")))

    如果大部分操作都需要一段时间加载，也可以改变全局等待时间

        from selenium import webdriver

        driver = webdriver.Firefox()
        driver.implicitly_wait(10) # seconds
        driver.get("http://somedomain/url_that_delays_loading")
        myDynamicElement = driver.find_element_by_id("myDynamicElement")
        
3.对iframe的支持 

    当页面中有iframe时，必须切换到iframe所有操作才会起作用 
        
    Selenium 中可用的方法
        switch_to_frame(frame_reference)
        switch_to_default_content()
        switch_to_window(window_name)
        ......
        
    Example：

        from selenium import webdriver
        
        browser.switch_to_frame(browser.find_element_by_tag_name("iframe"))


### 待解决问题  

+ polymer 的shadow DOM, 很难用selenium获取和操作  
+ 目前项目规模不大，unittest还可以应付，不知是否需要使用robotframewrok这样的框架  
