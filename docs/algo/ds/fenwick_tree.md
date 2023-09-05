---
comments: true
---

# 树状数组

树状数组维护的运算必须具有结合律，并且可逆（就是知道 $a$ 与 $b$ 操作的结果以及 $a$ 之后，能求出来 $b$）。

所以这就是个坑，若是模意义下乘就必须保证每一个数都有它的逆元。

## 单点修改

``` cpp
void add(int x, int val) {
    for(int i=x; i<=n; i+=lowbit(i))
        a[i] += val;
}
```

## 区间查询

``` cpp
int query(int x) { // (1)
    int res = 0;
    for(int i=x; i; i-=lowbit(i))
        res += a[i];
    return res;
}
int query(int l, int r) {
    return query(r) - query(l-1);
}
```

1. 该函数返回的是 $[1,\ x]$ 这个区间之和。

## 树状数组上二分

感觉没啥用。

二分 $x$ 的话，先把 $x$ 拆成二进制，从高到底逐位考虑。