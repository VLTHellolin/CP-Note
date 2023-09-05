---
comments: true
---

# ARC164A Ternary Decomposition 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_arc164_a) | [AtCoder 题面](https://atcoder.jp/contests/arc164/tasks/arc164_a)

题目大意是给定正整数 $N$ 和 $K$，问是否存在一个非负整数序列 $(m_1,\ m_2,\ \dots,\ m_K)$ 满足 $N=3^{m_1}+3^{m_2}+\dots +3^{m_K}$。

嘶，~~好像在哪见过~~。

考虑将 $N$ 表示为三进制形式，比如 $17$ 的三进制形式是 $(122)_3$，所以

$$17=1\times 3^2+2\times 3^1+2\times 3^0$$

也就是

$$17=3^2+3^1+3^1+3^0+3^0$$

所以说，我们只要将 $N$ 的三进制形式的所有位相加（令其为 $S$），当 $S> K$ 时输出 `No`，当 $S=K$ 时输出 `Yes`。

```cpp
do {
    s += n%3;
    n /= 3;
} while(n); // 计算 S

if(s>k) {
    ans = "No";
} else if (s==k) {
    ans = "Yes";
}
```

但是 $S< K$ 的时候不能直接判断输出什么，还是例如 $N=17$，此时 $S=5$ 对吧，那如果 $K=6$，输出什么呢？$K=7$ 呢？

答案是 `No` 和 `Yes`。

至于为什么，我们还是来看三进制形式：$(122)_3$。

题目中并没有限制序列中某一个数最多出现几次，所以最高位的那个 $1$ 可以退 $3$ 给下一位，就成了 $(52)_3$（当然三进制下不能这么写）。

发现了么，当高位退了一个 $1$ 给低位，$S$ 就会减一再加三。所以我们可以推断，当 $S\le K$ 并且 $K-S\equiv 0 \pmod 2$，输出 `Yes`，否则是 `No`。

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll t, n, k, s;
string ans;
void solve() {
    cin >> t;
    while(t--) {
        cin >> n >> k;
        s = 0; // 多测记得初始化！
        do {
            s += n%3;
            n /= 3;
        } while(n);

        if(s>k) {
            ans = "No";
        } else {
            ans = ((k-s)%2) ? "No" : "Yes";
        }

        cout << ans << endl;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```
