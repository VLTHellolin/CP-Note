---
comments: true
---

# ABC299D Find by Query 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc299_d) | [AtCoder 题面](https://atcoder.jp/contests/abc299/tasks/abc299_d) | [AC 记录](https://www.luogu.com.cn/record/110148355) | $\texttt{\color{f2b373}*763}$

注意到 $\log(2\times 10^5)\approx 17$，小于最大次数 $20$，所以第一反应是二分。

但赛时我在这题犹豫了一下，真的可以用二分吗？

考虑一种数据：

$$00000\dots 11111$$

很明显像这种数据，二分可以秒杀。

之后，题中又告诉我们：$S_1=0$ 并且 $S_n=1$。

那么，当 $S_{mid}=0$ 时，一定存在 $S_{mid}=0$ 且 $S_r=1$；当 $S_{mid}=1$ 时，一定存在 $S_l=0$ 且 $S_{mid}=1$；

也就是说，无论什么样的数据，最终一定可以化成上面这种秒杀形式。

不信的话，我们举例看一下（红色是当前区间）：

$$
{\color{red}0001110101}\\
{\color{red}0001}110101\\
00{\color{red}01}110101\\
000{\color{red}1}110101
$$

完整代码如下：

```cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

int n;
bool w;
int l,r,mid;

void solve() {
    cin>>n;
    l=1,r=n;
    while(l<=r){
        mid=(l+r)>>1;
        cout<<"? "<<mid<<endl;
        cin>>w;
        if(!w)l=mid+1;
        else r=mid-1;
    }
    cout<<"! "<<r<<endl; // 一定注意最后输出的是 r
}


int32_t main() {
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```
