---
comments: true
---

# ABC323D Merge Slimes 题解

!!! abstract
    对于两个相同的史莱姆，我们可以把它们合成一个更大的史莱姆。而更大的史莱姆在之后又可被合成，以此类推。

这个过程可以用 `std::map` 来实现，由于其键值自动排序的特性，我们 range-for 访问到的元素键值一定是递增的。同时，`std::map` 是一个 **关联式容器**，所以在插入元素时，任何迭代器都不会失效。

具体实现是，先把每个史莱姆的体积作为键值，存储他们的数量，之后遍历每个元素，如果元素中史莱姆数量 $b$ 大于等于两个，那么更大的史莱姆会出现 $\lfloor\frac{b}{2}\rfloor$ 个，同理这个史莱姆只会剩下 $b\bmod 2$ 个。

``` cpp
constexpr int N = 1e5 + 11;
int n, s, c, ans;
std::map<i64, i64> buc;
void solve() {
    std::cin >> n;
    rep(i, n) {
        std::cin >> s >> c;
        buc[s] = c;
    }
    for(auto &[a, b] : buc) {
        if(b >= 2) {
            buc[a * 2] += b / 2;
            ans += b % 2;
        } else {
            ans += b;
        }
    }
    std::cout << ans << '\n';
}
```