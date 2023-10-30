---
comments: true
---

# ABC326G Unlock Achievement 题解

需要注意，以下我们将题目中提到的变量全部转为小写。

容易发现，若我们假定所有成就都会达成，那么我们需要考虑的就是一个最小化投入的问题。再观察数据范围，$n$ 和 $m$ 都是极小的 $50$ 级别，合理地猜测使用最小割解决。

???+ question "为啥想到用最小割？"
    下面会讲到把每个技能拆成五个节点，对应五个等级，容易发现这些 **技能等级** 和 **成就** 都只有达成 or 未达成两种情况，那么根据割的定义，割可以把图分成与源点相连的部分和与汇点相连的部分，自然达成未达成的情况就决策出来了。

首先，建立超级源点 $s$ 和超级汇点 $t$，之后，定义顶点 $A(x, y)$ 表示 $x$ 技能的第 $y$ 级（$x\in [1, n],\ y\in [1, 5]$），定义顶点 $B(z)$ 表示第 $z$ 项成就（$z\in [1, m]$）。

开始连边，每个技能初始都是 $1$ 级，我们连接边 $s\to A(i, 1)$，容量为 $+\infty$。

之后，每个技能升级需要 $c_i$ 块钱，我们对于 $j\in [1,4]$，连接边 $A(i, j)\to A(i, j+1)$，容量设为 $(j - 1)\times c_i$。

这里技能内不可能出现 **下一级达成但上一级未达成** 的情况（即等级之间有依赖关系），所以要连接一条反边容量为 $+\infty$。

之后，连接边 $A(i, 5)\to t$，容量设为 $4\times c_i$，表示这个技能完成了升级。

之后思考，如果我达成了某项成就，我就会获得一些钱，那么如何把他转化成 **投入** 呢？注意到前面提到 **假定所有成就都会达成**，于是我的决策就变成了 **选择哪项成就不达成**，**最小化不达成所带来额外的代价**，所以连边 $s\to B(i)$，容量为 $a_i$。

这里需要注意；达成 $\forall j\in [1, n],\ A(j, l_{i, j})$ 是达成 $B(i)$ 的充要条件。为了避免技能升级升够了成就还没达成的情况，由当前成就向这 $n$ 个技能等级连边，容量为 $+\infty$。

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
            mf.add_edge(5 * i + j + 1, 5 * i + j, Inf);
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