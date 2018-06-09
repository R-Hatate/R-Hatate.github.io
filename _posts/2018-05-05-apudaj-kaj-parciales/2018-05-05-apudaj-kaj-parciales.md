---
layout: post
title: apudaj kaj parciales
category: 信息学竞赛
tags: [差分, 前缀和]
---

# 差分

## 定义

定义一个数列 $\\{ a_i \| 1 \le i \le n \\}$ 的差分数列为

$$
\{ b_i | 1\le i \le n, b_1=a_1,b_{i(i \ge 2)}=a_{i}-a_{i-1}\}
$$

## 实现

### 手写

``` cpp
a[0] = 0;
for(int i = 1 ; i <= n ; ++ i) b[i] = a[i] - a[i - 1];
```

### STL

``` cpp
adjacent_difference(a + 1, a + 1 + n, b + 1);
```

## 本质

观察差分的定义式，可以发现差分本质上是关于数列的**“微分”**

# 前缀和

## 定义

定义一个数列 $\\{a_i \| 1 \le i \le n \\}$ 的前缀和数列为

$$
\{ b_i | 1\le i \le n, b_i=\sum_{k=1}^{i}a_k\}
$$

## 实现

### 手写

``` cpp
b[1] = a[1];
for(int i = 2 ; i <= n ; ++ i) b[i] = b[i - 1] + a[i];
```

### STL

``` cpp
partial_sum(a + 1, a + 1 + n, b + 1);
```

## 本质

观察前缀和的定义式，可以发现前缀和的本质上是关于数列的**“积分”**

# 题外话……

如何将一个数列读进来？

``` cpp
copy(istream_iterator<int> { cin }, istream_iterator<int> {}, a + 1);
```

那么输出呢？

``` cpp
copy(a + 1, a + 1 + n, ostream_iterator<int> {cout, " "});
```

奇技淫巧， 不足挂齿

# 差分和前缀和在序列上的问题

## A

> 给你一个数列，有若干次询问，每次询问**区间和**
>
> 即如果询问$l,r$，你需要输出$\sum\limits_{i=l}^{r}a_i$

在高中数学课上，曾经学过数列是$a_i=f(i)$这样定义的，所以本质上数列是一种**函数**

那么只需要将数列积分后，查询**“面积”**即可

所以前缀和之后输出$b_r-b_{l-1}$就是答案

## B

> 给你一个数列，有若干次**区间加**操作
>
> 即如果修改$l,r,c$，那么$\forall i \in [l,r]$，$a_i \leftarrow a_i+c$
>
> 在所有的修改之后，需要$\forall i \in [1,n]$，输出$a_i$

既然数列都是函数了，**区间加**操作充其量就是一个修改相邻两项的**斜率**

然后两两抵消了，只剩下两端的修改

那么就相当于对$f'(x)$的每一次进行两次**单点加**，最后进行一次**前缀和**

即$b\_l \leftarrow b\_i+c,b\_{r+1} \leftarrow b\_{r+1} - c$，最后求前缀和后输出

## C

> 给你一个数列，初值为$0$
>
> 有若干次操作，每次诸如**区间加上一个等差数列**
>
> 最后询问每一个位置上的值

可以发现，等差数列的**首项**、**公差**是可以**叠加**的

差分之后直接维护就好

如果要支持在线修改和询问区间和呢？线段树！

## D

> 区间加$k$次多项式
>
> 最后求每个点的值

差分$k$次后成为区间加，最后前缀和回来

细节可以参考[rxz的pdf](http://ruanxingzhi.coding.me/File/%E8%AF%BE%E4%BB%B6/%E5%B7%AE%E5%88%86%E4%B8%8E%E5%89%8D%E7%BC%80%E5%92%8C.pdf)

## E

> 有一个$n \times m$的网格，初始状态是空的
>
> 有$𝑘$次操作
>
> 每次操作指定一个矩形（给出四个顶点），铺上地毯
>
> 最后询问有多少个点没有被铺地毯

二维差分后，成为四个点的加法，最后二维前缀和，扫一遍看看那些点是$0$

## F

> noip 2012 tg 借教室

二分答案+前缀和后判定

# 树上差分

树上差分一般用于处理链上或者子树上的累加和，换句话来说是计算贡献的一种打标记方式

# 差分在树上的应用

## A

> 给定一棵树，以及若干次操作
>
> 每次诸如将路径$(u,v)$中的点权加上一个值，求最终所有点的点权

设$tag[]$为最终答案，对于修改$(u, v, val)$，转换为

``` cpp
tag[u] += val;
tag[v] += val;
tag[lca(u, v)] -= val;
tag[fa[lca(u, v)]] -= val;
```

并在$dfs$的同时统计答案

``` cpp
void dfs(int u, int fa) {
    for(int i = head[u], v ; i ; i = rest[i]) {
        if((v = to[i]) != fa) {
            dfs(v, u);
            tag[u] += tag[v];
        }
    }
}
```

原理？

对于$u$，它的$tag$会使得它到根节点所有节点的权值都加上该值

$u$和$v$的$tag$分别加上$val$，会使$u$和$v$为起点两条到根的路径的权值都加上$tag$

此时$lca$到根的权值被加了两次

对于把$lca$的$tag$减去$val$，会使$lca$到根的权值减了一遍$tag$

而$fa[lca]$到根实际不应该被增加，所以还应将其$tag$减去$val$，将其到根的影响去除

## B

> 给定一棵树，以及若干次操作
>
> 每次诸如将路径$(u,v)$中的边权加上一个值，求最终所有边的边权

与上一个例子同理，只不过将$u$的点权作为它到父边的边权了，此时根节点的点权无意义

此时打标记的方法为

``` cpp
tag[u] += val;
tag[v] += val;
tag[lca(u, v)] -= val;
```

是不是少了一步？

因为$lca$实际意义是$lca$到$fa[lca]$的边权，这个不在路径$(u, v)$上，所以只有三步$tag$ 

## C

> 给定一棵树，每个点有一个$1 \sim n$的颜色
>
> 对于每一个点给了一个数$k$
>
> 对于每一个点，求以这个点位根的子树中颜色为$k$的点的个数

设$cnt$为某种颜色的出现次数，然后在$dfs$的时候维护差分

答案如下统计

``` cpp
void dfs(int u, int fa) {
    int mem = cnt[k[u]];
    cnt[color[u]] ++;
    for(int i = head[u], v ; i ; i = rest[i]) {
        if((v = to[i]) != fa) {
            dfs(v, u);
        }
    }
    ans[u] = cnt[k[u]] - mem;
}
```

## D

> 给定一个树，若干次操作
>
> 每次选择一条路径，往路径上每个点上放一个$i$类型的物品
>
> 问最终每个点上，最多的物品是哪个类型

对于$(u,v,lca)$，可以拆分成$u$和$v$分别放一个物品，$fa[lca]$和$lca$分别删掉一个物品

维护一下众数就好

## E

> noip 2016 tg 天天爱跑步

显然对于每一个人，他只会对所经过的路径上的观察员产生贡献（也有可能不产生贡献）

将路径$(u, v)$拆分成$(u, k)$和$(lca, v)$，其中$fa[k] == lca$且$(u, k)$是$(u, lca)$的子路径

对于$(u, k)$，如果观察员$x$在$(u, k)$上，且满足$deep[x] + time[x] == deep[u]$，则$x$会得到$1$的贡献（即这个人会被$x$看到）

对于$(lca, v)$，如果观察员$x$在$(lca, v)$上，且满足$deep[v] - deep[x] == len(u, v) - time[x]$，则$x$会得到$1$的贡献

定义$cnt[2][]$表示某个下标出现次数，$(id, val, flag)$表示$cnt[id][val]$加上$flag$，由于$val$可能为负，所以需要加上$offset$

``` cpp
struct T {
    int id, val, f;
};
vector<T> tag[N];
void addTag(int u, int v) {
    int d = getLca(u, v);
    int len = deep[u] + deep[v] - (deep[d] << 1);
    tag[u].push_back((T) {1, deep[u], 1});
    tag[d].push_back((T) {1, deep[u], -1});
    tag[v].push_back((T) {2, deep[v] - len + offset, 1});
    tag[fa[d][0]].push_back((T) {2, deep[v] - len + offset, -1});
}
int calc(int u) {
    return cnt[1][deep[u] + w[u]] + cnt[2][deep[u] - w[u] + offset];
}
void sol(int u) {
    int old = calc(u);
    for(int i = 0 ; i < tag[u].size() ; ++ i) {
        int id = tag[u][i].id;
        int val = tag[u][i].val;
        int f = tag[u][i].f;
        cnt[id][val] += f;
    }
    for(int i = head[u], v ; i ; i = rest[i]) {
        if((v = to[i]) != fa[u][0]) {
            sol(v);
        }
    }
    ans[u] = calc(u) - old;
}
```

## F

> bzoj 2588
>
> 给定一棵$n$个点的树，点有点权
>
> 有$m$次询问
>
> 每次求树上两点之间路径上第$k$大的点权
>
> 强制在线

每个点以其父亲节点为$last$建立主席树

下标是点权，值是这个权值出现次数
$$
w[u,v]=w[u]+w[v]-w[lca]-w[fa[lca]]
$$
