---
comments: true
---

# ABC326G Unlock Achievement 题解

需要注意，以下我们将题目中提到的变量全部转为小写。

容易发现，若我们假定所有成就都会达成，那么我们需要考虑的是 **哪些技能等级 / 成就达成** 和 **哪些技能等级 / 成就不达成**，就是一个最小化投入的问题。再观察数据范围，$n$ 和 $m$ 都是极小的 $50$ 级别，合理地猜测使用最小割解决。

首先，建立超级源点 $s$ 和超级汇点 $t$，之后，定义顶点 $A(x, y)$ 表示 $x$ 技能的第 $y$ 级（$x\in [1, n],\ y\in [1, 5]$），定义顶点 $B(z)$ 表示第 $z$ 项成就（$z\in [1, m]$）。

开始连边，每个技能初始都是 $1$ 级，我们连接边 $s\to A(i, 1)$，容量为 $+\infty$。

之后，每个技能升级需要 $c_i$ 块钱，我们对于 $j\in [1,4]$，连接边 $A(i, j)\to A(i, j+1)$，由于后面要弄最小割，容量需要设为 $(j - 1)\times c_i$。

这里技能内不可能出现 **下一级达成但上一级未达成** 的情况，所以要连接一条反边容量为 $+\infty$（代码实现中就不用连了，流不回去的）。

之后，连接边 $A(i, 5)\to t$，容量设为 $4\times c_i$，表示这个技能完成了升级。

之后思考，如果我达成了某项成就，我就会获得一些钱，那么如何把他转化成 **投入** 呢？注意到前面我们提到 **假定所有成就都会达成**，于是，连边 $s\to B(i)$，容量为 $a_i$，表示如果这个成就没有达成，那么可以视为额外多了 $a_i$ 的投入。

这里需要注意；达成 $\forall j\in [1, n],\ A(j, l_{i, j})$ 是达成 $B(i)$ 的充要条件。所以将这 $n$ 个技能等级和当前这个成就连边（因为我们前面刚刚连接了超级源点向成就的边，成就还没有和 $t$ 连通，所以边的方向为 **成就连等级**），容量为 $+\infty$。

最后答案即为 $\sum a_i - \operatorname{maxflow}(s, t)$。

至此我们成功做完了这一题。注意需要开 `long long`，以及使用 ACL 能省不少事。

``` cpp
#include <bits/stdc++.h>
#include "atcoder/all"
constexpr int N = 60;
constexpr long long Inf = 1e12;
int n, m, s, t;
long long c[N], a[N], l[N][N];
int main() {
    std::cin.tie(nullptr)->sync_with_stdio(false);
    std::cin >> n >> m;
    for (int i = 1; i <= n; ++i)
        std::cin >> c[i];
    for (int i = 1; i <= m; ++i)
        std::cin >> a[i];
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
            std::cin >> l[i][j];
    atcoder::mf_graph<long long> mf(5 * (n + 1) + m + 6);
    s = 0, t = 5 * (n + 1) + m + 1;
    for (int i = 1; i <= n; ++i) {
        mf.add_edge(s, 5 * i + 1, Inf);
        for (int j = 1; j <= 4; ++j) {
            mf.add_edge(5 * i + j, 5 * i + j + 1, c[i] * (j - 1));
        }
        mf.add_edge(5 * i + 5, t, c[i] * 4);
    }
    for (int i = 1; i <= m; ++i) {
        mf.add_edge(s, 5 * (n + 1) + i, a[i]);
        for (int j = 1; j <= n; ++j) {
            mf.add_edge(5 * (n + 1) + i, 5 * j + l[i][j], Inf);
        }
    }
    std::cout << std::accumulate(&a[1], &a[m + 1], 0ll) - mf.flow(s, t) << '\n';
}
```