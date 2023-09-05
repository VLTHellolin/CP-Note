---
comments: true
---

# ABC306D Poisonous Full-Course 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc306_d) | [AtCoder 题面](https://atcoder.jp/contests/abc306/tasks/abc306_d)

注意到题目中说只能选择吃不吃，而不是选择顺序，所以可以用 dp 来解决。

很经典的 01 背包。

二维 dp，第一维是菜品编号，第二维是 $0$ 或 $1$ 表示健康或中毒状态。

转移（$y$ 表示当前菜品美味值）：

**若当前菜品解毒**

选择吃菜，那么可以从有毒的状态转移到无毒，也可以从无毒的状态转移到无毒。

选择不吃，那么可以从有毒的状态转移到有毒，也可以从无毒的状态转移到无毒。

$$f_{i, 0} = \max(f_{i-1, 0}+y,\ f_{i-1, 0},\ f_{i-1, 1}+y)$$
$$f_{i, 1} = f_{i-1, 1}$$

**若当前菜品有毒**

选择吃菜，那么可以从无毒的状态转移到有毒。

选择不吃，那么可以从有毒的状态转移到有毒，也可以从无毒的状态转移到无毒。

$$f_{i, 0} = f_{i-1, 0}$$
$$f_{i, 1} = \max(f_{i-1, 0}+y,\ f_{i-1, 1})$$

``` cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

constexpr static int N=3e5+11;
int n;
bool x;
int y;
int dp[N][2];
void solve(){
    cin >> n;
    for(int i=1; i<=n; i++){
        cin >> x >> y;
        if(!x) {
            dp[i][0]=max({dp[i-1][0]+y,dp[i-1][0],dp[i-1][1]+y});
            dp[i][1]=dp[i-1][1];
        } else {
            dp[i][0]=dp[i-1][0];
            dp[i][1]=max({dp[i-1][0]+y,dp[i-1][1]});
        }
    }
    cout << max(dp[n][0], dp[n][1]) << endl;
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```