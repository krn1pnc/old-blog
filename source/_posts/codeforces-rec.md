---
title: Codeforces 杂题选记
date: 2023-09-24 20:58:57
categories: 做题记录
---

感觉往博客里塞了不少篇幅较短的文章，有点松散了，应当处理一下．

<!-- more -->

## [1864C Divisor Chain](https://codeforces.com/contest/1864/problem/C)

反过来思考整个过程，我们尝试从 $1$ 开始，给当前数加上自己的一个因数来凑出来 $x$．

可以发现 $2$ 的整数次幂是容易凑出的，因为任何数都是自己的因数，直接倍增即可．

如果我们凑出了 $x$ 二进制表示下的最高位，那么 $x$ 的其他位都是那个数的因数，而且都只被用了一次，那么直接凑上即可．

## [1864E Guess Game](https://codeforces.com/contest/1864/problem/E)

考虑模拟决策过程.

由于二人没有头发，他们会从最高位开始考虑，那么轮数就是 $a$ 和 $b$ 二进制表示的 LCP 中 $1$ 的个数乘 $2$ 加 $1 + [a > b]$．

求期望直接求所有情况的轮数和，然后除 $n^2$．

## [1858D Trees and Segments](https://codeforces.com/contest/1858/problem/D)

不妨设 $f_i$ 表示 $0$ 最长连续段长度为 $i$ 时，最长的 $1$ 连续段的长度．那么对于一个  $x$，答案即为 $\max\limits_i \{x \times i + f_i\}$．

考虑如何求出 $f_i$，首先我们可以钦定一个连续段 $[l, r]$ 为 $0$，不妨设需要的修改次数为 $\mathrm{ct}$，那么我们要求的就是 $[1, l - 1]$ 这个前缀和 $[r + 1, n]$ 这个后缀中，修改 $k - \mathrm{ct}$ 次能够得到的最长 $1$ 连续段，这个也可以简单 DP 求出．

## [79D Password](https://codeforces.com/contest/79/problem/D)

考虑原序列的差分序列，题意转化为：每次改变 $x$ 和 $x + a_i$ 的状态，将有 $\le 2k$ 个 $1$ 的序列变成全 $0$ 的．

首先可以通过 $n$ 次最短路计算出改变任意两个位置状态的代价．由于边权为 $1$，01 bfs 即可．

然后就直接设 $f_{S}$ 表示将 $S$ 内的位置全变成 $0$ 的最小代价，枚举仍不是 $0$ 的位置，转移即可．

## [1796D Maximum Subarray](https://codeforces.com/contest/1796/problem/D)

贪不动，尝试 DP．

先将所有 $a_i$ 减去 $x$，问题转化为要将原序列中某些位置加上 $2x$，最大化最大子段和．

考虑设 $f_{i, j}$ 表示在 $i$ 处结束的子段，加了 $j$ 次的最大和．

转移需要考虑当前点改不改，由于我们可以在 $i$ 之后的某些位置操作，所以取答案时，需要对所有满足 $1 \le i \le n, \max\{k - (n - i)\} \le j \le k$ 的 $f_{i, j}$ 取 $\max$．

注意不要转移到非法状态．注意不要转移到非法状态．注意不要转移到非法状态．

## [1764E Doremy's Number Line](https://codeforces.com/contest/1764/problem/E)

若 $k \le a_1$，那么可以直接涂掉；若 $k > a_1 + b_1$，那么不可能涂掉．我们只需要解决 $a_1 < k \le a_1 + b_1$ 的情况．

$1$ 显然要拿出来涂 $k$，所以先踢掉它．事实上，我们只需要判定抛开 $1$ 后，剩下的操作能涂到的最大位置是否 $\ge k - b_1$ 即可．

不妨设 $p = \argmax\limits_i a_i$，分类讨论其对能取到的最大右端点 $r$ 的贡献．

1. 不由 $p$ 取到，此时其他点都可以任意被涂了，所以 $r = \max_{i \not= p} a_i + b_i$．
2. 由 $p$ 取到，此时 $p$ 可以扩展剩余 $n - 2$ 个位置取到的最大值 $r^\prime$．

那么按照 $a_i$ 排序后取一遍 $\max$ 即可．

## [1817D Toy Machine](https://codeforces.com/contest/1817/problem/D)

容易 ~~通过瞎摁~~ 观察到，当 $k \le \lceil (n - 2) / 2 \rceil$ 时，可以通过 $k - 1$ 次 $\texttt{LDRU}$ 加上一次 $\texttt{L}$ 将 $k$ 转到最左边．

考虑 $k > \lceil (n - 2) / 2 \rceil$ 时怎么做，我们可以通过镜像上面的操作将目标点转到右上角，然后重复最多 $\lfloor n / 2 \rfloor$ 次 $\texttt{RDRU}$ 将所有的块堆到右上角，此时进行 $\texttt{RU}$ 操作就转化成了 $k = \lceil (n - 2) / 2 \rceil$ 的情况，直接做即可．

## [1852A Ntarsis' Set](https://codeforces.com/contest/1852/problem/A)

考虑二分最大的 $p$，使得 $[1, p]$ 都能在 $k$ 轮操作之后被删掉，答案显然就是 $p + 1$．

判定答案时，我们不妨换一下对删数操作的定义．若当前数的排名是 $x$，我们直接将删数操作定义为 $x \leftarrow x - \sum_i [a_i \le x]$．按照这个定义在没有被删除的元素上操作的结果和原定义是吻合的，而在被删除的元素上则带来了一些比原定义更好的性质．若存在某个 $a_i = x$，我们考察进行这个操作后会发生什么：

1. $[1, x]$ 中的元素被删空，此时 $\forall j \le i, a_j = j$，即本次删了一个前缀，操作一次之后 $x = 0$，正好对应删空．
2. $[1, x]$ 中的元素未被删空，此时 $\sum_i [a_i \le x] \not= x$，操作后 $x \not= 0$，正好对应没删空．

那么判定就直接暴力进行 $k$ 轮操作，判定 $x = 0$ 即可．

## [1852B Imbalanced Arrays](https://codeforces.com/contest/1852/problem/B)

首先观察到，如果同时存在 $a_i = n, a_j = 0$，那么一定无解，因为这要求 $b_i + b_j < 0$ 且 $b_i + b_j > 0$，显然矛盾．

考虑 $x = \argmax_i |b_i|$，那么 $a_x$ 只可能填 $0$ 或 $n$，具体填哪个取决于 $b_x$ 的符号．又由于 $-n \le b_i \le n, b_i \not= 0$，且不存在 $b_i + b_j = 0$，也就是说 $|b_i|$ 构成一个长度为 $n$ 的排列，所以如果 $a$ 中同时不存在 $0$ 或 $n$，那么也无解．

于是我们可以这样构造：从大到小分配 $n \sim 1$，每次找到 $a_i = 0$ 或 $a_i = n$，设当前最大没有分配的数为 $\mathrm{cur}$，若 $a_i = 0$，则 $b_i = -\mathrm{cur}$，否则 $b_i = \mathrm{cur}$．

显然可以在对 $a$ 排序后利用双指针优化．

## [1852C Ina of the Mountain](https://codeforces.com/contest/1852/problem/C)

即模 $k$ 意义下全都变成 $0$．如果不模 $k$，这不是我们积木大赛吗！若记 $b_i = a_i - a_{i - 1}$，那么答案就是 $\sum_i [b_i > 0]b_i$．

模 $k$ 可以看做预先给 $a$ 某些位置加 $k$，反映在差分数组上就是选择若干个二元组 $(i, j)$，其中 $i < j$，然后给 $b_i$ 加 $k$，给 $b_j$ 减 $k$．

于是问题转化为进行任意次上述操作，最小化 $\sum_i [b_i > 0]b_i$．

显然我们不会对一个正数加，也不会对一个负数减．于是我们可以从前往后考虑，每遇到正数，就找前面第一个没有使用过的、最小的负数，若使用后更优就直接用．由于放着最小值不用没有收益，所以正确性显然．

## [1848C Vika and Price Tags](https://codeforces.com/contest/1848/problem/C)

考虑 $n = 1$ 的情况，不妨设 $(x, y) = (a_1, b_1)$，然后就可以观察到如果已经达到了目标状态，即 $x = 0$，那么接下来进行的操作就会形如

$$
(0, y) \to (y, y) \to (y, 0) \to (0, y)
$$

周期为 $3$．

那么只需要算出每个位置上的数对达到目标状态的次数，然后判定 $\bmod\ 3$ 下是否相等即可．

考虑快速计算操作次数，不妨设 $a < b$，$b = ta + r$，状态变化形如

$$
(a, ta + r) \to (ta + r, (t - 1)a + r) \to ((t - 1)a + r, a) \to (a, (t - 2)a + r)
$$

周期也为 $3$，且每轮中 $t$ 会减 $2$．由于我们只关心操作次数 $\bmod\ 3$ 后的值，所以模拟操作时可以直接将 $k$ 模 $2$．然后讨论一下

1. $t \bmod 2 = 1$ 时，$(a, a + r) \to (a + r, r) \to (r, a)$，使用 $2$ 步操作转化为子问题，递归解决 $(r, a)$．
2. $t \bmod 2 = 0$ 时，直接递归解决 $(a, r)$．

$a > b$ 时类似．每次子问题规模至少减半，那么复杂度 $\Theta(n \log V)$，其中 $V$ 是值域．

## [1848D Vika and Bonuses](https://codeforces.com/contest/1848/problem/D)

观察可以发现，个位数的变化情况形如

![变化情况](/images/codeforces-rec/1.png)

容易发现 $2 \to 4 \to 8 \to 6$ 是一轮循环，且经过一轮循环之后 $s$ 会增加 $20$．

考虑对一个 $s(s \in \{2, 4, 6, 8\})$ 求解．设循环轮数为 $x$，由于我们一定先给 $s$ 增再累计，所以操作后的值 $w = (s + 20x)(k - 4x) = -80x^2 + (20k - 4s)x + sk$，是一个关于 $x$ 的二次函数，容易求出极值．然后对每个可能的 $s$ 做一遍就行了．

记得判掉 $0$ 和 $5$．

## [1848E Vika and Stone Skipping](https://codeforces.com/contest/1848/problem/E)

设 $x = f + (f - 1) + \cdots + (f - c + 1) = c(f + f - c + 1) / 2$，即以某个力 $f$ 丢出石子后，第 $c$ 次接触水面在 $x$ 处．

- 若 $c$ 是偶数，那么 $x = (c / 2)(2f - c + 1) = p(2k + 1)$，由于有 $f - c + 1 > 0$，所以 $p \le k$．
- 若 $c$ 是奇数，那么 $x = c(f - (c - 1) / 2) = (2k + 1)p$，此时有 $p > k$．

所以 $x$ 的每个奇因子 $2k + 1$ 都唯一对应一个 $p$，也唯一对应一个 $f$，对答案贡献恰好 $1$．那么问题转化为快速计算 $x$ 的奇因子数目．考虑 $x$ 的唯一分解 $x = \prod {p_i}^{a_i}$，那么答案就是 $\prod\limits_{p_i \not= 2} (a_i + 1)$．

由于直接维护会涉及逆元操作，而模数不是大质数，逆元不一定总存在，那么直接使用大炮打蚊子，拿个线段树维护．

## [1848F Vika and Wiki](https://codeforces.com/contest/1848/problem/F)

打个表先．$k$ 次操作后，$j$ 位置上的数会被异或到 $i$ 位置的次数是 $\binom{i}{j - i}$．

那么，第 $2^k$ 次操作后，$a_i$ 会变成 $a_i \oplus a_{i + 2^k \bmod n}$．注意到操作 $n$ 次后会变成 $0$，也就是说肯定存在解．

那么直接考虑从高位到低位确定答案，能填 $0$ 就填 $0$．

## [712D Memory and Scores](https://codeforces.com/contest/712/problem/D)

设 $f_{i, j}$ 表示进行了 $i$ 轮，分数差为 $j$ 时的局面数．

然而这样直接转移的话会带个诡异系数，不是很好优化，考虑直接玩 $2t$ 轮，每一轮只有一个人操作，那么有 $f_{i, j} \leftarrow f_{i - 1, p}$，其中 $p \in [j - k, j + k]$．

## [1846E Rudolf and Snowflakes](https://codeforces.com/contest/1846/problem/E)

即确定是否存在 $k$ 和 $n$ 满足 $\sum\limits_{i = 0}^{n} k^i = x$，其中 $k \ge 2, n \ge 2$．

考虑等比求和公式，上式等于 $(k^{n + 1} - 1) / (k - 1)$．顶上那个指数的可能数量是 $\log$ 级别的，于是考虑枚举 $n$，二分 $k$，最后检验是否合法就行．

注意精度和炸 `long long`．

## [1841E Fill the Matrix](https://codeforces.com/contest/1841/problem/E)

显然我们的策略是每次找到最长的横线填数，设长度为 $l$，那么贡献就是 $l - 1$．

考虑计算每种不同长度横线的个数，考虑一个位置 $i$，能作为以 $i$ 为右端点的横线的左端点只能是 $[1, i]$ 这段前缀中的后缀最大值，这个可以单调栈维护，然后在弹栈的时候记录一下前驱，就可以算数量了．

## [1840G1 In Search of Truth (Easy Version)](https://codeforces.com/contest/1840/problem/G1)

即构造一个长度 $\le 2023$ 的序列 $a_i$，使得 $\forall i \le 10^6, \exists l, r, \sum\limits_{k = l}^r a_k = i$．

考虑光速幂的构造，设置块长 $B$，显然有 $n = \lfloor n / B \rfloor B + n \bmod B$．那么前面丢 $B$ 个 $1$，后面丢 $\lfloor 10^6 / B \rfloor$ 个 $B$．显然 $B = \sqrt{10^6} = 10^3$ 时最优．

## [1839D Ball Sorting](https://codeforces.com/contest/1839/problem/D)

我们的策略是选出一个上升子序列，这个上升子序列将原序列划分成了若干部分，然后每个部分放一个白球，进行消除．

于是设 $f_{i, j}$ 表示考虑了 $[1, i]$ 这个前缀，$i$ 在上升子序列里，被划分出了 $j$ 部分，上升子序列的最长长度．

若 $a_{i - 1} < a_i$，那么可以直接继承 $f_{i - 1, j}$．否则直接转移．

注意答案要做前缀 $\min$．

## [1837E Playoff Fixing](https://codeforces.com/contest/1837/problem/E)

考虑从最上面开始往下推．设最上面为第一层．

考虑安排第 $i$ 层，此时第 $i - 1$ 层已经安排完毕，故只有新增的 $2^{i - 1}$ 个人的位置需要确定，若未钦定任何位置，那么答案需要乘上 $(2^{i - 1})! \times 2^{2^{i - 1}}$，前面那个因子是在考虑新加入的人的排列，后面那个因子是在考虑新加入的人在某场比赛中的先后．

现在某些位置已经确定了，也就是确定了某些人的排列和某些场次的先后，那么减去即可．

## [735E Ostap and Tree](https://codeforces.com/contest/735/problem/E)

设 $f_{u, i}$ 表示 $u$ 子树中，存在一个黑点与 $u$ 之间的距离为 $i$，且距离大于 $i$ 的点的限制均被满足的方案数．

考虑合并 $v$ 这棵子树，枚举 $i, j$，转移如下：

1. 若 $i + j \le 2k$，那么两条路径直接拼接不会导致有点不被满足，那么直接 $f_{u, i} \times f_{v, j} \to f_{u, \min\{i, j + 1\}}$．
2. 若 $i + j > 2k$，不妨设 $u$、$v$ 子树中达到 $i$、$j$ 限制的点分别为 $s$、$t$，那么 $s \to t$ 这条链上一定存在某些节点不被满足，此时有 $f_{u, i} \times f_{v, j} \to f_{u, \max\{i, j + 1\}}$．

答案即为 $\sum\limits_{i \le k} f_{1, i}$．

## [1348F Phoenix and Memory](https://codeforces.com/contest/1348/problem/F)

首先考虑求出一个排列．若存在不止一个满足条件的排列，那么其他排列一定可以通过交换求出的排列中的某两项来求出．

贪个心先，将线段按照左端点升序排序，考虑按照升序确定 $1 \sim n$ 这些值所填的位置，每次取出能够覆盖到 $i$ 的，右端点最小的区间，将 $i$ 放到对应的位置上．由于题目保证有解，这样的构造显然合法．

然后就是确定能否交换两个位置了．设位置 $i$ 和 $j$ 可以交换，那么有：$a_i \le p_j \le b_i, a_j \le p_i \le b_j$，考虑按照 $p_i$ 升序添加元素干掉第一个限制，然后第二个限制相当于看有没有区间覆盖到 $p_i$，拿一棵线段树维护区间覆盖单点查询即可．

## [1832E Combinatorics Problem](https://codeforces.com/contest/1832/problem/E)

扰动一下：

$$
\begin{aligned}
  b_{i, k}
  &= \sum_{j = 1}^i \binom{i - j + 1}{k} a_j \\
  &= \sum_{j = 1}^i \left(\binom{i - j}{k} + \binom{i - j}{k - 1}\right) a_j \\
  &= \binom{0}{k} a_i + \binom{0}{k - 1} a_i + \sum_{j = 1}^{i - 1} \binom{i - j + 1}{k} a_j + \sum_{j = 1}^{i - 1} \binom{i - j + 1}{k - 1} a_j \\
  &= b_{i - 1, k} + b_{i - 1, k - 1} + [k - 1 = 0] a_i
\end{aligned}
$$

递推即可．

## [1830B The BOSS Can Count Pairs](https://codeforces.com/contest/1830/problem/B)

考虑 $a_i a_j = b_i + b_j \le 2n$，那么考虑根号分治．

枚举 $k \le \sqrt{2n}$，不妨设 $a_i = k$，那么有 $k a_j = b_i + b_j \iff k a_j - b_j = b_i$，拿个桶计数就行了．$a_j = k$ 时由于钦定 $i < j$，需要单独拿出来扫一遍．

## [1515E Phoenix and Computers](https://codeforces.com/contest/1515/problem/E)

设 $f_{i, j}$ 表示已经打开了 $i$ 台电脑，已经打开的电脑构成了 $j$ 个连续段的方案数．

考虑对连续段操作：

1. 延长连续段．可以选择在某个连续段两边插入，或间隔一个插入，第二种操作会导致中间的那个未被打开的电脑被打开，有 $2j \times f_{i, j} \to f_{i + 1, j}$，$2j \times f_{i, j} \to f_{i + 2, j}$，

2. 合并连续段．间隔两个格子的连续段可以通过任意打开一台电脑来合并，有 $2(j - 1) \times f_{i, j} \to f_{i + 2, j - 1}$；间隔三个格子的连续段可以通过打开中间的一台电脑合并，有 $(j - 1) \times f_{i, j} \to {i + 3, j - 1}$．

3. 新建连续段．$(j + 1) \times f_{i, j} \to f_{i + 1, j + 1}$．

## [1228E Another Filling the Grid](https://codeforces.com/contest/1228/problem/E)

设 $g_{i, j}$ 表示恰有 $i$ 行 $j$ 列最小值为 $1$ 的方案，$f_{i, j}$ 表示钦定 $i$ 行 $j$ 列，使得剩下的行列最小值不为 $1$ 的方案数，有

$$
f_{n, n} = \sum_{i = 0}^n \sum_{j = 0}^n \binom{n}{i} \binom{n}{j} g_{i, j} \iff g_{n, n} = \sum_{i = 0}^n \sum_{j = 0}^n \binom{n}{i} \binom{n}{j} (-1)^{n - i + n - j} f_{i, j}
$$

其中 $f_{i, j}$ 显然为 $(k - 1)^{n^2 - (n - i)(n - j)} \times k^{(n - i)(n - j)}$．

可以直接计算．

## [1120C Compress String](https://codeforces.com/contest/1120/problem/C)

设 $f_i$ 表示考虑了前 $i$ 个字符的最小代价，有转移

$$
\begin{aligned}
  f_i &\leftarrow f_{i - 1} + a \\
  f_i &\leftarrow f_{i - k} + b
\end{aligned}
$$

其中 $k$ 满足 $s[i - k + 1 : i]$ 是 $s[: i - k]$ 的子串．

这样决策点很难找，不妨直接枚举子串的结束位置 $j$，设 $\mathrm{lcs}_{i, j}$ 表示前缀 $s[: i]$ 和 $s[: j]$ 的最长公共后缀，有转移 $f_i \leftarrow f_{\max\{i - \mathrm{lcs}_{i, j}, j\}}$．这样就能做到 $\Theta(n)$ 转移了．

## [1499F Diameter Cuts](https://codeforces.com/contest/1499/problem/F)

设 $f_{u, i}$ 表示 $u$ 的子树内，以 $u$ 开始的链最长的长度为 $i$ 的方案数．

转移考虑合并一棵子树 $v$，枚举 $u, v$ 中最长链的长度 $i, j$，有

1. $i + j \le k$ 时，此时可以选择不断开边 $(u, v)$，也可选择断开 $(u, v)$，有转移 $f_{u, \max\{i, j + 1\}} \leftarrow 2 \times f_{u, i} f_{v, j}$．
2. $i + j > k$ 时，此时只能选择断开 $(u, v)$，有转移 $f_{u, \max\{i, j + 1\}} \leftarrow f_{u, i} f_{v, j}$．

看似是 $\Theta(nk^2)$ 的，但事实上若枚举上界定为子树深度，那么可以分析出时间复杂度为 $O(nk)$．分析参考 [这篇博客](https://blog.csdn.net/lyd_7_29/article/details/79854245)．

## [1819B The Butcher](https://codeforces.com/contest/1819/problem/B)

首先可以找到最大的长和宽，这可能是原长方形的或宽．

然后可以算面积和，就可以初步判定长宽的合法性．

考虑如何确定某个长宽是合法的．考虑模拟，每次选出最长的长或宽尝试去剪，若某次选不出来就寄了．可以用两个 `multiset` 维护．

## [1817B Fish Graph](https://codeforces.com/contest/1817/problem/B)

将环上外挂两个节点的节点称作关键节点，显然关键节点的度数 $\ge 4$．

考虑枚举关键节点 $u$，我们只需要搜出一个过 $u$ 的最小简单环就能直接构造．

如何搜最小简单环？考虑以 $u$ 为根的 bfs 树，若 bfs 过程中访问到了一条横叉边 $(x, y)$，满足 $x$ 和 $y$ 在 $u$ 的不同子树内，那么 $u \to x \to y \to u$ 显然是一个合法的简单环，而 bfs 过程中访问到的第一个这样的横叉边构成的环显然是最小的．记录 bfs 树上的父边就能还原方案．

## [1811G Vlad and the Nice Paths](https://codeforces.com/contest/1811/problem/G)

先考虑如何求出最长长度．

设 $f_i$ 表示以 $i$ 结束的好路径的最长长度是多少，转移枚举之前的位置 $j$，其中 $j$ 满足区间 $[j, i]$ 中 $a_i$ 出现的次数 $\ge k$．

考虑如何计数，设区间 $[j, i]$ 中 $a_i$ 的出现次数为 $\mathrm{ct}$，由于钦定选 $a_i$，那么次数的贡献系数就是 $\binom{\mathrm{ct} - 1}{k - 1}$．

## [1870D Prefix Purchase](https://codeforces.com/contest/1870/problem/D)

考虑选择序列中价格最小的位置 $i$，然后买 $\lfloor k / c_i \rfloor$ 次，这样显然给出了操作完后 $a_1$ 可能的最大值．

接下来考虑在不动 $a_1$ 的情况下调整使得字典序最大．设上次调整完后剩余的钱数为 $r$，上次买的位置是 $p$，我们找到 $[p + 1, n]$ 中的最小值 $c_q$，然后卖掉 $\lfloor r / (c_q - c_p) \rfloor$ 个 $p$ 后加钱买个数相等的 $q$，这样显然不会改变 $a_1$ 的值，由于 $q$ 在 $p$ 后边，所以会增大字典序．由于每次选的是后缀最小值，所以调整过程一定能结束．

此时已经可以利用优先队列做到 $O(n \log n)$，但是注意到每次选的是最靠后的后缀最小值，可以直接预处理做到 $O(n)$．

## [1864F Exotic Queries](https://codeforces.com/contest/1870/problem/D)

先考虑没有区间询问怎么做．

若我们每次都能将等于某个值的所有数归 $0$，那么答案就是值域区间中值的个数，容易证明这是答案的下界．

考虑将我们未考虑到的操作次数加上去．考虑一个值在序列中所有的出现位置，若相邻位置之间的最大值小于当前值，那么当前值需要的归零次数需要增加一次．统计答案就是统计这样的点对数目．

考虑带上区间询问怎么做．考虑将询问挂右端点上扫描线，需要统计的点对 $(l, r)$ 需要满足

$$
l \le \max_{i = l + 1}^{r - 1} a_i < a_l = a_r
$$

由于总点对数目是 $O(n)$ 的，弄个树状数组维护单点加后缀和，然后暴力加点即可．

当然还需要一棵线段树维护单点赋值区间 $\max$．

## [1863F Divide, XOR, and Conquer](https://codeforces.com/contest/1863/problem/F)

考察一个区间 $[l, r]$ 在何时可以被划分出并保留 $[l, k]$．

记 $s_{l, r} = \bigoplus\limits_{i = l}^r a_i$．对于一个划分点 $k$，记 $x = s_{l, k}, y = s_{k + 1, r}$．

1. 若 $s_{l, r} = 0$，那么对于任意的划分点 $k$，一定有 $x = y$，也就是说可以随便划分．
2. 若 $s_{l, r} \not= 0$ 时，考虑 $s_{l, r}$ 的最高位 $p$，对于一个划分点 $k$，$x$ 和 $y$ 的第 $p$ 位一定不同，且高于 $p$ 的位数一定相同．也就是说，第 $p$ 位决定了 $x$ 和 $y$ 的大小关系．

那么我们按照区间长度降序遍历所有区间，记 $\mathrm{st}_i$ 表示考察 $i$ 开始的合法区间，每个合法区间的异或和的最高位在 $\mathrm{st}_i$ 中的对应位都是 $1$．也就是说，对于 $\mathrm{st}_i$ 中的每一位 $p$，若该位上为 $1$，那么存在一个合法区间 $[i, x]$，使得 $s_{i, x}$ 的最高位为 $p$．

当我们枚举一个区间 $[l, k]$ 时，我们只需判断 $s_{l, k} \operatorname{and} \mathrm{st}_l$ 是否有值，若有值则 $[l, k]$ 可以被某个区间划分出来．

保留右半边的情况类似．

## [1863E Speedrun](https://codeforces.com/contest/1863/problem/E)

容易发现，若我们第一天的开始时间已经确定了，那么整个流程都是确定的，我们只需要决策第一天有多少活动需要推迟一天完成即可．

尝试设 $f_{u, 0 / 1}$ 表示 $u$ 这个活动不推迟 / 推迟一天完成时，依赖 $u$ 的事件的最晚完成时间．为了完成后继事件 $v$ 到 $f_{u, 1}$ 的转移，我们需要判定 $u$ 推迟一天后是否会影响到 $v$ 的完成．如果影响了，则需要从 $f_{v, 1}$ 转移．

麻烦之处在于，$u \to v$ 这条边造成的时间限制不一定是紧的，也就是说 $v$ 可能需要在 $u$ 完成后推迟很久才能被完成，此时尝试推迟 $u$ 的完成不会对 $v$ 的完成造成影响．

我们可以先尝试钦定顺序做这些活动，给每个节点先钦定一个完成日期，若 $u$ 的前驱 $p$ 的完成时刻晚于 $u$，那么 $u$ 需要推迟一天．然后这样就可以快速计算推迟造成的影响了，而由于我们按照逆拓扑序进行 DP，所以推迟不会对后面递推到的点造成影响，无后效性．

## [1854C Expected Destruction](https://codeforces.com/contest/1854/problem/C)

若集合是可重的，那么答案就是 $\sum_i m + 1 - a_i$．考虑去掉由于插入重复数字导致集合元素减少对期望造成的贡献．

发现只有原序列中相邻的元素才可能造成这样的贡献，于是考虑枚举这样的元素对，除去贡献．

考虑 $(a_i, a_{i + 1})$ 的时候，直接设 $f_{x, y}$ 表示 $a_i$ 加了 $x$ 次，$a_j$ 加了 $y$ 次，转移显然．当 $x$ 和 $y$ 的差值等于 $a_{i + 1} - a_i$ 时，就直接贡献到答案上．

## [1822G Magic Triples](https://codeforces.com/contest/1822/problem/G)

枚举 $j$，再枚举 $d | a_j$，$a_i$ 和 $a_k$ 分别能取 $a_j / d, a_j \times d$，拿个桶统计即可．

然而这样做的时间复杂度是 $O(n \sqrt{V})$ 的，直接做就寄了．由于有 $a_i \le {10}^9$，那么 $a_j \times d \le {10}^9$．

考虑根号分治，设置阈值 $B$．

1. 当 $a_i \le B$ 的时候直接做，这部分复杂度是 $O(nB^{1 / 2})$．
2. 当 $a_i > B$ 时，有用的因数不超过 $V / B$ 个，直接枚举因数，这部分复杂度是 $O(nV / B)$．

显然 $B$ 取 $V^{2 / 3}$ 时达到平衡，复杂度 $O(nV^{1 / 3})$，可以通过．

## [1707C DFS Trees](https://codeforces.com/contest/1707/problem/C)

由于边权互不相同，原图的 MST 确定，直接弄出来．

题目中给的那个伪代码就是会找到原图的一个 dfs 树，想到 dfs 树合法的条件就是不存在横叉边，若当 MST 的根确定为 $i$ 时，存在作为横叉边的非树边，那么 $i$ 一定不合法．

这个判定条件是充要的，由于我们找到的是 MST，所以任意一条非树边 $(u, v)$ 加入后构成的环中，$(u, v)$ 的边权一定是最大的，那么无论走到 $u$ 或 $v$ 中的哪一个点，都不会先走 $(u, v)$．

首先随便钦定一个根，然后依次考虑所有非树边 $(u, v)$．

- 若 $u$ 是 $v$ 的祖先或 $v$ 是 $u$ 的祖先，那么这条链上除开 $u$ 和 $v$ 的点都要被日掉．
- 不然，除了 $u$ 和 $v$ 子树内的点都要被日掉．

树上差分打标记即可．

## [1375G Tree Modification](https://codeforces.com/contest/1375/problem/G)

由于树是一个二分图，考虑将这个二分图的两个部分染成黑白两色．如果我们对 $a, b, c$ 进行一次操作，不妨假设操作前 $a, c$ 是白色，$b$ 是黑色，那么由于操作后 $a$ 连到了 $c$ 上，其颜色改变，其他节点颜色不变．

考虑对菊花图染色，发现其中有一种颜色只有一个节点，设原树上白色节点数为 $w$，黑色节点数目为 $b$，答案即为 $\min\{w, b\} - 1$．

~~以后遇到树上诡异操作可以尝试染色后观察性质．~~

## [1870E Another MEX Problem](https://codeforces.com/contest/1870/problem/E)

容易设计一个 2D / 1D 的动态规划：设 $f_{i, j}$ 为考虑前 $i$ 个元素，划分出的段的 mex 的异或和为 $j$ 的方案是否存在．这个 DP 从状态到转移都难以优化，于是我们考察有用的转移的数量．

容易发现，对于以当前位置为右端点且 mex 相同的所有区间，只有最短的那个是有用的，因为可以不选，转移区间变小必定不劣．

然后感受一下这个数量级，发现总数是 $O(n)$ 的．证明见 [官方题解](https://codeforces.com/blog/entry/120524)．