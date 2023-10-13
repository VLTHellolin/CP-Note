---
comments: true
---

# ABC224G Roll or Increment 题解

## 前置分析

下面设当前投出的值是 $D$，我们先仔细考虑一下题目中两种操作：显然的，我们不应该在增加之后重投，并且当 $D>T$ 时，我们应该立即重投。

结合上面两点，我们应选择在一些重投之后增加，或者直接增加。

### 直接增加？

显然这种方法仅当 $S<T$ 时适用，此时要花费的代价就是 $A\cdot(T-S)$。

### 先重投再增加？

不妨设区间 $[E, T]$ 并规定只有当 $D\in [E, T]$ 时才执行增加。现在的任务就是该如何选择这个 $E$。

考虑重投部分：设这个区间大小 $L=T-E+1$，重投时投中这个区间的概率是 $\dfrac{L}{N}$，所以我们所有重投的期望成本就是 $\dfrac{BN}{L}$。

考虑增加部分：此时 $D$ 满足 $D\in [E, T]$，所以我们增加的期望成本就是 $\dfrac{\sum\limits^{T}_{i=E}A\cdot (T-i)}{L}=\dfrac{A\cdot (L-1)}{2}$。

综上，完成整个操作的期望成本是 $\dfrac{BN}{L}+\dfrac{A\cdot (L-1)}{2}$。下面提供两种求解答案的方式。

## 二分

时间复杂度 $O(\log T)$，可以通过。

``` cpp
using ld = long double;
ld search(int l, int r) {
    auto f = [&](const int l) {
        return 1.0l * n * b / l + a * (l - 1) / 2.0l;
    };
    while (l < r) {
        int mid = (l + r) >> 1;
        if (f(t - mid + 2) < f(t - mid + 1))
            l = mid + 1;
        else
            r = mid;
    }
    return f(l);
}
void solve() {
    std::cout << std::fixed << std::setprecision(10);
    std::cin >> n >> s >> t >> a >> b;
    if (t == s)
        std::cout << 0 << '\n';
    else if (s > t)
        std::cout << search(1, t) << '\n';
    else
        std::cout << std::min(search(s + 1, t), 1.0l * a * (t - s)) << '\n';
}
```

## 不等式

设定义在 $\R$ 上的函数 $f(x)=\dfrac{BN}{x}+\dfrac{A\cdot (x-1)}{2}$，这里的自变量也即我们前面提到的 $L$。将该式化简：

$$
\begin{aligned}
f(x) &= \dfrac{BN}{x}+\dfrac{A\cdot (x-1)}{2} \\
&= \dfrac{BN}{x}+\dfrac{Ax}{2}-\dfrac{A}{2} \\
\end{aligned}
$$

不管这个常数项 $\dfrac{A}{2}$，发现前面这两项都是非负实数，我们可以利用均值不等式求出该函数的最小值（梦回高一数学）：

$$
\begin{aligned}
\dfrac{BN}{x}+\dfrac{Ax}{2}-\dfrac{A}{2} &\ge \sqrt{2\times \dfrac{BN}{x}\times\dfrac{Ax}{2}}-\dfrac{A}{2} \\
\sqrt{2\times \dfrac{BN}{x}\times\dfrac{Ax}{2}}-\dfrac{A}{2} &=\sqrt{NAB}-\dfrac{A}{2} \\
\end{aligned}
$$

综上，$f(x)$ 在 $x=\sqrt{\dfrac{2\times NB}{A}}$ 处最小。又易证 $f(x)$ 为凸函数，在代码实现中我们只需计算 $f(x-1),\ f(x),\ f(x+1)$ 几个值就行了。

时间复杂度 $O(1)$，可以通过。

``` cpp
void solve() {
    auto f = [&](const int l) {
        return 1.0l * n * b / l + a * (l - 1) / 2.0l;
    };
    std::cout << std::fixed << std::setprecision(10);
    std::cin >> n >> s >> t >> a >> b;
    ld ans = 0;
    if (s != t) {
        ans = 1.0l * n * b;
        if (s <= t)
            chmin(ans, 1.0l * a * (t - s));
        chmin(ans, std::min(f(1), f(t)));
        p = std::sqrt(2.0l * n * b / a);
        if (p <= t) {
            chmin(ans, f(p));
            if (p >= 1) chmin(ans, f(p - 1));
            if (p <= t) chmin(ans, f(p + 1));
        }
    }
    std::cout << ans << '\n';
}
```

## 后记

推式子真的太爽啦！！！但这题 *2374 多少有点虚高了罢。