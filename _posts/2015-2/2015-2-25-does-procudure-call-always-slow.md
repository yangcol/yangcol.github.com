---
layout: post
category :  翻译
tagline: "过程调用一定耗费更多吗"
tags : [mysql, index, primary key]
title: "过程调用一定比GOTO要慢吗?"
---

> 这是翻译1977年 Guy Lewis Steele Jr.的文章，反驳了过程调用比goto(比如循环，switch等)语句慢的说法。
> 其实直到现在，依旧有这样的言论存在。
> 等你把这篇论文看完后，你就会明白，过程调用不是非要比goto语句慢，如果真的慢了，是因为编译器不给力！

> 原文标题
> DEBUNKING THE "EXPENSIVE PROCEDURE CALL" MYTH or, PROCEDURE CALL IMPLEMENTATIONS CONSIDERED HARMFUL or, LAMBDA: THE ULTIMATE GOTO by Guy Lewis Steele Jr. *

[原文下载地址](http://repository.readscheme.org/ftp/papers/ai-lab-pubs/AIM-443.pdf)
[其他相关论文地址](http://library.readscheme.org/page1.html)
#摘要:

民间流传的说法是GOTO声明是很廉价的，过程调用是昂贵的。这种说法其实是语言设计的太糟糕了，而且这种说法还在变得更流行。在这篇文章中，从理论的思考以及一个已有的实现方式来打破这种说法。文章的结论表明：不限制过程调用的使用可以极大地提供文体(stylistic)的自由。尤其是，任意的流程图，都能够直接写成结构化的程序而不需要引入额外的变量。使用GOTO语句和过程调用的困难性被特征化为抽象程序概念和实际的语言构造之间的冲突。

关键词: 过程调用, 子过程调用, 结构化编程, 变成风格, 编译器, 优化

A. Procedure Calls as GOTO Statements
A. 过程调用可以被翻译为GOTO Statements

如果一个调用做得最后一件事情是调用另一个外部的过程，那么，就可以被编译为GOTO。这样的调用被称作尾递归，因为调用貌似是递归的，但其实不是的，因为它只出现在调用的结尾。

**在这里阐述的尾递归概念和网上（百度）上的概念有很大不一样，国内的尾递归概念经常概念上是同一个函数的调用，但其实，只要把这种尾递归最后的表达式看做一个额外的函数，也是符合上述尾递归的描述。**


B 过程调用可以很快！
Fateman(Fat73) further emphasized  this point. In actual timing tests conducted on numeric coding using maclisp compiler and the hen current DEC FORTRAN compilter, the MACLISP code was faster!
C 为什么过程调用拥有坏名声！


在很多时候，编译器在准备过程调用以及结束调用时需要申请所有申明的变量，有时候甚至要保存和存储所有的寄存器变量。
as currently implemented的方式上，过程调用确实很慢！

只要编译得当，过程调用可以很快！

D. 我们如何做这件事呢?


E.  如何表述过程调用？
