---
layout: post
category : 大数据
tagline: "hadoop入门"
tags : [hadoop]
title: "在mac上安装hadoop"
---

##环境

使用OS X Yosemite, MAC上已安装homebrew


##步骤1:
	
	brew install hadoop

这里安装的是hadoop 2.6.0

##步骤2:
进入hadoop目录
	$ cd /usr/local/Cellar/hadoop/2.6.0/libexec

如果目录结构不一样，可以使用

	$ ls -l $(which hadoop)

查看hadoop的执行文件在哪个目录下

##步骤3:
试一下能否ssh localhost

	$ ssh localhost

如果不能正确执行，尝试下面的办法:

* 编辑/etc/sshd_config文件，注释掉
#ForceCommand /usr/local/bin/ssh_session

启动sshd服务:

	$ sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist


查看是否启动：

	$ sudo launchctl list | grep ssh

如果看到下面的输出表示成功启动了:

	－－－－－－－－－－－－－－
	- 0 com.openssh.sshd


最后可以ssh localhost 成功

##步骤4:

试一下单机能不能玩起来，hadoop默认是可以单机跑。
	
	$ cd /usr/local/Cellar/hadoop/2.6.0/libexec
	$ mkdir input
	$ cp etc/hadoop/*.xml input
	$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep input output 'dfs[a-z.]+'
	$ cat output/*

如果有正常的日志信息即表明正常。

##步骤5:
组成单节点的集群。

修改etc/hadoop/core-site.xml:

	<configuration>
	    <property>
	        <name>fs.defaultFS</name>
	        <value>hdfs://localhost:9000</value>
	    </property>
	</configuration>

修改etc/hadoop/hdfs-site.xml:

	<configuration>
	    <property>
	        <name>dfs.replication</name>
	        <value>1</value>
	    </property>
	</configuration>

##步骤6：
执行例子
    
*格式化文件系统*

	$ bin/hdfs namenode -format

*开启NameNode和DataNode守护进程:*

	$ sbin/start-dfs.sh

hadoop守护进程的日志输出到$HADOOP_LOG_DIR目录(默认是$HADOOP_HOME/logs)。

打开浏览器指向NameNode地址，在这里是http://localhost:50070/

*创建HDFS目录来执行MapReduce任务:*
      
	$ bin/hdfs dfs -mkdir /user
	$ bin/hdfs dfs -mkdir /user/<username>

*把输入文件拷贝到分布式文件系统:*

	$ bin/hdfs dfs -put etc/hadoop input

*执行hadoop提供的例子:*

      $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep input output 'dfs[a-z.]+'

*检查输出文件:*

把输出文件从分布式文件系统拷贝到本地并且进行检查:

	$ bin/hdfs dfs -get output output

	$ cat output/*

或者:

直接查看分布式文件系统:
 
	$ bin/hdfs dfs -cat output/*

如果不再使用，把守护进程停掉:

	$ sbin/stop-dfs.sh

## 在浏览器上查看

http://localhost:50075/