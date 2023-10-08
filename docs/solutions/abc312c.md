---
comments: true
---

# ABC312C Invisible Hand 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc312_c) | [AtCoder 题面](https://atcoder.jp/contests/abc312/tasks/abc312_c)

手玩一下样例或者打个暴力就很容易发现，答案是这个序列：$A_1, A_2, \dots, A_n, B_1+1, B_2+1,\dots, B_m+1$ 排序后的第 $m$ 项（下面记这个排序后的序列为 $C$）。

下面来证明一下这个答案是正确的。

首先，如果我要以 $0$ 日元定价，我们发现卖的都不愿意卖，买的都愿意买，如果我要以 $10^9+1$ 日元定价，那么卖的都愿意卖，买的都不愿意买。

我们不妨记以 $x$ 日元定价时卖家数与买家数的差值是 $D_x$，这个问题就转化成了求满足 $D_x\ge 0$ 的最小 $x$。显然的，$D_0 = -m$。

当我们令 $x$ 为 $C$ 序列中的值时，我们发现，如果 $C$ 序列中这个值是 $B$ 序列中某个值加 $1$ 的结果，那么此时买家就会少一个（因为此时定价刚好比这个买家给的价格高 $1$ 日元）；如果这个值是 $A$ 序列中某个值，那么此时卖家就会多一个。显然的，$x$ 的增加只会让买家减少或卖家增多。因此，我们可以推断 $D_{C_1} = -m+1,\ D_{C_2} = -m+2,\ \dots,\ D_{C_m} = -m+m = 0$。

```cpp
// * HELLOLIN'S SOL CODE TEMPLATE v1.0.1
#include <bits/extc++.h>

namespace hellolin {
namespace lib {
using ll = long long;
using ull = unsigned long long;
using lll = __int128;
using ulll = __uint128_t;
using ld = long double;
template <class T, class U> inline bool chmax(T &x, U y) { return y > x ? (x = y, 1) : 0; }
template <class T, class U> inline bool chmin(T &x, U y) { return y < x ? (x = y, 1) : 0; }
#define pb emplace_back
}
using namespace lib;

// * CORE CODE BEGIN * //
constexpr static ll N = 4e5 + 514;
int n, m;
ll a[N];
void solve() {
    std::cin >> n >> m;
    for(int i=1; i<=n; i++) std::cin >> a[i];
    for(int i=n+1; i<=n+m; i++) {
        std::cin >> a[i];
        ++a[i];
    }
    std::sort(a+1, a+1+n+m);
    std::cout << a[m] << '\n';
}
// * CORE CODE END * //

} // namespace hellolin

int main() {
    // freopen(".in", "r", stdin), freopen(".out", "w", stdout);
    std::ios::sync_with_stdio(false), std::cin.tie(nullptr), std::cout.tie(nullptr);
    hellolin::solve();
    return 0;
}
```