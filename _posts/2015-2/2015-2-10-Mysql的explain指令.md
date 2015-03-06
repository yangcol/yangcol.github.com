---
layout: post
category : 服务端开发
tagline: "mysql的explain指令"
tags : [mysql, explain]
---

在mysql中使用explain函数，可以看到sql语句的执行计划

	explain select * from statdata where name='all' and `date` < '2015-02-10';

	+----+-------------+----------+------+---------------+---------+---------+-------+--------+-------------+
	| id | select_type | table    | type | possible_keys | key     | key_len | ref   | rows   | Extra       |
	+----+-------------+----------+------+---------------+---------+---------+-------+--------+-------------+
	|  1 | SIMPLE      | statdata | ref  | PRIMARY       | PRIMARY | 130     | const | 548683 | Using where |
	+----+-------------+----------+------+---------------+---------+---------+-------+--------+-------------+

##EXPLAIN列的解释：

table：显示这一行的数据是关于哪张表的

type：这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为const、eq_reg、ref、range、indexhe和ALL

possible_keys：显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从WHERE语句中选择一个合适的语句

key： 实际使用的索引。如果为NULL，则没有使用索引。很少的情况下，MYSQL会选择优化不足的索引。这种情况下，可以在SELECT语句中使用USE INDEX（indexname）来强制使用一个索引或者用IGNORE INDEX（indexname）来强制MYSQL忽略索引

key_len：使用的索引的长度。在不损失精确性的情况下，长度越短越好

ref：显示索引的哪一列被使用了，如果可能的话，是一个常数

rows：MYSQL认为必须检查的用来返回请求数据的行数

##Cardinality基数
1. cardinality简单的说就是，你索引列的唯一值的个数，如果是复合索引就是唯一组合的个数。
2. 这个数值将会作为mysql优化器对语句执行计划进行判定时依据。如果唯一性太小，那么优化器会认为，这个索引对语句没有太大帮助，而不使用索引。
3. cardinality值越大，就意味着，使用索引能排除越多的数据，执行也更为高效。


##MySQL如何查询当前正在运行的SQL语句
作者：zhaolinjnu出处：IT专家网2008-04-21 08:30
　　通过status命令，查看Slow queries这一项，如果值长时间>0,说明有查询执行时间过长
以下是引用片段：
　　mysql> status; 
　　-------------- 
　　mysql Ver 11.18 Distrib 3.23.58, for redhat-linux-gnu (i386) 
　　Connection id: 53 
　　Current database: (null) 
　　Current user: root@localhost 
　　Current pager: stdout 
　　Using outfile: '' 
　　Server version: 5.0.37-log 
　　Protocol version: 10 
　　Connection: Localhost via UNIX socket 
　　Client characterset: latin1 
　　Server characterset: latin1 
　　UNIX socket: /tmp/mysql.sock 
　　Uptime: 4 days 16 hours 49 min 57 sec 
　　Threads: 1 Questions: 706 Slow queries: 0 Opens: 177 Flush tables: 1 Open tables: 52 Queries per second avg: 0.002 
　　--------------
　　这时再通过show processlist命令来查看当前正在运行的SQL，从中找出运行慢的SQL语句，找到执行慢的语句后，再用explain命令查看这些语句的执行计划。
　　mysql> show processlist;
　　+----+------+-----------+------+---------+------+-------+------------------+
　　| Id | User | Host | db | Command | Time | State | Info |
　　+----+------+-----------+------+---------+------+-------+------------------+
　　| 53 | root | localhost | NULL | Query | 0 | NULL | show processlist |
　　+----+------+-----------+------+---------+------+-------+------------------+
　　1 row in set (0.00 sec)
