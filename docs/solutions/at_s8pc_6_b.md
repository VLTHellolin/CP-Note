---
comments: true
---

# AT_s8pc_6_b AtCoder Market 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_s8pc_6_b)

[题面(AtCoder)](https://atcoder.jp/contests/s8pc_6/tasks/s8pc_6_b)

AtCoder Problems 评级难度：<span style="color: #bfbfbf">None</span>。

## 题意

- 有一条数轴。
- 现在有 $N$ 个动点，这 $N$ 个动点初始都在 $x$ 位置，同时开始运动，每个动点运动一次会向左或向右移动一个单位，所耗时间是 $1$ 秒，第 $i$ 个动点的运动路径必须是 $x\to A_i\to B_i\to y$，到达终点就不必运动了。
- 现在，由你指定 $x$ 和 $y$，问在所有点都走最短路径的情况下，最短的总耗费时间是多少。

## 思路

要使时间最短，不难想到，最划算的是将 $x$ 和 $y$ 设为某个动点要经过的 $A$ 或 $B$。

之后计算取最小值就行了（数轴上 $a$ 与 $b$ 两点的距离是 $|a-b|$）。

## 代码

``` cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
ll n, a[50], b[50];
vector<ll>c;
ll now, ans;
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n;
    ans=LONG_LONG_MAX-1;
    for(ll i=1; i<=n; i++)
    {
        cin>>a[i]>>b[i];
        c.push_back(a[i]);
        c.push_back(b[i]);
    }
    for(ll x:c)
    {
        for(ll y:c)
        {
            now=0;
            for(ll i=1; i<=n; i++)
                now+=abs(x-a[i])+abs(a[i]-b[i])+abs(b[i]-y);
            ans=min(ans,now);
        }
    }
    cout<<ans<<endl;
    return 0;
}
```