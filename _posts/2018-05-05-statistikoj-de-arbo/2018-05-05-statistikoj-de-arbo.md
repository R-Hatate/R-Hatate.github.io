---
layout: post
title: 树上统计
category: 信息学竞赛
tags: [差分, 前缀和, 树分块, 启发式合并, dsu on tree]
---

# 0x00 前言

信息学竞赛中有一类题，诸如对于静态树上的每一个节点，统计它所率的子树的节点的一些信息，时间复杂度一般要求 $O(n) \sim O(n \cdot logn)$，本文讨论一些常见的解决方法，与这些方法背后的异曲同工之处。

# 0x01 树上差分

树上差分一般用于处理链上或者子树上的累加和，换句话来说是计算贡献的一种打标记方式。

> 给定一棵树，以及若干次操作，每次诸如将路径(u,v)中的点权加上一个值，求最终所有点的点权。

设tag[]为最终答案，对于修改(u, v, val)，转换为

``` cpp
tag[u] += val;
tag[v] += val;
tag[lca(u, v)] -= val;
tag[fa[lca(u, v)]] -= val;
```

并在dfs的同时统计答案

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

对于u，它的tag会使得它到根节点所有节点的权值都加上该值， tag[u] += val;tag[v] += val;  会使u和v为起点两条到根的路径的权值都加上tag。

此时lca(u, v)到根的权值被加了两次，对于tag[lca(u, v)] -= val;会使lca(u, v)到根的权值减了一遍 tag 。

而fa[lca(u, v)]到根实际不应该被增加，所以还应tag[fa[lca(u, v)]] -= val; 将 fa[lca(u, v)] 到根的影响去除。
    
> 给定一棵树，以及若干次操作，每次诸如将路径 (u,v) 中的边权加上一个值，求最终所有边的边权。

与上一个例子同理，只不过将 u 的点权作为它到父边的边权了，此时根节点的点权无意义。

此时打标记的方法为

``` cpp
tag[u] += val;
tag[v] += val;
tag[lca(u, v)] -= val;
```

是不是少了一步？因为 lca(u, v) 实际意义是 lca(u, v) 到 fa[lca(u, v)] 的边权，这个不在路径 (u, v) 上，所以只有三步tag 。
    
> 例 $ 3 $：给定一棵树，每个点有一个 $ 1 \sim n $ 的颜色，对于每一个点给了一个数 $ k $ ，求对于每一个点，求以这个点位根的子树中颜色为 $ k $ 的点的个数。

设 cnt 为某种颜色的出现次数，然后在 dfs 的时候维护差分。

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

> 给定一个树,若干次操作,每次选择一条路径,往路径上每个点上放一个i类型的物品,问最终每个点上,最多的物品是哪个类型。——罗翔宇《数据结构例题选讲》

用打tag的方式处理路径，然后维护众数    

> noip2016D1T2 天天爱跑步

显然对于每一个人，他只会对所经过的路径上的观察员产生贡献（也有可能不产生贡献）。

将路径(u, v) 拆分成 (u, k) 和 (lca(u, v), v) ，其中 fa[k] == lca(u, v) 且 (u, k) 是 (u, lca(u, v)) 的子路径。

对于 (u, k) ，如果观察员 x 在 (u, k) 上，且满足 deep[x] + time[x] == deep[u] ，则 x 会得到 $ 1 $ 的贡献（即这个人会被 x 看到）。

对于 (lca(u, v), v) ，如果观察员 x 在 (lca(u, v), v) 上，且满足 deep[v] - deep[x] == len(u, v) - time[x] ，则 x 会得到 $ 1 $ 的贡献。

定义 cnt[2]\[\] 表示某个下标出现次数，(id, val, flag) 表示 cnt[id]\[val\] += flag; ，由于 val可能为负，所以需要加上offset。
    
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

# 0x02 树的启发式合并

可以解决许多无修改询问子树的问题。

经典例题就是合并两棵树，做法就是将大小小的那棵树拆点后一个一个添加进去。

例题： SDOI2013 森林

顺便一提，合并（这里只添加一条边后成为一颗新树）两棵树后重心只会沿着这两棵树原先的重心在合并之后的唯一路径上移动最多一条边。

以及并查集也是树，所以可以根据并查集的size 进行启发式合并，然后可以进行暴力爬链的操作了（忘了那道经典的例题了）。
    
# 0x03 dsu on tree

算法流程

（ son[] 表示重儿子， fa[] 表示父亲， add() 表示加入该点为根的子树的贡献， del() 表示删除该点为根的子树的贡献， sol() 表示计算该点的贡献。）

``` cpp
void dfs(int u) {
    for(int i = head[u], v ; i ; i = rest[i]) {
        if((v = to[i]) != fa[u] && v != son[u]) {
            dfs(v);
            del(v);
        }
    }
    if(son[u]) dfs(son[u]);
    for(int i = head[u], v ; i ; i = rest[i]) {
        if((v = to[i]) != fa[u] && v != son[u]) {
            add(v);
        }
    }
    add(u);
    sol(u);
}
```

时间复杂度 $ O(n \cdot logn \cdot T(合并)) $

两篇文章

[[Tutorial] Sack (dsu on tree) - Codeforces](http://codeforces.com/blog/entry/44351)

[[trick] dsu on tree](http://blog.csdn.net/QAQ__QAQ/article/details/53455462)

# 0x04 基于深度的树分块

[ruanxingzhi](https://www.zhihu.com/question/60674478/answer/178873326)

# 0x05 基于连通性的树分块

SCOI2005 王室联邦