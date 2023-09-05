---
comments: true
---

# ARC124B XOR Matching 2 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_arc124_b)

[题面(AtCoder)](https://atcoder.jp/contests/arc124/tasks/arc124_b)

AtCoder Problems 评级难度：<span style="color: #ffb972">788</span>。

## 题意

- 给你长度为 $N$ 的序列 $a$ 和 $b$。
- 求有多少个非负整数 $x$ 满足：有一种修改序列 $b$ 的方法使得 $\forall i (1\le i\le N),  a_i \oplus b_i = x$。
- 按升序输出所有满足条件的 $x$。
- $1\le N\le 2000, 0\le a_i,b_i<2^{30}$。

## 思路

不难证明，当 $a\oplus b=c$ 时，$a\oplus c=b$ 成立。

所以说，问题可以从求 $a_i \oplus b_i=x$ 转化为求 $a_i \oplus x=p_i$（若 $p=b$，则 $x$ 符合条件）。

枚举 $x$，让 $p_i = a_1 \oplus x$，再对两个序列进行比较，时间复杂度 $O(N^2 \log N)$。

注意答案可能有重复的，可以把答案扔到 set 里，既能去重还能自动排序。

``` cpp
#include <bits/stdc++.h>
#define each(x,y) for(auto &x:y)
using namespace std;

const int N = 2020;
int n, tmp;
vector<int> a, b, p;
set<int>ans;

bool judge(int x)
{
    p=a;
    each(i,p)
        i^=x;
    
    sort(p.begin(), p.end());
    return b==p;
}

void solve()
{
    cin>>n;
    for(int i=1; i<=n; i++)
    {
        cin>>tmp;
        a.push_back(tmp);
    }
    for(int i=1; i<=n; i++)
    {
        cin>>tmp;
        b.push_back(tmp);
    }
    sort(b.begin(), b.end());
    each(i,b)
    {
        tmp=a[0]^i;
        if(judge(tmp)) ans.insert(tmp);
    }
    cout<<ans.size()<<endl;
    each(i,ans)
        cout<<i<<endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);

    solve();
    return 0;
}
```

