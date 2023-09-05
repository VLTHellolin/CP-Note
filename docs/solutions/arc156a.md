---
comments: true
---

# ARC156A Non-Adjacent Flip 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_arc156_a)

[题面(AtCoder)](https://atcoder.jp/contests/arc156/tasks/arc156_a)

AtCoder Problems 评级：<span style="color: #ffb972">510</span>。

## 题意

- **多组测试数据**。
- 给你一个字符串 $S$，只包含 $1$、$0$，每次你可以选**不相邻**的两个字符，将两字符翻转（分别对于两个字符，将 $1$ 换成 $0$，$0$ 换成 $1$）。
- 问你有没有方法把 $S$ 变成全 $0$ 的字符串，如果有，输出最少的步骤，否则输出 `-1`。
- $1\le T\le 2\times 10^5,\ 3\le N\le 2\times 10^5,\ \sum N\le 2\times 10^5$。

## 思路

一拿到这题的思路是统计字符串中 $1$ 的个数，奇数输出 `-1`，偶数输出个数除以二。这样会 $\texttt{WA}\times 3$，因为题目要求选的两位置**不相邻**。

那么都有哪些特殊情况呢？很容易想到，当只含有两个 $1$ 且两个 $1$ 相邻，需要特判；而 $1111$、$111111$ 等等，这种就不用了。具体如下：

1. $11$、$011$、$110$： `-1`；
2. $0110$： `3`（$0110\to 0{\color{red} 0}1{\color{red} 1}\to {\color{red}1}01{\color{red}0}\to { {\color{red} 0}0{\color{red}0}0}$）；
3. 其他情况（字符串中仅有两个 $1$，且相邻，例如 $1100$）： `2`（$\underbrace{\dotso}_{0} 1100\underbrace{\dotso}_{0}\to \dotso{\color{red}{0}}10{\color{red}1}\dotso\to \dotso0{\color{red}0}0{\color{red}0}\dotso$）。

## 代码

``` cpp
#include <iostream>
using namespace std;
int t, n, ans;
string s;
bool f;
void solve()
{
    cin>>n>>s;
    ans=0; // 多组测试数据，记得初始化
    f=0;
    for(int i=0; i<s.length()-1; i++)
    {
        ans+=(s[i]=='1');
        if(i && (s[i]=='1'&&s[i-1]=='1')) f=1; // 有两个相邻的 1
    }
    if((ans==2)&&f)
    {
        if(s=="11" || s=="011" || s=="110")
            cout<<(-1)<<endl; // 几种无解情况
        else if(s=="0110") cout<<3<<endl;
        else cout<<2<<endl;
    }
    else if(ans%2)
        cout<<(-1)<<endl; // 是奇数，这种情况下怎么操作总会有奇数个 1，显然无解
    else cout<<ans/2<<endl; // 一般情况
}
int main()
{
    cin>>t;
    while(t--) solve(); // 多组测试数据
    return 0;
}
```
