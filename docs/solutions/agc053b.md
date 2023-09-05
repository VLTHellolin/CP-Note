---
comments: true
---

# AGC053B Taking the middle 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_agc053_b)

[题面(AtCoder)](https://atcoder.jp/contests/agc053/tasks/agc053_b)

AtCoder Problems 评级难度：<span style="color: #337dff">1755</span>。

## 思路

不难想到，当现在是第 $i(1\le i\le N)$ 回合时，Aoki 能拿的所有牌编号范围是 $[n-i+1,\ n+i]$。

那么我们有一个想法：Takahashi 把较大的牌拿走，让 Aoki 把较小的牌拿走。

可以用优先队列来实现，大数在队尾小数在队首，让 Aoki 每次把队首的牌拿走，最后队列里剩余的就是 Takahashi 拿牌的最优方案。

时间复杂度为循环乘优先队列操作，不考虑读入为 $O(N\log N)$。

## 代码

``` cpp
#include <bits/stdc++.h>
// #include <atcoder/all>
using namespace std;
#define int long long
const static int N=2e5+1145;
int n,v[N<<1],ans;
priority_queue<int,vector<int>,greater<int>>q;
inline void solve()
{
    cin>>n;
    for(int i=1;i<=(n<<1);i++)cin>>v[i];
    for(int i=1;i<=n;i++)
    {
        q.push(v[n-i+1]);
        q.push(v[n+i]);
        q.pop();
    }
    while(q.size())
    {
        ans+=q.top();
        q.pop();
    }
    cout<<ans<<endl;
}
#undef int
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    solve();
    return 0;
}
```

