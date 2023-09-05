---
comments: true
---

# ABC309C Medicine 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc309_c) | [AtCoder 题面](https://atcoder.jp/contests/abc309/tasks/abc309_c)

首先我们对这些吃药信息排序，以 $a_i$（吃药持续的天数）为关键字升序排序，这样我们这些吃药信息就是按结束时间排序的了。

`pair` 大法好！

``` cpp
// pair<ll,ll> a[N];
cin >> n >> k;
for(ll i=1; i<=n; i++) {
    cin >> a[i].first >> a[i].second;
    now += a[i].second;
}
sort(a+1, a+1+n);
```

获取到第一天要吃的药总数（即代码中的 `now`），之后，遍历这些信息，总数减去 $b_i$，一旦总数小于等于 $k$，输出答案。

输出答案时需要注意当前这天还是在吃药的，所以要输出 $a_i + 1$。

时间复杂度 $O(N\log N)$。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll n,k,now;
pair<ll,ll> a[300010];
void solve() {
    cin >> n >> k;
    for(ll i=1; i<=n; i++) {
        cin >> a[i].first >> a[i].second;
        now += a[i].second;
    }
    sort(a+1, a+1+n);
    if(now <= k) {
        cout << 1 << endl;
        return;
    }
    for(ll i=1; i<=n; i++) {
        now -= a[i].second;
        if(now <= k) {
            cout << a[i].first + 1 << endl;
            return;
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```