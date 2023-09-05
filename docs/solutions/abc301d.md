---
comments: true
---

# ABC301D Bitmask 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc301_d) | [AtCoder 题面](https://atcoder.jp/contests/abc301/tasks/abc301_d) | [AC 记录](https://www.luogu.com.cn/record/110510447) | $\texttt{\color{73f273}*885}$

$\log(10^{18})\approx 60$，纯纯的贪心。

考虑将 $N$ 转换为二进制表示的形式，那么只需要与 $S$ 进行比较，尽可能往大填就行了。

这种方法虽然可行但代码实现太长了。其实还可以继续简化：

先将 $S$ 中的 `?` 当做 `0` 计算，算出 $S$ 的十进制表示 $T'$。

``` cpp
#define int long long
reverse(s.begin(),s.end());
int t=0;
for(int i=0;i<s.length();i++)
    if(s[i]=='1')
        t+=1ll<<i; // 注意 long long
```


若 $T'>N$ 直接 `-1`，否则，在 $T'\le N$ 的情况下，将 $S$ 中 `?` 填成 `1` 即可。由于只让求 $T$ 的值，所以不必更新 $S$ 了。

``` cpp
if(t>n){
    cout<<-1<<endl;
    return;
}
for(int i=s.length()-1;i>=0;i--) // 优先填大的
    if(s[i]=='?')
        if(t+(1ll<<i)<=n)
            t+=1ll<<i;
```

完整代码如下：

``` cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

string s;
int n,t;
void solve(){
    cin>>s>>n;

    reverse(s.begin(),s.end());
    for(int i=0;i<s.length();i++)
        if(s[i]=='1')
            t+=1ll<<i;
    
    if(t>n){
        cout<<-1<<endl;
        return;
    }
    for(int i=s.length()-1;i>=0;i--)
        if(s[i]=='?')
            if(t+(1ll<<i)<=n)
                t+=1ll<<i;

    cout<<t<<endl;
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```
