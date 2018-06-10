---
layout: post
title: ODT
category: 信息学竞赛
tags: [ODT, 平衡树]
---

# 起源

[codeforces 896 C](http://codeforces.com/contest/896/problem/C)

> 给定一个序列$a_i$，长度为$n(n \le 10^5)$
> 有$m(m \le 10^5)$次操作
>
> - `1 l r x` $\forall i \in [l,r],a_i \leftarrow a_i + x$
> - `2 l r x` $\forall i \in [l,r],a_i \leftarrow x$
> - `3 l r x` 输出$[l,r]$的第$k$小，保证$1 \le x \le r-l+1$
> - `4 l r x y` 输出$(\sum\limits_{i=l}^{r}a_i^x) \text{ mod } y$
> 
> **保证数据随机**

看起来不可做的样子~~实际上真的不可做~~，不过由于数据随机生成，那么会有一些神奇的性质

~~看起来和 **Segment tree Beats!** 一样毒瘤~~

# 怎么做呢

**ODT**的核心思想是**推平**，即区间赋值~~所以为什么不叫Bulldozer Tree呢~~

以下翻译自[ODT的题解](http://codeforces.com/blog/entry/56135)

> 我们可以发现，有一个操作可以使得一段区间里所有数字都相同（即操作$2$）
> 我们可以用一棵平衡树（比如说std::set）来维护每一个数字都相同的区间
> 对于操作$2$，我们可以删掉平衡树上$[l,r]$中的所有的区间，并且添加一个新的区间$[l,r]$到平衡树上
> 比如说原先平衡树上有$[1,2],[3,3],[4,5]$，现在要删掉$[3,4]$，那么平衡树上的区间变成了$[1,2],[3,4],[5,5]$，当然要实现一个split操作，来诸如$[4,5]$这种区间
> 对于操作$1,3,4$，我们可以暴力的在树上提取出这些区间，然后在上面操作
> **时间复杂度证明**
> 
> > 我们假设现在随机的选取了一个区间$[l,r]$，之后我们随机的选择一个操作，假设当前平衡树上$[l,r]$中有$x$个区间
> > $\frac{1}{4}$的概率我们用$O(x)$的时间去删除$O(x)$个节点
> > $\frac{2}{4}$的概率我们用$O(x)$的时间苟着
> > $\frac{1}{4}$的概率我们用$O(x)$的时间苟着，并相平衡树添加两个新节点
>
> 所以我们期望用$O(x)$的一件去删除$O(x)$个点
> 在用平衡树维护的同时，这道题的时间复杂度是$O(m \text{log} n)$
> 如果操作$3$和操作$4$变为输出$\sum\limits_{i=l}^{r}a_i$，那么时间复杂度看起来变为$O(m \text{log} \text{ log} n)$，但我不会证……

然而为什么一定要用std::set实现，如果序列长度下降的很快的话，直接用数组就行了

~~所以我选择用std::set实现~~

胡乱看一看**推平**操作的推平区间长度的期望是多少，即从序列中随机取出一个子区间的长度的期望

区间总个数为（即长度为$1$、长度为$2$等区间的个数和）

$$
T=n+(n-1)+(n-2)+ \cdots + 1=\frac{n(1+n)}{2}
$$

长度的期望为

$$
E=\frac{n }{T} \times 1 + \frac{n-1}{T} \times 2 + \cdots + \frac{(n-(n-1))}{T} \times n
$$

$$
ET=n \times 1 + (n-1) \times 2 + \cdots (n-(n-1)) \times n
$$

$$
ET=n + 2n + 3n + \cdots + n \cdot n - (1 \times 2 + 2 \times 3 +  \cdots + (n-1)n)
$$

$$
ET=\frac{n^2(1+n)}{2}-(1 \times (1 + 1) + 2 \times(2+1) + \cdots + (n-1) \times ((n-1) + 1))
$$

$$
ET=\frac{n^2(1+n)}{2}-(1^2 + 2^2 + \cdots + (n-1)^2) - (1 + 2 + \cdots (n-1))
$$

$$
ET=\frac{n^2(1+n)}{2}-\frac{(n-1)((n-1)+1)(2(n-1) + 1)}{6}-\frac{(1+(n-1))n}{2}
$$

$$
ET=\frac{n^2(1+n)}{2}-\frac{n(n-1)(2n-1)}{6}-\frac{n^2}{2}
$$

$$
E\frac{n(n+1)}{2}=\frac{n^3}{2}-\frac{n(n-1)(2n-1)}{6}
$$

$$
E=\frac{3n^3-n(n-1)(2n-1)}{3n(n+1)}=\frac{3n^3-n(n-1)(2n-1)}{3n(n+1)}
$$

$$
E=\frac{n(n^2+3n-1)}{3n(n+1)}=\frac{(n+1)(n+2)-3}{3(n+1)}=\frac{n}{3}+(\frac{2}{3}-\frac{3}{n+1})
$$

$$
E \approx \frac{n}{3}
$$

也就是说推一次就合并了$\frac{1}{3}$左右的区间咯？看起来的确是log级别的~~大概~~

# 实现

[代码](http://codeforces.com/contest/896/submission/37654105)

本身就是直接暴力$\cdots$

# 两道例题

- codeforces 896 C
- codeforces 915 E