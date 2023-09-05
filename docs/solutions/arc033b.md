---
comments: true
---

# ARC033B メタ構文変数 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_arc033_2)

[题面(AtCoder)](https://atcoder.jp/contests/arc033/tasks/arc033_2)

AtCoder Problems 评级难度：<span style="color: #ffb972">789</span>。

## 题意

- 给两个长度为 $N$ 的集合 $A$ 与 $B$，求 $\frac{\operatorname{size}(A\cap B)}{\operatorname{size}(A\cup B)}$。
- $1\le N\le 10^5,\ 1\le A_i, B_i\le 10^9$。

## 思路

对这些数计数，所有出现次数大于等于 $2$ 的元素的个数就是 $\operatorname{size}(A\cap B)$。下标数可能很大，所以扔 `map` 里。

将这些数扔到 `set` 里，`set.size()` 就是 $\operatorname{size}(A\cup B)$。

## 代码

``` cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
set<ll>x;
map<ll,ll>y;
int n,m,a,ans;
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n>>m;
    for(int i=1; i<=n+m; i++)
    {
        cin>>a;
        x.insert(a);
        ++y[a];
    }
    for(auto i:y)ans+=(i.second>=2);
    cout<<fixed;
    cout.precision(10);
    cout<<ans*1.0/x.size()<<endl;
    return 0;
}
```