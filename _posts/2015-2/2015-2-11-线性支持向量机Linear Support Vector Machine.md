---
layout: post
category : 算法
tagline: "线性支持向量机"
tags : [PLA, ]
---
{% include JB/setup %}

PLA?depending on randomness

VC bound? whichever you like!

	Eout(w) <= Ein(w) + Omiga(h)

Gaussian-like
测试资料与训练资料有一些区别（测量误差）

同样是线性拟合，好的超平面拥有更好的对测量误差的容忍度。（costfunction会更小！）

考虑到高斯噪声，样本离超平面越远，就有更高的噪声容忍度。

<==> more robust to overfitting 
<==> robustness of hyperplane

定义一个线的分类效果，就是看这个线离最近的资料的距离，越远越好。

robustness === fatness distance to closes xn

找出最fat线。


max fatness(w)

subject to  w classifies every(xn, yn) correctly
			fatness(w) = min distance(xn, w)


fatness: formally called margin

Large-Margin Separating Hyperplane


Distance to Hyperplane

max w marg(w)

subject to     		exery ynwT xn > 0
					margin(w) = min n=1,...,N distance(xn,w)

shorten x and w
distance needs w0 and w1,...,wd differently (to be derived)

w0 取名b(bias截距)

//
h(x) = sign(wT * x + b

wT是平面的法向量

//

distance(x, b, w) = 