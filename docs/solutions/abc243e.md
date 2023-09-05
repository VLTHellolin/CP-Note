---
comments: true
---

# ABC243E Edge Deletion 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_abc243_e)

[题面(AtCoder)](https://atcoder.jp/contests/abc243/tasks/abc243_e)

AtCoder Problems 评级难度：<span style="color: #337dff">1637</span>。

**UPD1**: 补充了结论的说明，使代码更易读。

## 题意

- 给你一张无重边无自环的联通带权无向图。
- 定义 $d(i,j)$ 是 $i$ 到 $j$ 的最短路径上的边权之和。
- 你需要删除一些边。要求删完之后的图满足下列条件：
  - 图仍然联通；
  - 对于 $1\le i,j\le N$，删边前的 $d(i,j)$ **等于**删边后的 $d(i,j)$。
- 现在问你最多能删多少条边。
- 数据保证：
  - $2\le N\le 300,\ N-1\le M\le \frac{N(N-1)}{2}$。
  - $1\le u< v\le N,\ 1\le w\le 10^9$。
  - 图是联通的，没有重边和自环。

## 思路

思路很简洁，先跑一遍最短路，$N\le 300$，所以用 Floyd 就行了。

对于每一个点对 $(i,j)$ 满足 $1\le i,j\le N,\ i\not = j$，如果能够找到一个新的点 $k$ 满足 $1\le k\le N,\ i\not = k,\ j\not =k$ 且 $d(i,j)=d(i,k)+d(k,j)$ 说明这一条边是可以删去的。

注意无向图记录的 $\mathrm{ans}$ 要除以二。

## 代码

``` cpp
#include <bits/stdc++.h>
// #include <atcoder/all>
using namespace std;
#define int long long
const static int N=500,INF=1e18+114514;
int n,m,u,v,w;
int gra[N][N],ans;
bool f;
inline void solve()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)for(int j=1;j<=n;j++)if(i!=j)gra[i][j]=INF;
    for(int i=1;i<=m;i++)
    {
        cin>>u>>v>>w;
        gra[u][v]=w;
        gra[v][u]=w;
    }
    for(int k=1;k<=n;k++)for(int i=1;i<=n;i++)for(int j=1;j<=n;j++) // Floyd
        gra[i][j]=min(gra[i][j],gra[i][k]+gra[k][j]);
    ans=0; // 记录不可以删的边
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=n;j++)
        {
            if(i!=j)
            {
                f=0;
                for(int k=1;k<=n;k++)
                {
                    if(i!=k&&j!=k)
                    {
                        if(gra[i][j]==gra[i][k]+gra[k][j]) // 如果这个条件成立说明这条边可以删除
                        {
                            f=1;
                            break; // 至少有一个 k 点满足条件就可以跳出了
                        }
                    }
                }
                ans+=!f;
            }
        }
    }
    ans/=2; // 无向图，结果要除以二
    cout<<m-ans<<endl;
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