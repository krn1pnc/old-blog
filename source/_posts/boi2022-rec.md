---
title: BalticOI 2022 做题记录
date: 2023-11-21 15:23:31
categories: 做题记录
---

Abltic．

<!-- more -->

## Day 1

### A. Art Collections

通过猜猜猜，发现将 $\begin{pmatrix} 1 & 2 & \cdots & n \end{pmatrix}$ 的循环左移全都问一遍就行了！

假设询问排列 $R = \begin{pmatrix} k & k + 1 & \cdots & n & 1 & \cdots & k - 1 \end{pmatrix}$ 得到的结果是 $a$，将其循环左移一位后得到 $R^\prime = \begin{pmatrix} k + 1 & \cdots & n & 1 & \cdots & k \end{pmatrix}$，设询问 $R^\prime$ 得到的结果为 $b$，发现若设 $P$ 中值为 $k$ 的元素所在的位置是 $p$，那么有 $b - a = n - p - p + 1$，可以得到 $p = (n + a - b + 1) / 2$．

### B. Event Hopping

从左往右跳时，由于我们只能跳到包含当前区间右端点的区间，故直接贪心选右端点最靠右的区间不对，会导致跳到终点后面回不来．

考虑倒着做，这样就可以跳到所有右端点在当前区间内的区间，此时贪心选左端点最靠左的区间就对了．

用个倍增优化跳跃过程即可．

### * C. Uplifting Excursion

背包大小很大，但是物品重量和价值都很小，所以我们考虑先贪心得到一个方案再调整．

不妨令 $\sum_i i \times a_i \ge L$，若不满足，直接将值域翻转即可．

考虑一个贪心策略：拿完 $\le 0$ 的数后从小到大选正数，保证不超过 $L$，直到不能再选．

不妨设贪心得到的重量为 $L^\prime$，则有 $L - L^\prime \le m$．由于我们最终要将重量调整到 $L$，且物品重量的绝对值不会超过 $m$，故可以调整物品的加入和删除顺序，使得在调整过程中达到的每个重量都在 $[L - m, L + m]$ 之内．What's more，若我们两次调整到了同一个重量，那么将中间的调整过程全都删去，不会使答案变劣．这是由于我们一开始的解是贪心得到的，调整过程中加入的物品重量一定大于删除物品的重量，也就是说加入的物品数量 $\ge$ 删除的物品数量，调整步数肯定越小越好．

故只会调整 $O(m)$ 次，背包值域 $O(m^2)$．对加入和删除跑多重背包即可，需要使用单调队列或二进制分组优化．

## Day 2

### ! A. Flight to the Ford

不会通信．

### B. Stranded Far From Home

考虑一个点权转边权，令边 $(u, v)$ 的权值为 $\max\{a_u, a_v\}$，然后建出 Kruskal 重构树，考虑从一个叶子节点开始，若子树大小大于父亲的权值那么就往上跳，那么一个颜色能给所有节点染上，当且仅当其能跳到根．从根开始 dfs 一遍即可．

### * C. Boarding Passes

题意是你可以安排组上船的顺序和每个人是从前面还是后面登船，但是组内的人上船的顺序随机，你需要最小化游客经过其他游客的期望数量．

单走一个状压，设 $f_S$ 为 $S$ 中的组已经上船的最小期望．考虑 $F_S \to F_{S \cup i}$ 的转移．记 $s$ 表示 $i$ 组内的人数，$i$ 组内的人肯定有一段前缀从前面上船，剩下的人从后面上船．

枚举前缀的长度 $k$，$i$ 组内造成的贡献为

$$
\frac{k(k - 1)}{4} + \frac{(s - k)(s - k - 1)}{4}
$$

记 $\mathrm{cost}_{i, j, k}$ 为 $i$ 组在 $j$ 组后登船，$i$ 组前 $k$ 个人从前面上船的贡献，容易 $(g^2n)$ 预处理．

但是这样避免不了 $O(n)$ 的转移，寄．但是注意到对于一对 $(i, j)$，$i$ 组和 $j$ 组之间的贡献是凸的，组内贡献是个二次函数，显然也是凸的，凸函数求和还是凸的！于是可以二分最大值．时间复杂度 $O(2^g \log n)$．
