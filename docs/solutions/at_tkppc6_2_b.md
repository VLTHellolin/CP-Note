---
comments: true
---

# AT_tkppc6_2_b Replace to the Other 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_tkppc6_2_b)

[题面(AtCoder)](https://atcoder.jp/contests/tkppc6-2/tasks/tkppc6_2_b)

AtCoder Problems 评级难度：<span style="color: #bfbfbf">None</span>。

## 思路

对于 $S$ 和 $T$，先将偶数位上的字符反转，于是操作就变成了：

- 对于 $S_i\not =S_{i+1}\ (1\le i< N)$，反转 $S_i$ 与 $S_{i+1}$。

不难想到的是，操作后 $S$ 和 $T$ 串中 `A` 的数量应分别和之前一致。所以如果 $S$ 和 $T$ 串中 `A` 的数量不一样，直接 `-1`。

否则，如果 `A` 的数量一样（令其为 $X$），令 $S$ 当中 `A` 的位置是 $P_1,\ P_2,\ P_3,\ \dots,\ P_X$，$T$ 当中 `A` 的位置是 $Q_1,\ Q_2,\ Q_3,\ \dots,\ Q_X$。

要使 $P$ 中元素都与 $Q$ 相等，不难想到对于每个 $i (1\le i\le X)$，要执行的操作数是 $|P_i-Q_i|$（可以理解成数轴上两点之间的距离）。举例如下：

$$S=\texttt{ABBB},\ T=\texttt{BBAB}\ (0)$$

$$S=\texttt{ABBB},\ T=\texttt{B{\color{red}AB}B}\ (1)$$

$$S=\texttt{ABBB},\ T=\texttt{{\color{red}{AB}}BB}\ (2)$$

那么答案就是 $\sum_{i=1}^X |P_i-Q_i|$ 了。

时间复杂度大约是 $O(N)$。

## 代码

``` cpp
#include <bits/stdc++.h>
// #include <atcoder/all>
using namespace std;
#define int long long
int n,ans;
string s,t;
vector<int> p,q;
inline void solve()
{
    cin>>n>>s>>t;
    for(int i=0;i<s.length();i++)
    {
        if(i&1)
        {
            s[i]='A'+(s[i]=='A');
            t[i]='A'+(t[i]=='A');
        }
        if(s[i]=='A')p.push_back(i);
        if(t[i]=='A')q.push_back(i);
    }
    if(p.size()!=q.size())
    {
        cout<<-1<<endl;
    }
    else
    {
        for(int i=0;i<p.size();i++)
        {
            ans+=abs(p[i]-q[i]);
        }
        cout<<ans<<endl;
    }
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