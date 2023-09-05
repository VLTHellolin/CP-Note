---
comments: true
---

# ABC176D Wizard in Maze 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_abc176_d)

[题面(AtCoder)](https://www.luogu.com.cn/remoteJudgeRedirect/atcoder/abc176_d)

## 题意

- $H$ 行 $W$ 列的方格，每个方格都是 `#`（墙）或者 `.`（路）。
- 魔术师一开始在位置 $(C_h, C_w)$，他想到达位置 $(D_h, D_w)$。
- 魔术师有两种移动方式：上下左右走，或者使用魔法瞬移到以当前格为中心的 $5 \times 5$ 正方形中任意一格。
- 问魔术师最少用几次瞬移魔法能到目标位置。

## 思路

一道很适合练习广搜 (BFS) 的题。

第二种方法要尽量少用，所以可以试着能否通过第一种方法走到终点，如果不能就用一下下第二种方法，继续重复判断能否走到终点。

基于上面的想法，可以开一个双端队列，第一种方法的优先级高，所以把第一种走的方法压在队列前面，第二种方法压在队列后面。

其余的细节都写在代码里啦！

## 代码

``` cpp
// Coder          : Hellolin
// Time           : 2023-02-06 12:32:38
// Problem        : [ABC176D] Wizard in Maze
// Contest        : Luogu
// URL            : https://www.luogu.com.cn/problem/AT_abc176_d
// Time Limit     : 2000 ms
// Memory Limit   : 1 GiB

#include <iostream>
#include <utility> // pair 大法好
#include <deque>
#include <cstring>
using namespace std;
#define pi pair<int, int>
#define fi first
#define se second
#define rep(x, y, z) for (int x = y; x <= z; x++)

const int MAX = 1e3 + 114;

deque<pi> a;
int dx[5] = {0, 1, -1, 0, 0};
int dy[5] = {0, 0, 0, 1, -1};
int path[MAX][MAX];

int h, w, ans;
// 开始位置 结束位置 当前位置 下一个移动位置
pi str, fin, now, mv;
char tmp;
bool img[MAX][MAX];

void init()
{
    ans = -1;
    memset(path, -1, sizeof(path));
    // 记得起始位置 path 设 0
    path[str.fi][str.fi] = 0;
    a.clear();
    // 在前面压一个开始位置
    a.push_front(str);
}

void bfs()
{
    while (a.size()) // while (!a.empty())
    {
        now = a.front();
        a.pop_front();

        if (now == fin) // 到终点了
        {
            ans = path[now.fi][now.se];
            return;
        }

        // 上下左右四个方向
        rep(i, 1, 4)
        {
            mv = now;
            mv.fi += dx[i];
            mv.se += dy[i];

            if (mv.fi < 1 || mv.fi > h || mv.se < 1 || mv.se > w) // 越界检查
                continue;

            if (!img[mv.fi][mv.se])
            {
                // 当前这条路还没走过，或者当前这条路使用过魔法次数较少
                if (path[mv.fi][mv.se] == -1 || path[mv.fi][mv.se] > path[now.fi][now.se])
                {
                    // 普通移动，放到队首
                    a.push_front(mv);
                    // 普通移动不需要 +1
                    path[mv.fi][mv.se] = path[now.fi][now.se];
                }
            }
        }

        // 以当前方格为中心的 5x5
        rep(x, now.fi - 2, now.fi + 2)
        {
            rep(y, now.se - 2, now.se + 2)
            {
                if (x < 1 || x > h || y < 1 || y > w) // 越界检查
                    continue;

                if (!img[x][y])
                {
                    // 当前这条路还没走过
                    if (path[x][y] == -1)
                    {
                        // 魔法移动，放到队尾
                        a.push_back({x, y});
                        // 魔法移动记得 +1
                        path[x][y] = path[now.fi][now.se] + 1;
                    }
                }
            }
        }
    }
}

int main()
{
    ios::sync_with_stdio(false);

    cin >> h >> w;

    cin >> str.fi >> str.se;
    cin >> fin.fi >> fin.se;

    rep(i, 1, h)
    {
        rep(j, 1, w)
        {
            cin >> tmp;
            img[i][j] = (tmp == '#'); // 记 墙=1 路=0
        }
    }

    init();
    bfs();

    cout << ans << endl;

    return 0;
}
```
