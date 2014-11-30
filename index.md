---
layout: page
title: 我的进步阶梯
tagline: Stay foolish, Stay hungry ...
---
{% include JB/setup %}


    
## 我的博客

写一些自己在学习中的总结或是一些别人的好的分享，具体包括

+ 测试总结（主要工作）
+ 测试自动化的一些东西(selenium, robotfrawork)
+ 敏捷测试(关注点和方向）
+ Python（热爱的语言，有了它QA更强大）
+ Javascript/Web前端技术(Bootstrap)
+ 其他... 


### 近期文章：

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



