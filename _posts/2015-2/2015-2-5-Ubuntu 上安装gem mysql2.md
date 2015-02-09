---
dpn: ubunt-gem-mysql2
layout: post
category : service
tagline: "ruby gem mysql开发包安装"
tags : [mysql, gem, rails, mysql2]
---
{% include JB/setup %}

# 安装ubuntu需要的必要组件
安装mysql2的gem文件时，需要进行编译，引用的头文件是包libmysqld-dev提供的，
执行

		sudo apt-get install libmysqld-dev
