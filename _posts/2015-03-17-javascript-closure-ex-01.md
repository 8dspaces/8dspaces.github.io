---
layout: post
category : blog
title: "一个关于使用闭包的例子"
tags : [javascript]
---


在给一个element 数组循环指定事件时，经常会出现错误，而是用闭包可以很好的解决这样的问题 


        <!DOCTYPE HTML>
        <html lang="en-US">
        <head>
            <meta charset="UTF-8">
            <title></title>
        </head>
        <body>
            <p id="help">Helpful notes will appear here</p>
            <p>E-mail: <input type="text" id="email" name="email"></p>
            <p>Name: <input type="text" id="name" name="name"></p>
            <p>Age: <input type="text" id="age" name="age"></p>
            <script type="text/javascript">
        function showHelp(help) {
          document.getElementById('help').innerHTML = help;
        }

        function setupHelp() {
          var helpText = [
              {'id': 'email', 'help': 'Your e-mail address'},
              {'id': 'name', 'help': 'Your full name'},
              {'id': 'age', 'help': 'Your age (you must be over 16)'}
            ];


        /*wrong example 
          for (var i = 0; i < helpText.length; i++) {
            var item = helpText[i];
            document.getElementById(item.id).onfocus = function() {
              showHelp(item.help);
            }
          }
          
            数组 helpText 中定义了三个有用的提示信息，每一个都关联于对应的文档中的输入域的 ID。通过循环这三项定义，依次为每一个输入域添加了一个 onfocus 事件处理函数，以便显示帮助信息。
            运行这段代码后，您会发现它没有达到想要的效果。无论焦点在哪个输入域上，显示的都是关于年龄的消息。
            该问题的原因在于赋给 onfocus 是闭包（showHelp）中的匿名函数而不是闭包对象；在闭包（showHelp）中一共创建了三个匿名函数，但是它们都共享同一个环境（item）。在 onfocus 的回调被执行时，循环早已经完成，且此时 item 变量（由所有三个闭包所共享）已经指向了
            helpText 列表中的最后一项。
        */    

          for (var i = 0; i < helpText.length; i++) {
            (function(i){
                var item = helpText[i];
                document.getElementById(item.id).onfocus = function() {
                      showHelp(item.help);
                }
            })(i)
          }
        }

        setupHelp();
            
        </script>

        </body>
        </html>
