---
comments: true
---

# ABC302D Impartial Gift 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc302_d) | [AtCoder 题面](https://atcoder.jp/contests/abc302/tasks/abc302_d) | [AC 记录](https://www.luogu.com.cn/record/111043578) | $\texttt{\color{f2b373}*682}$

说是两人的礼物差不能超过 `d`，要使和最大，那就枚举 $A$，再二分 $B$，就行了，时间复杂度 $O(n\log n)$。

可以用标准库里的 `lower_bound` 和 `upper_bound` 函数来二分。

设当前枚举 $A$ 序列，枚举到元素 $x$，那么在 $B$ 序列中我们要找到第一个大于等于 $x-d$ 值的位置（设它为 $l$），还要找第一个大于 $x+d$ 值的位置（设它为 $r$，注意这里右端点是不合法的值），这样一来，若 $r-l\ge 1$，说明合法值区间存在，要取最大就是 $r-1$ 位置的值加上 $x$ 就行了。

``` cpp
auto l=lower_bound(b.begin(),b.end(),x-d);
auto r=upper_bound(b.begin(),b.end(),x+d);
if(r-l>=1)
    ans=max(ans,x+*--r);
```

如果觉得难以理解，来看个栗子（样例 1）：

$$
n=2,\ m=3,\ d=2\\
A=3,10\\
B=2,5,15
$$

枚举序列 $A$ 的元素，当前是第一个。

此时 $x-d=1,\ x+d=5$，所以二分到的区间如下（青色）所示：

$$
A={\color{red}3},10\\
B={\color{cyan}2,5},{\color{yellow}15}
$$

实际上 $r$ 指向 $15$，但这里我们二分的是大于 $x+d$，说明 $r$ 不合法，所以要退一个位置。

那么第一个答案就是 $3+5=8$。

同理，枚举到第二个元素时：

$$
A=3,{\color{red}10}\\
B=2,5,{\color{yellow}15}
$$

我们发现左端点和右端点都指向 $15$，而实际上 $15$ 对于 $10$ 是个不合法值（因为它是右端点），所以此时不应计入答案。



所以样例 1 的最终答案是 $8$。

附上完整代码：

``` cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

int n,m,d,ans;
vector<int>a,b;
void solve(){
    cin>>n>>m>>d;
    a.resize(n),b.resize(m);
    for(int&i:a)cin>>i;
    for(int&i:b)cin>>i;
    // 二分前保证区间有序
    sort(a.begin(),a.end());
    sort(b.begin(),b.end());
    ans=-1;
    for(int x:a){
        auto l=lower_bound(b.begin(),b.end(),x-d);
        auto r=upper_bound(b.begin(),b.end(),x+d);
        if(r-l>=1)
            ans=max(ans,x+*--r);
    }
    cout<<ans<<endl;
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```
