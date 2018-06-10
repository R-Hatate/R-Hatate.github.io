---
layout: post
title: 非旋转平衡树
category: 信息学竞赛
tags: [非旋转平衡树, 平衡树]
---

# 所以这是啥……

在维护各种数据的时候，常用的方法是平衡树套上一堆东西

普通的平衡树主要面临几个问题

1. 代码超长不易调试
2. 不可持久化
3. 维护序列和维护集合的写法千差万别

~~当然splay还是很有用的，LCT全靠它~~

非旋转treap是一个很有实用性的平衡树，核心操作只有split和merge

如果是维护的数集的话，可能还需要一个ls操作来查询小于等于某个数的个数

# split(x,k)

## 用途

将子树x的前k个节点和后n-k个节点分成两个子树，保证分成后的子树的先序遍历依然满足原序列的先后顺序

## 实现

``` cpp
pair<int, int> split(int x, int k) {
    push(x);
    pair<int, int> res;
    if(k <= sz[ch[x][0]]) {
        res = split(ch[x][0], k);
        ch[x][0] = res.second;
        res.second = x;
    } else {
        res = split(ch[x][1], k - sz[ch[x][0]] - 1);
        ch[x][1] = res.first;
        res.first = x;
    }
    update(x);
    return res;
}
```

是不是很简单

# merge(x,y)

## 用途

将子树x和子树y合并，返回合并后的新树

## 实现

``` cpp
int merge(int x, int y) {
    push(x), push(y);
    if(!x || !y) return x | y;
    if(fix[x] < fix[y]) {
        ch[x][1] = merge(ch[x][1], y);
        return update(x), x;
    } else {
        ch[y][0] = merge(x, ch[y][0]);
        return udpate(y), y;
    }
}
```

似乎比split操作还简单

# ls(x,val)

## 用途

查询小于等于val的数的个数

## 实现

``` cpp
int ls(int x, int val) {
    push(x);
    if(!x) return 0;
    if(val >= :: val[x]) {
        return sz[ch[x][0]] + 1 + ls(ch[x][1], val);
    } else {
        return ls(ch[x][0], val);
    }
}
```

利用二叉树的性质计算

# 所以到底能干啥……

## 维护序列

通过split操作，可以提取区间，并打标号实现各种区间操作

## 维护数集

如果将数字的大小关系当作顺序的话，这也是一个维护序列的操作了，通过split和ls可以精准定位，之后该干啥干啥

# 可持久化平衡树

可以发现，如果将merge和split中的修改ch[x]改为新建节点并复制ch[x][0]和ch[x][1]，之后返回新建节点的话，就实现了可持久化平衡树

# 关于fix的小技巧

``` cpp
srand((unsigned long long) new char); // cstdlib
iota(fix, fix + N, 0); // numeric
random_shuffle(fix, fix + N); // algorithm
```