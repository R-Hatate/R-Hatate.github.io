---
layout: post
title: 不等式杂题
category: 数学
tags: [数学, 不等式]
---

# 1

## 题目描述

已知$\frac{x}{a}+\frac{y}{b}=1$经过点$(\cos \alpha, \sin \alpha)$，求$\frac{1}{a^2}+\frac{1}{b^2}$的最小值

## 题解

### 解法1

先化简直线方程：$bx+ay=ab$

可以得知点$(\cos \alpha, \sin \alpha)$是单位圆上一点，即$(0,0)$到该直线的距离不超过$1$

即
$$
d=\frac{|b \cdot 0+a \cdot 0 - ab|}{\sqrt {a^2+b^2}} \le 1
$$
即
$$
a^2b^2 \le a^2 + b^2
$$
即
$$
\frac{1}{a^2}+\frac{1}{b^2} \ge 1
$$

### 解法2

将点代入直线方程得
$$
b \cos \alpha + a \sin \alpha = ab
$$
观察左式，得
$$
b \cos \alpha + a \sin \alpha=\sqrt {a^2 + b^2} \cos (\beta - \alpha)
$$
其中$tan \beta = \frac{a}{b}$

即
$$
b \cos \alpha + a \sin \alpha=\sqrt {a^2 + b^2} \cos (\beta - \alpha) \le \sqrt {a^2 + b^2}
$$
即
$$
\sqrt {a^2 + b^2} \le ab
$$
即
$$
\frac{1}{a^2}+\frac{1}{b^2} \ge 1
$$

### 解法3

将点代入直线方程后，相当于
$$
\overrightarrow{(b,a)} \cdot \overrightarrow{(\cos \alpha, \sin \alpha)} = ab
$$
即
$$
\sqrt {b^2+a^2} \cdot \sqrt {\cos^2 \alpha + \sin^2 \alpha} \cdot \cos \beta = ab
$$
即
$$
ab \le \sqrt {a^2 + b^2}
$$
即
$$
\frac{1}{a^2}+\frac{1}{b^2} \ge 1
$$

# 2

## 题目描述

已知正数$a,b,c$满足$a+b+c=1$，求$\sqrt {3a + 1} + \sqrt {3b + 1} + \sqrt {3c + 1}$的最大值

## 题解

构造$\sqrt {\frac{1}{x}(3a+1) \cdot x} \le \frac{\frac{1}{x}(3a+1)+x}{2}$，且满足$\frac{1}{x}(3a+1) = x$当且仅当$a=\frac{1}{3}$

解得$x = \sqrt 2$

即
$$
\begin{align}
&\sqrt{\frac{1}{\sqrt 2}(3a+1) \cdot \sqrt 2}+\sqrt{\frac{1}{\sqrt 2}(3b+1) \cdot \sqrt 2}+\sqrt{\frac{1}{\sqrt 2}(3c+1) \cdot \sqrt 2}\\
\le & \frac{\frac{1}{\sqrt 2}(3a+3b+3c+3)+3 \sqrt 2}{2}\\
= & 3 \sqrt 2
\end{align}
$$
即
$$
\sqrt {3a + 1} + \sqrt {3b + 1} + \sqrt {3c + 1} \le 3 \sqrt 2
$$

# 3

## 题目描述

已知$a_i$为实数，$b_i$为正数，求证
$$
\Pi \frac{a_i^2}{b_i} \ge \frac{(\sum a_i)^2}{\sum b_i}
$$


## 题解

柯西不等式是说，对于$a_i$和$b_i$，有如下不等式成立
$$
(\sum a_i^2)(\sum b_i^2) \ge (\sum a_ib_i)^2
$$
在这道题中，不妨如下转化
$$
\sum (\frac{a_i}{\sqrt b_i})^2 \ge \frac{(\sum \frac{a_i}{\sqrt b_i} \times \sqrt b_i)^2}{\sum (\sqrt b_i)^2}
$$
移项后便是柯西不等式的形式

即
$$
(\sum (\frac{a_i}{\sqrt b_i})^2)(\sum (\sqrt b_i)^2) \ge (\sum \frac{a_i}{\sqrt b_i} \times \sqrt b_i)^2
$$
