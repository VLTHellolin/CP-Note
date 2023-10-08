---
comments: true
---

# ABC323C World Tour Finals 题解

由于题目保证一定有解，所以对于每个人，贪心地做得分多的题目就好了。

注意已经是最大得分时要输出 $0$。

``` cpp
constexpr int N = 200;
int n, m;
int a[N], r[N], ans, mx;
str s[N];
void solve() {
    std::cin >> n >> m;
    rep(i, m) std::cin >> a[i];
    rep(i, n) {
        std::cin >> s[i];
        s[i] = '#' + s[i];
        rep(j, m) {
            if (s[i][j] == 'o')
                r[i] += a[j];
        }
        r[i] += i;
    }
    mx = *std::max_element(r + 1, r + 1 + n);
    rep(i, n) {
        if(r[i] == mx) {
            std::cout << 0 << '\n';
            continue;
        }
        ans = 0;
        vec<int> wt;
        rep(j, m) if (s[i][j] == 'x')
            wt.pb(a[j]);
        std::sort(wt.begin(), wt.end(), std::greater<int>());
        for (auto &j : wt) {
            ++ans;
            if ((r[i] += j) > mx) break;
        }
        std::cout << ans << '\n';
    }
}
```
