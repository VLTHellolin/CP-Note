---
comments: true
---

# 向量 - vector

## 简介

`vector` 相当于一个不定长的数组，随机访问复杂度为常数，插入删除复杂度为线性，在末尾插入删除复杂度为均摊常数。

!!! tip "提示"

    vector 是 C++ 标准程序库里最基本的容器，大多数状况下都很有效率。vector 设计之初即是为了改善 C 语言原生数组的种种缺失与不便，而欲提供一种更有效、更安全的数组。vector 的使用接口刻意模拟 C 语言原生数组，较明显的差异在于存储器管理，原生数组必须在宣告数组的时候明确指定数组长度(例如 int a[5])，但是 vector 不需要指定，而是会在执行期依据状况自我调整长度，动态增大容量。[^1]

[^1]: 摘自 [维基百科](https://zh.wikipedia.org/wiki/Vector_(STL))。

``` cpp title="vector 的定义"
template <class T, class Alloc = std::allocator<T> >
class vector;
```

## 模板特化

[`vector<bool>`](./vector_bool.md) 类模板特化，相当于不定长 `bitset`。

## 常用成员函数

| 名称 | 定义 | 作用 | 复杂度 | 备注 |
| ---- | ---- | --- | ----- | ---- |
| 构造函数 | `vector();` | 默认构造函数，构造空容器。 | 常数。 |  |
| 构造函数 | `vector(size_type n, const T& value);` | 构造含 `n` 个值为 `value` 的容器。 | $O(n)$ |  |
| 构造函数 | `vector(size_type n);` | 构造含 `n` 个默认值的容器。 | $O(n)$ |  |
| 构造函数 | `vector(InputIt first, InputIt last);` | 构造含 `[first, last)` 值的容器。 | $O(n)$，$n$ 为 `first` 与 `last` 的距离。 |  |
| 构造函数 | `vector(const vector& x);` | 构造含 `x` 中值的容器。 | $O(\mathrm{x.size()})$ |  |
| 构造函数 | `vector(vector&& x);` | 构造含 `x` 中值的容器。 | 常数。 | 自 C++11 |
| 构造函数 | `vector(initializer_list<T> l);` | 构造含 `l` 中值的容器。 | $O(\mathrm{l.size()})$ |  |
| `operator=` | `vector& operator=(const vector& x)` | 将 `x` 中的值赋值给当前容器。 | $O(\mathrm{x.size()})$ |  |
| `operator=` | `vector& operator=(initializer_list<T> l)` | 将 `l` 中的值赋值给当前容器。 | $O(\mathrm{l.size()})$ |  |
| `operator[]` | `reference operator[](size_type p)` | 返回位于 `p` 位置元素的引用。 | 常数。 |  |
| `size()` | `size_type size();` | 返回容器大小。 | 常数。 |  |
| `resize()` | `void resize(size_type n);` | 修改容器大小。 | $O(n)$ |  |
| `resize()` | `void resize(size_type n, const value_type& value);` | 修改容器大小，若 `n` 大于原大小，后续插入的元素等于 `value`。 | $O(n)$ |  |
| `reserve()` | `void reserve(size_type n);` | 增加容量，不改变容器大小。 | $O(n)$ |  |
| `clear()` | `void reserve();` | 删除所有元素，不改变容量，容器大小变为 $0$。 | $O(n)$ |  |
| `insert()` | `iterator insert(const_iterator p, const T& value);` | 在 `p` 之前插入 `value`，返回指向新插入元素的迭代器。 | $O(m)$，$m$ 为 `p` 到结尾的距离。 |  |
| `insert()` | `iterator insert(const_iterator p, size_type n, const T& value);` | 在 `p` 之前插入 `n` 个 `value`，返回指向第一个插入元素的迭代器。 | $O(n+m)$，$m$ 为 `p` 到结尾的距离。 |  |
| `insert()` | `iterator insert(const_iterator p, InputIt first, InputIt last);` | 在 `p` 之前插入 `[first, last)`，返回指向第一个插入元素的迭代器。 | $O(n+m)$，$n$ 为 `first` 与 `last` 的距离，$m$ 为 `p` 到结尾的距离。 |  |
| `insert()` | `iterator insert(const_iterator p, initializer_list<T> l);` | 在 `p` 之前插入 `l` 中的值，返回指向第一个插入元素的迭代器。 | $O(\mathrm{l.size()}+m)$，$m$ 为 `p` 到结尾的距离。 |  |
| `emplace()` | `iterator emplace(const_iterator p, Args&&...args);` | 在 `p` 之前插入由 `args` 就地构造的值，返回指向该值的迭代器。 | $O(m)$，$m$ 为 `p` 到结尾的距离。 | 自 C++11 |
| `erase()` | `iterator erase(iterator p);` | 移除 `p`，返回移除后的迭代器。 | $O(n)$ |  |
| `erase()` | `iterator erase(iterator first, iterator last);` | 移除 `[first, last)`，返回移除后的迭代器。 | $O(n)$ |  |
| `push_back()` | `void push_back(const T& value);` | 将 `value` 添加到容器尾部。 | 均摊常数。 | |
| `pop_back()` | `void pop_back();` | 移除容器尾部的元素。 | 常数。 | |
| `emplace_back()` | `void emplace_back(Args&&...args);` | 将由 `args` 就地构造的值添加到容器尾部。 | 均摊常数。 | 自 C++11 |
| `emplace_back()` | `reference emplace_back(Args&&...args);` | 将由 `args` 就地构造的值添加到容器尾部，返回这个值。 | 均摊常数。 | 自 C++17 |
| `swap()` | `void swap(vector& x);` | 交换该容器和 `x`。 | 常数。 | |

## 本质

`vector` 的底层维护了他的 **容量** 和 **大小**，容量可以理解为最多能含多少元素，也就是底层定长数组的大小。

当插入元素即将超过容量时，容器会新分配一个容量大一倍的数组，把原来的复制过来后将原来的内存释放。

这个操作是均摊常数的，遇到卡常的题目可以预先使用 `reserve()` 开大容量。

## 迭代器失效

在对容器进行修改大小等操作时，之前存储的迭代器可能会失效，在失效的迭代器上操作是无效的，可能会出现运行时错误。

下表列出了会失效的几种情况。[^2]

[^2]: 摘自 [cppreference](https://zh.cppreference.com/w/cpp/container/vector)。

| 操作 | 失效迭代器 |
| ---- | ------- |
| `swap` | `end()` |
| `clear`、赋值 | 全部失效 |
| 使用 `reserve`、`shrink_to_fit` 更改容量 | 全部失效 |
| 使用 `resize` 更改大小 | 被删除元素和 `end()` |
| `erase` | 被删除元素、其之后的所有元素和 `end()` |
| `push_back`、`emplace_back` | `end()` |
| `insert`、`emplace` | 被插入元素之后的所有元素和 `end()` |
| `pop_back` | 被删除元素和 `end()` |
