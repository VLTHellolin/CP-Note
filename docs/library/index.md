---
comments: true
---

# Hellolin Library

实际上，我从 2023 年初左右就启动了编写 Hellolin Library 的计划，断断续续写到了 8 月左右，Hellolin Library 迎来了第一个 Preview 版本。

开源协议我选择了 MPLv2，~~对于我这种不喜欢 MIT 也不喜欢 GPL 的很合适~~。

你可以自由地参考或使用这个算法库，但请留意：

1. 不要过多依赖算法库。**特别是在洛谷等评测平台上，不要直接抄袭某段代码**，否则造成的一切后果请自负。
2. 留意代码中的 breaking updates。如果你对哪个版本不满意，可以随时在 GitHub 上找到完整的历史版本。
3. 由于个人的代码能力有限，某些代码的可读性不高，还请谅解。

!!! warning
    推荐的语言标准是 C++20，最低可编译标准是 C++14，低于此标准不保证能编译成功。

    如果你不知道怎么设置标准，可参考对应编译器的文档。比如 GCC / LLVM：`-std=c++20`。

!!! bug
    正如上面所说，这个算法库由我个人维护，出现缺陷可能不能及时修复。各位大佬可至 GitHub 的 issue 区反馈。

下面是目录（~~左边也有~~）。

## hellolin

- [离散化 `distribution.hpp`](./hellolin/distribution.hpp.md)
- [高精度运算 `huge.hpp`]()
- [输入输出流 `io_fleet.hpp`]()
- [内存池 `memory_pool.hpp`]()
- [自取模整数 `modint.hpp`]()
- [随机数生成器 `random_number.hpp`]()

## hellolin/ds

- [并查集 `dsu.hpp`]()

## hellolin/graph

- [双连通分量 `bcc.hpp`]()
- [欧拉回路 `euler.hpp`]()
- [最大费用最大流 `max_cost_flow.hpp`]()
- [最大流 `max_flow.hpp`]()
- [最小费用最小流 `min_cost_flow.hpp`]()
- [强连通分量 `scc.hpp`]()
- [two-sat 问题 `two_sat.hpp`]()

## hellolin/string

- [Lyndon 定理 `lyndon.hpp`]()
- [马拉车算法 `manacher.hpp`]()
- [后缀数组 `suffix_array.hpp`]()

## hellolin/utility

- [计算资源占用 `process_occupy.hpp`]()
