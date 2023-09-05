---
comments: true
---

# ABC180D Takahashi Unevolved 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

**UPD1**：修改了代码中的一个小错误。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_abc180_d)

[题面(AtCoder)](https://atcoder.jp/contests/abc180/tasks/abc180_d)

## 题意

- 给定正整数 $X,\ Y,\ A,\ B$。初始，**STR** 值等于 $X$，**EXP** 值为 $0$；
- 可以选择让 **STR** 值乘 $A$ 或加 $B$，两种操作都会使 **EXP** 值加 $1$；
- **STR** 值不能大于 $Y$，问你经过若干次操作后，**EXP** 值最大是多少；
- $1\le X< Y\le 10^{18},\ 2\le A\le 10^9,\ 1\le B\le 10^9$。

## 思路

这是一道贪心题目。

既然要使得 **EXP** 值尽量高，就是让操作次数尽可能多，即优先选择 **STR** 值增长较慢的方式。

如果直接进行模拟的话，会 TLE。毕竟 $10^{18}$ 的范围可不是闹着玩的。

考虑优化，可以试着找找使 **STR** 增长较慢的规律：

题目中 **STR** 增长的两种方式是乘 $A$ 或加 $B$，那么应该先乘还是先加呢？

如果先加后乘各一次，**STR** 值变化如下：

$$X \to X + B \to A \cdot (X + B)$$

如果先乘后加各一次：

$$X \to A\cdot X \to A\cdot X + B$$

很容易看出来，后面这种方法（先乘后加）的 **STR** 值增长较少。

当 $X<\frac{Y}{A}$（再乘一次不会超出 $Y$）且 $X\cdot A<X+B$（乘 $A$ 比加 $B$ 更划算）时，使 $X$ 乘 $A$。

接下来的加法运算就没必要循环了，直接加上 $\frac{Y-X-1}{B}$ 就可以了（分子减一是为了保证加上的这些不会超出 $Y$ 的范围）。

## 代码

``` cpp
// Coder          : Hellolin
// Time           : 2023-02-03 13:01:22
// Problem        : [ABC180D] Takahashi Unevolved
// Contest        : Luogu
// URL            : https://www.luogu.com.cn/problem/AT_abc180_d
// Time Limit     : 2000 ms
// Memory Limit   : 1 GiB

#include <iostream>
#define ll long long // 开 long long 防止溢出

ll x, y, a, b, ans;

int main()
{
    // 加速输入输出
    std::ios::sync_with_stdio(false);
    
    std::cin>>x>>y>>a>>b;

	// 循环进行乘法操作
    for (ans=0; x<(y/a) && (x*a)<(x+b); x*=a) ++ans;

	// 进行加法操作
    std::cout<<ans+((y-x-1)/b)<<std::endl;

    return 0;
}
```
