---
layout: post
category : 服务端开发
tagline: "nohup"
tags : [service, ubuntu]
---
{% include JB/setup %}

nohup ./command.sh > output 2>&1 & 

主要是中间的 2>&1的意思 
这个意思是把标准错误（2）重定向到标准输出中（1），而标准输出又导入文件output里面。


###需要注意的
在mac上如果使用了tmux，再使用nohup命令有可能会报detached 错误，这是因为nohup命令需要把这个命令和当前的会话解绑，而tmux本身就做了解绑的操作。切回非tmux模式使用nohpu就没问题了。