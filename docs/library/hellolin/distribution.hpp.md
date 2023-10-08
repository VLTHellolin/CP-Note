---
comments: true
---

# 离散化

当输入的范围很大，或需要转换成数字以提高算法效率时，离散化很有用。

## 定义

``` cpp
namespace hellolin {
template <class T>
class distribution;
}
```

## 使用

| 名称 | 时间 | 作用 |
| ---- | ---- | ---- |
| 构造函数 | $O(1)$。 | 构造一个 distribution。 |
| `void push(T x);` | $O(1)$。 | 向当前序列推入一个元素。 |
| `void push(T x, Args... y);` | $O(n)$。 | 向当前序列推入若干个元素。 |
| `int distribute(T x);` | 第一次 $O(n\log n)$，之后 $O(\log n)$。 | 离散传入的元素，返回一个 $[1, n]$ 中的值，表示元素在序列中的排名。 |
| `T tribute(int x);` | 第一次 $O(n\log n)$，之后 $O(1)$。 | 还原传入的元素。 |
| `int size();` | $O(1)$。 | 返回序列大小。 |
| `void clear();` | $O(1)$。 | 清空序列。 |

???+ info
    关于离散和还原两个函数：由于推入完元素需要先进行排序，这里指的第一次是推入完元素后的第一次，在下一次推入或清空操作之前，时间复杂度都是可以接受的。

## 代码

``` cpp title="distribution.hpp"
#ifndef HELLOLIN_DISTRIBUTION_HPP
#define HELLOLIN_DISTRIBUTION_HPP 1
#include <algorithm>
#include <vector>

namespace hellolin {

template <class T>
class distribution {
private:
    bool ready;
    std::vector<T> d;
    void init() {
        std::sort(d.begin(), d.end());
        d.erase(std::unique(d.begin(), d.end()), d.end());
        ready = 1;
    }

public:
    explicit distribution()
        : ready(0) {}
    void push(T x) {
        d.emplace_back(x);
        ready = 0;
    }
    template <class... Args>
    void push(T x, Args... y) {
        push(x);
        push(y...);
    }
    int distribute(T x) {
        if (!ready) init();
        return std::lower_bound(d.begin(), d.end(), x) - d.begin() + 1;
    }
    T tribute(int x) {
        if (!ready) init();
        return d[x - 1];
    }
    int size() {
        if (!ready) init();
        return d.size();
    }
    void clear() {
        d.clear();
    }
};

} // namespace hellolin

#endif
```