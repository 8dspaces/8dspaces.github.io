---
layout: post
category : blog
title: "sqlalchemy笔记"
tags : [blog, python,sqlalchemy]
---


<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>

#SQLAlchemy笔记

SQLAlchemy是最优秀的Python ORM(Object Relational **Mapper**) library


## SA layer
 
+ Raw sql   

    `seesion.execute("SELECT * FROM users;")`
    
    防止sql Injectiion   
    `session.execute(text("DELETE FROM students WHERE id = :id", {id: 3}))`
    
+ sql Expression Language(Level 1)

    `select([users]).all()`
    
+ ORM(Level 2)

    `Session.query(User).all()`
    
## engine
一切的开始……  
用于和不同的数据库建立连接

    from sqlalchemy import create_engine
    
    engine = create_engine(r'sqlite://db.sqlite', echo = False)
    
    
## declative 

+ old style mapping(still work)   

        # 创建Table
        users_table = Table('users', metadata,



+ new style  
新的方式，使用declative 方式，将table和类的创建和mapper自动完成
        
        from sqlalchemy.ext.declarative import declarative_base
         
        Base = delcarative_base()

        class User(Base): 
            __tablename__ = "users"
            

        metadata = Base.metadata
        
        metadata.create_all(engine)









Query 是数据库最基本的操作

+ User.query.get(**primary_key**)


### Query --> filter (对应where)

    User.query.filter(User.username == 'rick')



 `.one() `    - exception 
 `.first() `  - None
 `.all() `    - empty list


 `.group_by()`
 `.count()`
 `.order_by()`
 `.limit()`
 `.having()`


 ### autoload  

      # does a query against the database at load time to 
      # load the columns
    

 ### autoload declarative 



 ### fitting to an existing db


+ ### Python properties 

      class User(Base):


