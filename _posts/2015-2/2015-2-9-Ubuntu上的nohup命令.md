---
layout: post
category : 服务端开发
tagline: "nohup"
tags : [service, ubuntu]
---
{% include JB/setup %}

nohup ./command.sh > output 2>&1 & 

主要是中间的 2>&1的意思 
这个意思是把标准错误（2）重定向到标准输出中（1），而标准输出又导入文件output里面， 