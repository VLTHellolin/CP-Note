---
comments: true
---

# ABC309D Add One Edge 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc309_d) | [AtCoder 题面](https://atcoder.jp/contests/abc309/tasks/abc309_d)

在第一部分（点 $1$ 到点 $N_1$）和第二部分（点 $N_1+1$ 到 $N_1+N_2$）各跑一遍 dijkstra，找到离 $1$ 最远的点和离 $N_1+N_2$ 最远的点，两个点一连，这样构成的 $1$ 到 $N_1+N_2$ 的路径是最长的。

``` cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/priority_queue.hpp>
using namespace std;
using namespace __gnu_pbds;
using ll = long long;

constexpr static ll N = 3e5+114, INF = 1145141919;

ll n1, n2, m, a, b;

vector<ll> g1[N], g2[N];
__gnu_pbds::priority_queue<pair<ll,ll>, greater<pair<ll,ll>>, pairing_heap_tag> q;
ll dis[N];
bool vis[N];

ll ans1, ans2;

void dij(ll l, ll s) {
    dis[s]=0;
    q.push(make_pair(0,s));
    while(q.size()) {
        ll p = q.top().second;
        q.pop();
        if(vis[p]) continue;
        vis[p] = 1;

        if(l==1) {
            for(auto i:g1[p]) {
                ll v=i, w=1;
                if(dis[v]>dis[p]+w) {
                    dis[v]=dis[p]+w;
                    q.push(make_pair(dis[v], v));
                }
            }
        } else {
            for(auto i:g2[p]) {
                ll v=i, w=1;
                if(dis[v]>dis[p]+w) {
                    dis[v]=dis[p]+w;
                    q.push(make_pair(dis[v], v));
                }
            }
        }
    }
}

void solve() {
    cin >> n1 >> n2 >> m;
    for(ll i=1; i<=m; i++) {
        cin >> a >> b;
        if(a>n1||b>n1) { // 在第二部分
            g2[a].push_back(b);
            g2[b].push_back(a);
        } else { // 在第一部分
            g1[a].push_back(b);
            g1[b].push_back(a);
        }
    }

    fill(dis+1, dis+n1+n2+1, INF);
    dij(1, 1), dij(2, n1+n2);

    for(ll i=1; i<=n1; i++) {
        ans1 = max(ans1, dis[i]);
    }
    for(ll i=n1+1; i<=n1+n2; i++) {
        ans2 = max(ans2, dis[i]);
    }
    cout << ans1 + ans2 + 1 << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```