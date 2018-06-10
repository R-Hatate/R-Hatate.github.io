---
layout: post
title: 奇技淫巧
category: 信息学竞赛
tags:
  - 奇技淫巧
published: true
---

# 各种lca的奇技淫巧

## 两个点的lca

[tarjan离线算法](https://www.cnblogs.com/JVxie/p/4854719.html) $O(n + q)$

[rmq在线算法](https://www.cnblogs.com/scau20110726/archive/2013/05/26/3100812.html) $O(n \cdot log_2n) - O(1)$

[倍增在线算法](https://www.cnblogs.com/FuTaimeng/p/5655616.html) $O(n \cdot log_2n) - O(log_2n)$

[树链剖分在线算法](https://www.cnblogs.com/fuyun-boy/p/6045709.html) $O(n) - O(log_2n)$

## 多个点的lca

[求k个点的lca](https://www.zhihu.com/question/46440863/answer/165741734)

## 树上信息统计

[静态子树信息统计](http://blog.csdn.net/qq_35392050/article/details/64537364)

# 数学

## 求零点

**牛顿迭代法**

$ x=x_0–\frac{f(x_0)}{f′(x0)} $

**徒手开根号(来源: [课件部分/Math/P60](http://hzwer.com/8847.html))**

求 $ \sqrt C $ 相当于求 $ f(x)=x^2 - C $ 的零点

$ x = x_0 - \frac{f(x_0)}{f'(x_0)} = x_0 - \frac{x_0 ^ 2 - C}{2 \cdot x_0} = \frac{1}{2} \cdot (x_0 + \frac{C}{x_0}) $

## 莫比乌斯反演

**[莫比乌斯反演证明](https://www.zhihu.com/question/50251847)**

**求狄利克雷卷积(来源: [5.1题外话](http://vfleaking.blog.uoj.ac/slide/87))**

$ f(n) = \sum_{d \| n}g(d) $

``` cpp
for(int i = 1 ; i <= n ; ++ i)
    g[i] = f[i];
for(int i = 1 ; i <= n ; ++ i)
    for(int j = i + i ; j <= n ; j += i)    
        g[j] -= g[i];
```

## 打表

### 分段打表

数据范围 $ 10^9 $

每 $ 10^3 $ 打一个表, 剩下的$ O(10^6) $ 暴力计算

### 常用打表

1. 博弈论中的SG函数
2. 组合数学的题猜结论
3. 贪心题猜结论

打完表之后放到excel中凑一凑

# 平衡树

## 正常向

[fhq treap](http://fanhq666.blog.163.com/blog/static/819434262011021105212299/) 除了LCT外都可以用(LCT不能用因为复杂度多一个log)

[kd tree](http://blog.csdn.net/QAQ__QAQ/article/details/54016323) k维分块? 时间复杂度 $ O(n^{1-\frac{1}{k}}) $ 替罪羊树的近亲

[01trie](https://magichut.pw/?p=100) 维护数集! 空间复杂度 $ O(n \cdot \log_2n) $

[三行splay](http://blog.csdn.net/wbysr/article/details/17056277)

## 关于treap的一个小技巧

``` cpp
srand((unsigned long long) new char);
for(int i = 1 ; i <= n + m ; ++ i)
    fix[i] = i;
random_shuffle(fix + 1, fix + 1 + n + m);
```

## 偷懒专用

**基于操作的重构**

每操作 $ O(\sqrt n) $ 次暴力sort

# 二分

[各种写法](https://www.zhihu.com/question/36132386)

# 随机数

## 避免无法使用time(0)的尴尬处境

``` cpp
srand((unsigned long long) new char);
```

## 生成若干个不同的随机数(比如说treap的fix数组)

``` cpp
srand((unsigned long long) new char);
for(int i = 1 ; i <= n + m ; ++ i)
    fix[i] = i;
random_shuffle(fix + 1, fix + 1 + n + m);
```

# 二分图

## 最大匹配

不带权: 最大流/匈牙利算法

带权: 费用流

## 完全二分图生成树计数

[给定一个左部分n个点，右m个点的完全二分图，求生成树个数](https://www.cnblogs.com/chouti/p/5814651.html)

$ n^{m-1} \cdot m^{n-1} $

# 树

## 树分块

[随机化分块](https://www.zhihu.com/question/60674478/answer/178873326)

[真-树分块](http://blog.csdn.net/popoqqq/article/details/42772237)

# 编译器

## 手动开O2/O3

``` cpp
%:pragma GCC optimmize(2)
%:pragma GCC optimmize(3)
```

## 手动开栈 [from konnyakuxzy](http://www.k-xzy.xyz/archives/4496)

``` cpp
int size = 128 << 20; // 128 MB
char* p = new char[size] + size;
__asm__ __volatile__(  
    "movq %0, %%rsp\n"
    "pushq $exit\n"
    "jmp main2\n"
    :: "r"(p));
```

## 去掉栈保护 [from konnyakuxzy](http://www.k-xzy.xyz/archives/4496)

``` cpp
%:pragma GCC optimize("Ofast,no-stack-protector")
```

## 优化$cpu$指令 [from konnyakuxzy](http://www.k-xzy.xyz/archives/4496)

``` cpp
%:pragma GCC target("avx")
```

## 优化大全 [from 轻如晨曦](https://www.luogu.org/space/show?uid=69441)

``` cpp
%:pragma GCC optimize("Ofast")
%:pragma GCC target("sse3","sse2","sse")
%:pragma GCC target("avx","sse4","sse4.1","sse4.2","ssse3")
%:pragma GCC target("f16c")
%:pragma GCC optimize("inline","fast-math","unroll-loops","no-stack-protector")
%:pragma GCC diagnostic error "-fwhole-program"
%:pragma GCC diagnostic error "-fcse-skip-blocks"
%:pragma GCC diagnostic error "-funsafe-loop-optimizations"
%:pragma GCC diagnostic error "-std=c++14"
```
