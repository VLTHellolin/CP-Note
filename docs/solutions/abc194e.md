---
comments: true
---

# ABC194E Mex Min 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_abc194_e)

[题面(AtCoder)](https://atcoder.jp/contests/abc194/tasks/abc194_e)

AtCoder Problems 评级难度：<span style="color: #72ff72">1088</span>。

## 题意

- 给你一个长度为 $N$ 的序列 $A$，求 $A$ 中所有长度为 $M$ 的连续子序列中，最小的 $\operatorname{mex}$ 值（定义 $\operatorname{mex}$ 为序列中最小未出现的自然数）。
- $1\le M\le N\le 15\times 10^5,\ 0\le A_i< N$。

## 思路

看到连续子序列，一下子想到类似滑动窗口的方法。

先算出窗口在初始位置时的 $\operatorname{mex}$ 值，在向右移动时检查移出的数，若该移出的数在当前窗口中出现次数变为 $0$，更新 $\operatorname{mex}$ 值即可。

``` cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

const static int N=2e6;
int n,m,a[N],ans;
map<int,int>t;

inline void solve()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)cin>>a[i];
    for(int i=1;i<=m;i++)++t[a[i]]; // 初始窗口
    for(int i=0;i<=n;i++) // 计算初始窗口 mex 值，注意范围要到 n
        if(!t[i])
        {
            ans=i;
            break;
        }
    
    for(int i=m+1;i<=n;i++) // 窗口向右滑动
    {
        --t[a[i-m]],++t[a[i]]; // 更新
        if(!t[a[i-m]]) // 发现 mex 值可能有变化
            ans=min(ans,a[i-m]);
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