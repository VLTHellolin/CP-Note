---
comments: true
---

# AGC049B Flip Digits 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_agc049_b)

[题面(AtCoder)](https://atcoder.jp/contests/agc049/tasks/agc049_b)

AtCoder Problems 评级难度：<span style="color: #72ff72">1171</span>。

## 题意

- 一个 $N$ 长度的 01 串 $S$，你可以进行若干次如下操作：
   - 选一个不在首位的 $1$ 位，反转这一位和前一位。
- 问有没有可能将 $S$ 变为 $T$，输出 `-1` 或最小次数。

## 思路

这个操作可以看作是将第 $i(2\le i\le N,\ S_i=1)$ 向左移一位（左移到的值做异或）。

开一个 $a$ 队列记录 $T$ 中 $1$ 的位置。再开一个 $b$ 队列，用处下面会提到。

若 $S$ 出现了 $1$，代表这个 $1$ 要去“执行任务”了，分两种情况（当前下标记作 $i(1\le i\le N)$）：

1. $a$ 队列空，代表之前的 $1$ 均已满足情况，再分两种情况：
   1. $b$ 队列空，代表这之前的子串都满足情况，唯有此位不满足（因为 $T$ 的这个位置不是 $1$），将此位入 $b$ 队列。
   2. $b$ 队列非空，代表这之前存在一个 $j(1\le j< i,\ S_j=1,\ T_j=0)$，此时需要将这个 $j$ 位从 $1$ 换为 $0$，记录步骤数，再将此位弹出 $b$ 队列。
2. $a$ 队列非空，代表这之前存在一个 $j(1\le j< i,\ S_j=0,\ T_j=1)$，此时需要将这个 $j$ 位从 $0$ 换到 $1$，记录步骤数，再将此位弹出 $a$ 队列。

**注意 $S$ 与 $T$ 要同时进行操作！否则队列记录的将不是之前的、最近的不同位！**

完成操作后，若 $a$ 或 $b$ 非空，代表操作完成后仍然至少存在一个 $i(1\le i\le N,\ S_i\not =T_i)$。无解，输出 `-1`。

否则，输出记录的步骤。

## 代码

``` cpp
#include <bits/stdc++.h>
using namespace std;
int n;
string s,t;
queue<int>a,b;
long long ans;
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n>>s>>t;
    for(int i=0; i<n; i++)
    {
        if(t[i]=='1')a.push(i);
        if(s[i]=='1')
        {
            if(a.empty())
            {
                if(b.empty()) b.push(i);
                else
                {
                    ans+=i-b.front(); // 记录步骤数
                    b.pop();
                }
            }
            else
            {
                ans+=i-a.front(); // 记录步骤数
                a.pop();
            }
        }
    }
    if(a.size()||b.size())cout<<-1<<endl;
    else cout<<ans<<endl;
    return 0;
}
```