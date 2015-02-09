---
dpn: mysql-2-sqlite
layout: post
category : service
tagline: "工作中碰到的一个问题"
tags : [mysql, sqlite]
---
{% include JB/setup %}

##介绍一个工作中遇到的问题

在开发的过程中，遇到这样一个问题，我这里的开发环境是使用mysql，产生了一批测试数据，需要把数据导出给前端同事测试用，前端同事使用的是sqlite3。

### 步骤1. 导出mysql的输入到csv格式

		select * into outfile '/tmp/statdata.csv' fields terminated by ',' enclosed by '"' escaped by '\\' LINES TERMINATED BY '\n' from statdata;


###步骤2. 导入数据到sqlite3

	在shell中执行以下命令：

		sqlite3 {targetdb}

		# 创建表结构
		create table statdata(
		`id` NOT NULL,
		`name` VARCHAR(32) NOT NULL, 
		`key` VARCHAR(128)  NOT NULL ,
		`duration` VARCHAR(32) NOT NULL,
		`date` DATETIME   NOT NULL,  
		`value` INT,
		PRIMARY KEY(`id`));

		.separator ','
		.import {filename}  {tablename}

	结束



###评论一下
开发环境和测试环境使用不同的数据库，我认为不是一个好的风格，即使sqlite3和mysql很多语句都通用，但是它们还是有不少区别，比如在查询语句的一些保留字的处理上。而且，如果需要使用mysql的一些特性，比方存储引擎，还是会带来环境迁移的问题的。
