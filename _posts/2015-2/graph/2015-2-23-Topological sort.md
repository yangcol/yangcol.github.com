---
layout: post
category : 算法
tagline: "dag有向图"
tags : [dag, graph, topology sort]
---

>这是coursera上的princeton算法课程的读书笔记å
>这个princeton公开课的[源地址](http://algs4.cs.princeton.edu)，如果对课程感兴趣可以在coursera上搜这一门课程。

DAG => Directed acyclic graph. 有向无环图

Topological sort. Redraw DAG so all edges point upwards.

让所有的节点向上指

Acyclic

Topological sort in a DAG: correctness proof
￼Proposition. Reverse DFS postorder of a DAG is a topological order.
￼Pf. Consider any edge v→w. When dfs(v) is called: ・Case 1: dfs(w) has already been called and returned.
dfs(0)
  dfs(1)
dfs(4)
4 done 1 done
  dfs(2)
  2 done
  dfs(5)
check 2
5 done 0 done
check 1
check 2
dfs(3)
  check 2
  check 4
  check 5
  dfs(6)
check 0
check 4
6 done 3 done
check 4
check 5
check 6
done
Thus, w was done before v.
・Case 2: dfs(w) has not yet been called.
dfs(w) will get called directly or indirectly by dfs(v) and will finish before dfs(v). Thus, w will be done before v.
・Case 3: dfs(w) has already been called,
but has not yet returned.
Can’t happen in a DAG: function call stack contains path from w to v, so v→w would complete a cycle.



Topological sort的证明
反转的DFS postorder是DAG的topological顺序

证明如下：考虑任意的边 v->w。当dfs(v)被调用
有下面几种情况

Case 1 => dfs(w)已经被调用，并且已经被返回。因此w在v之前被返回。

Case 2 => dfs(w)没有被调用。那么，由于v->w有直接的边或者非直接的边，那么，一定会调用dfs(w)。然后w也被返回。

Case 3 => dfs(w)已被调用，但是没有被返回，这样就会存在一个w ->v的边，构成环了，然后就会与DAG矛盾。

## Directed cycle detection
如何检测一个环



如果监测一个图有没有环?
Proposition：A digraph has a topological order iff no directed cycle
命题: 一个图，如果有拓扑顺序，那么就没有环
证明: 
	如果有环，那么就没有拓扑顺序（在上面的拓扑顺序已经证明了这一点）
	如果没有环，那么就可以找到一个拓扑顺序，使用DFS-based算法可以找到这样的拓扑顺序。

**还有其他方法吗？**
当然有。在学习链表的时候，有可能面临过这样的问题: 怎么判断链表是否有环？
一种方法是，遍历一个链表，一个采用步数为2，一个采用步数为1，如果他们在某个时候相遇(刚好走到同一个节点)，那么，这个链表是有环的。

#TODO 图有环的其他方法

