---
comments: true
---

# ABC302E Isolation 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc302_e) | [AtCoder 题面](https://atcoder.jp/contests/abc302/tasks/abc302_e) | [AC 记录](https://www.luogu.com.cn/record/111080126) | $\texttt{\color{73f273}*988}$

考虑开一个 `set` 来存图。剩下的就交给暴力了！（代码中有注释）

但是每次请求结束时还要输出 $\mathrm{ans}$，实际上此时不用浪费时间从头跑了，请求内部就可以计算答案，动态维护这个 $\mathrm{ans}$，复杂度 $O(Q\log N)$，可以通过本题。

完整代码如下：

``` cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

int n,q,x,u,v,ans;
vector<set<int>>a;
void solve(){
    cin>>n>>q;
    a.resize(n+1);
    ans=n; // 一开始所有点都是孤立的
    for(int i=1;i<=q;i++){
        cin>>x>>u;
        if(x==1){
            cin>>v;
            // 如果这个点之前就是孤立点，那么现在有边相连了，需要将 ans 减一
            ans-=a[u].empty();
            ans-=a[v].empty();
            // 加边
            a[u].insert(v);
            a[v].insert(u);
        }
        else{
            ans+=!a[u].empty(); // 如果这个点之前不是孤立的而现在是孤立的，需要 ans 加一
            for(int x:a[u]){ // 要删除所有与当前点相连的边，此处枚举每个与当前点相连的点并分别 erase 掉与当前点相连的边
                a[x].erase(u);
                ans+=a[x].empty(); // 如果删了之后有点被孤立了 ans 就加一
            }
            a[u].clear(); // 删边
        }

        cout<<ans<<endl;
    }
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```
