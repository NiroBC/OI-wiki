## 定义

（还记得这些定义吗？在阅读下列内容之前，请务必了解 [图论基础](/graph/basic) 部分）

生成子图

生成树

最小生成树：边权和最小的生成树

注意：只有连通图才有生成树，而对于非连通图，只能搞出生成森林

## Kruskal 算法

是一种常见并且好写的最小生成树算法

由 Kruskal 发明

基本思想是从小到大加入边，是个贪心算法

### 证明

思路很简单

为了造出一棵最小生成树

我们从最小边权的边开始，按边权从小到大依次加入

如果某次加边产生了环，就扔掉这条边

直到加入了 n-1 条边，即形成了一棵树

证明：使用归纳法，证明任何时候 K 算法选择的边集都被某棵 MST 所包含

基础：对于算法刚开始时，显然成立（最小生成树存在）

归纳：假设某时刻成立，当前边集为 F，令 T 为这棵 MST，考虑下一条加入的边 e

如果 e 属于 T，那么成立

否则，T+e 一定存在一个环，考虑这个环上不属于 F 的另一条边 f（一定只有一条）

首先，f 的权值一定不会比 e 小，不然 f 会在 e 之前被选取

然后，f 的权值一定不会比 e 大，不然 T+e-f 就是一棵比 T 还优的生成树了

所以，T+e-f 包含了 F，并且也是一棵最小生成树，归纳成立

### 实现

算法虽简单，但需要相应的数据结构来支持……

具体来说，维护一个森林，查询两个结点是否在同一棵树中，连接两棵树

抽象一点地说，维护一堆 **集合**，查询两个元素是否属于同一集合，合并两个集合

我们先啥都不管，假设已经实现了这个数据结构……

（伪代码）
```
for (edge(u, v, len) in sorted(edges)) {
	a = find_set(u), b = find_set(v);
	if (a != b) merge(a, b);
}
```

find_set 调用 $O(m)$ 次，merge 调用 $O(n)$ 次

排序的复杂度为 $O(m \log m)$，或 $O(m)$（假设能基数排序）

### “集合”数据结构的一种实现

只要支持两个接口：find_set 和 merge

我们先考虑暴力，直接维护每个元素属于哪个集合，以及每个集合有哪些元素

find_set：$O(1)$

merge：$O(n)$，需要将一个集合中的所有元素移到另一个集合中

于是考虑如何优化 merge

一个简单的思路是，将较小的集合中所有元素移到较大的集合中

复杂度是 $O(较小集合的大小)$

那么总时间复杂度是多少呢？

我们换一个角度分析，考虑每个元素对每次合并操作的贡献

很显然，一个元素所在的集合大小，在作为较小集合被合并一次之后，至少增加一倍

所以一个元素所在的集合，最多有 $\log n$ 次，作为较小集合被合并

一共$n$个元素，所以总时间复杂度为 $O(n \log n + m)$

这种做法或者思想，叫“启发式合并”

总之我们得到了 $O(n \log n + m \log m)$ 的Kruskal算法

## Prim 算法

是另一种常见并且好写的最小生成树算法
基本思想是从一个结点开始，不断加点（而不是 Kruskal 算法的加边）

### 证明

从任意一个结点开始

将结点分成两类：已加入的，未加入的

每次从未加入的结点中，找一个与已加入的结点之间边权最小值最小的结点

然后将这个结点加入，并连上那条边权最小的边

重复 n-1 次即可

证明：还是说明在每一步，都存在一棵最小生成树包含已选边集

基础：只有一个结点的时候，显然成立

归纳：如果某一步成立，当前边集为 F，属于 T这棵 MST，接下来要加入边 e

如果 e 属于 T，那么成立

否则考虑 T+e 中环上另一条可以加入当前边集的边 f

首先，f 的权值一定不小于 e 的权值，否则就会选择 f 而不是 e 了

然后，f 的权值一定不大于 e 的权值，否则 T+e-f 就是一棵更小的生成树了

因此，e 和 f 的权值相等，T+e-f 也是一棵最小生成树，且包含了 F

### 实现

也是需要一些数据结构来支持

具体来说，每次要选择距离最小的一个结点，以及用新的边更新其他结点的距离

等等，这很像 Dijkstra 算法……

其实跟 Dijkstra 算法一样，只要一个堆来维护距离即可

暴力：$O(n^2+m)$

二叉堆：$O((n+m) \log n)$

Fib堆：$O(n \log n + m)$

（伪代码）
```
H = new heap();
for (i = 1; i <= n; i++) H.insert(i, inf);
H.decrease_key(1, 0);
for (i = 1; i <= n; i++) {
	u = H.delete_min();
	for each edge(u, v, len) {
		H.decrease_key(v, len);
	}
}
```

注意：上述代码只是实现了 Prim 算法主体，如果要输出方案还需要记录额外的信息

注意：在遍历边表 `(u, v)` 时，如果 v 已经被 delete，就无需 decrease key

## 最小生成树小结

我们介绍了两种最小生成树的算法，各有特点

然后我们来考虑这样一些问题

一张图的最小生成树不一定是唯一的

什么时候一定唯一？

考虑 Kruskal 算法，当每条边权都不一样时，一开始的排序只有一种方案，就一定唯一了

那什么时候一定不唯一？

Kruskal算法中的“集合”，能否进一步优化？

## 次小生成树

## 第 k 小生成树