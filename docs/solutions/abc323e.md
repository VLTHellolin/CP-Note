---
comments: true
---

# ABC323E Playlist 题解

!!! abstract
    如果第一首歌在 $(X+0.5)$ 这个时间播放，那么他可能的开始时间 $s$ 满足 $s\in [X-t_1+1, X] \cap \N$。所以说，我们计算的答案就是在这个时间区间内，第一首歌开始播放的概率之和。

然后我们考虑 dp，设 $f_i$ 是在 $i$ 时间开始播放某首歌的概率，$f_0=0$。

转移比较显然了，若当前状态为 $f_i$（也就是某首歌从 $i$ 这个时间刚开始播放），设这首歌时长为 $l$，那么这首歌将会在 $l+x$ 这个时间结束，并立即随机播放另一首歌（每一首歌播放的概率是 $\frac{1}{n}$），伪代码如下：

$$
\begin{array}{ll}
1 & \textbf{Procedure } \text{Transfer.} \\
2 & \qquad f_0\gets 1 \\
3 & \qquad \textbf{For } i\coloneqq 0\textbf{ To } X \\
4 & \qquad\qquad \textbf{For } j\coloneqq 1\textbf{ To } n \\
5 & \qquad\qquad\qquad f_{i+t_j}\gets f_{i+t_j} + f_i\times \frac{1}{n}
\end{array}
$$

!!! tip

    这里注意也可能有其他状态转移到 $f_{i+t_j}$，所以我们要累加。

答案就是我们第一段说的 $\sum\limits_{i=X-t_1+1}^{X}f_i$。

!!! tip

    注意要特判掉 $X < t_1$ 的情况（显然这种情况下答案是 $\frac{1}{n}$）。

``` cpp
constexpr int N = 5e4 + 114;
using mint = atcoder::modint998244353;
int n, x, t[N];
mint p, ans, dp[N];
void solve() {
    std::cin >> n >> x;
    p = (mint) 1 / (mint) n;
    rep(i, n) std::cin >> t[i];
    if (t[1] > x) {
        std::cout << p.val() << '\n';
        return;
    }

    dp[0] = 1;
    rep(i, 0, x) {
        rep(j, n)
            dp[t[j] + i] += p * dp[i];
    }
    rep(i, x - t[1] + 1, x)
        ans += dp[i];
    ans *= p;
    std::cout << ans.val() << '\n';
}
```