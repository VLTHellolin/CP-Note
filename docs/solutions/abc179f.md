---
comments: true
---

# ABC179F Simplified Reversi 题解

一个边长为 $n$ 的正方形，中间 $(n-2)\times (n-2)$ 每个方格都有一颗黑子，底部和右边每个方格都有一棵白子。

有以下两种询问：

- `1 x`：在 $(1,x)$ 处放一颗白子，将其与向下最近的白子之间的所有黑子替换为白子。

- `2 x`：在 $(x,1)$ 处放一颗白子，将其与右边最近的白子之间的所有黑子替换为白子。

处理完所有的 $q$ 次查询后，问剩下多少黑子？

---

考虑如下的初始局面：

![](https://cdn.luogu.com.cn/upload/image_hosting/s8vzo3p3.png)

此时，假设有询问 `1 x`：

![](https://cdn.luogu.com.cn/upload/image_hosting/ufprvv3z.png)

替换完成后，我们发现右边这些方格不再受 2 操作的影响，于是可以缩小长宽：

![](https://cdn.luogu.com.cn/upload/image_hosting/xgywzqbo.png)

反过来同理。

之后，使用两个数组存储每行每列剩的黑子即可。两个数组只会在缩小长宽时更新，故时间复杂度为 $O(n+q)$。

``` cpp
constexpr int N = 2e5 + 5;
int a[N], b[N];

void main() {
    int n, c, r;
    read(n);
    std::fill(&a[1], &a[n + 1], n);
    std::fill(&b[1], &b[n + 1], n);
    ll ans = (1ll * n - 2ll) * (1ll * n - 2ll);
    c = r = n;
    for (int q = read(); q--;) {
        int op, x;
        read(op, x);
        if (op == 1) {
            if (x < c) {
                std::fill(&b[x], &b[c], r);
                c = x;
            }
            ans -= b[x] - 2;
        } else {
            if (x < r) {
                std::fill(&a[x], &a[r], c);
                r = x;
            }
            ans -= a[x] - 2;
        }
    }
    writeln(ans);
}
```

也可用线段树解决问题，区间 chmin + 单点查询即可。这里就不贴代码了。
