---
layout: post
category : 服务端开发
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

## 下面再继续总结一下mysql的导出命令

## mysqldump命令
mysqldump 命令的参数和mysql的执行类似

比如，我们现在要把某个数据库完整导出，可以用下面的命令:

	mysql -h 主机名 -D 数据库名 -u 用户名  -p > 目标文件

	example:
		mysql -h localhost -D test -u test -p > testdb.sql

导出的文件是一个完整地sql。可以直接让mysql执行执行。

	mysql -h localhost -p -Dtest <testdb.sql

上面的命令在执行时可能会需要下面的错误信息：

	ERROR 1839 (HY000) at line 24: 
	@@GLOBAL.GTID_PURGED can only be set when @@GLOBAL.GTID_MODE = ON

如果仅仅是做测试的话，可以把导出的sql文件当中的那两行删除掉，然后再执行就没问题了，或者在mysqldump时，增加–gtid-mode参数。

	mysql -h localhost -D test -u test -p -gtid-mode=OFF> testdb.sql


mysqldump还可以增加其他参数，比如表名称和筛选条件

	mysqldump -h1.1.1.1  -uuser   -ppassword   -P3306 mydb mytb –where "time <= cast('2014-04-03 16:00' as datetime)" –skip-lock-tables –default-character-set=utf8 –gtid-mode=OFF > mytb.txt 
