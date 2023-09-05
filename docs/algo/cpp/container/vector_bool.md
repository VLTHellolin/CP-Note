---
comments: true
---

# 向量中的 bool 特化 - vector&lt;bool&gt;

和 `bitset` 一样每个元素只占据 1 bit。

`vector<bool>` 相当于一个不定长 `bitset`（因为 `bitset` 的大小在编译期就确定了）。

但是 `vector<bool>` 通过 `operator[]` 访问时返回的不是 `bool&` 而是 `reference`，在写题时需要注意。