---
dpn: rails-many-2-many
layout: post
category : 服务端开发
tagline: "rails的many to manay 样例"
tags : [ubuntu, tmux]
---
{% include JB/setup %}

Rails中的关键字has_many以及belongs_to用来描述多对多的实体关系

举个例子来说，我们需要把公司的图书进行归档登记，每本书有一个名字，有一个或者多个作者，同时，一个作者可能写了不止一本书，我们得到的实体关系图如下图所示。
![关系图](/public/img/relationship.png)

有两个实体类，一个是book，另一个是author，他们之间是many to many的关系，还需要一个book与author关系的实体。
所以，如果对应到数据库当中，我们应当由三张表

* author
* book
* author_book_ship

###author
author的表含有主键id以及名称name

在author的model中，我们这么声明

		class Author < ActiveRecord::Base
		  	has_many :authorbookships
  			has_many :books, :through => :authorbookships
		end

###book
book的表含有主键id及书名name

在book的model中，我们这么声明

		class Book < ActiveRecord::Base
			has_many :authorbookships
			has_many :authors, :through => :authorbookships
		end


###author_book_ship

book的表含有主键id及author的id，book的id

在book的model中，我们这么声明

		class Authorbookship < ActiveRecord::Base
		  	belongs_to :book, :foreign_key => 'book_id'
		  	belongs_to :author, :foreign_key => 'author_id'
		end

##TODO未完成
