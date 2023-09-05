---
comments: true
---

# ARC048B AtCoderでじゃんけんを 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_arc048_b)

[题面(AtCoder)](https://atcoder.jp/contests/arc048/tasks/arc048_b)

AtCoder Problems 评级难度：<span style="color: #46ffff">1290</span>。

## 思路

对于每一级 AtCoder 值，我们设 $x$ 为比当前 AtCoder 值低的人数，$a$ 为出石头人数，$b$ 为出剪刀人数，$c$ 为出布人数，则对于每一个 AtCoder 值在这级的人：

- 出石头，胜场为 $b+x$，平场为 $a-1$，输场为 $N-x-a-b$；
- 出剪刀，胜场为 $c+x$，平场为 $b-1$，输场为 $N-x-b-c$；
- 出布，胜场为 $a+x$，平场为 $c-1$，输场为 $N-x-a-c$。

统计完成后，可以直接输出。

复杂度约为 $O(\max{(N,\max{(R)})})$。

## 代码

``` cpp
#include <bits/stdc++.h>
using namespace std;
#define fi first
#define se second
const static int N = 114514;
int n,tmp,tmp_,x,s;
// 1 石头 / 2 剪刀 / 3 布
struct p
{
    int a,b,c;
    int w[4],l[4],f[4]; // 胜场，输场，平场
}c[N];
vector<pair<int,int>>id;
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>tmp>>tmp_;
        
        s=max(s,tmp);
        id.push_back({tmp,tmp_});

        p&j=c[tmp];
        if(tmp_==1)++j.a;
        else if(tmp_==2)++j.b;
        else ++j.c;
    }
    for(int i=1;i<=s;i++)
    {
        p&j=c[i];

        j.w[1]=j.b+x;
        j.l[1]=n-j.a-j.b-x;
        j.f[1]=j.a-1;

        j.w[2]=j.c+x;
        j.l[2]=n-j.c-j.b-x;
        j.f[2]=j.b-1;

        j.w[3]=j.a+x;
        j.l[3]=n-j.a-j.c-x;
        j.f[3]=j.c-1;

        x+=j.a+j.b+j.c;
    }
    for(auto i:id)
        cout<<c[i.fi].w[i.se]<<' '<<c[i.fi].l[i.se]<<' '<<c[i.fi].f[i.se]<<endl;
    return 0;
}
```