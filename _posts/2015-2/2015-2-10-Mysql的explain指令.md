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

