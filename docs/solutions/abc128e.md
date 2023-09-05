---
comments: true
---

# ABC218E Akari 题解

!!! warning "年代久远"

    您正在查看一篇年代久远的题解，由于 Markdown 格式不兼容等问题，页面渲染可能会出现问题。其中的时效性信息可能不再有效，请仔细辨别。

[题面(洛谷)](https://www.luogu.com.cn/problem/AT_abc182_e)

[题面(AtCoder)](https://atcoder.jp/contests/abc182/tasks/abc182_e)

## 思路

拿到这题的第一个想法是打暴力模拟，对每个灯泡都上下左右扫一遍，直至障碍物或者边界，最后得出答案。但时间复杂度是 $O((H+W)\cdot N)$ 级别的，一定会 TLE。

考虑一个简单的优化，既然灯泡只能向上下左右发出光亮，那干脆沿着灯泡发出光亮的方向扫就可以了。

一次扫一个方向，四次扫四个方向，遇到灯泡就准备标记，遇到障碍物就准备停止标记，很好地避免了单独计算灯泡浪费时间的问题。如果不考虑读入，时间复杂度 $O(HW)$。

## 代码

``` cpp
// Coder          : Hellolin
// Time           : 2023-01-25 19:40:17
// Problem        : [ABC182E] Akari
// Contest        : Luogu
// URL            : https://www.luogu.com.cn/problem/AT_abc182_e
// Time Limit     : 2500 ms
// Memory Limit   : 1 GiB

#include <iostream>

const int MAX=1900;
// h, w, n 和 m 意义如题，p 数组存放网格图，x 和 y 用作读入灯泡/障碍物坐标
int h, w, n, m, p[MAX][MAX], x, y, ans;
// z 数组存放每个方格灯光照射情况
bool z[MAX][MAX], f;
int main()
{
    // 加速输入输出
    std::ios::sync_with_stdio(false);

    std::cin>>h>>w>>n>>m;
    
    for(int i=1; i<=n; i++)
    {
        std::cin>>x>>y;
        p[x][y]=1;
    }
    for(int i=1; i<=m; i++)
    {
        std::cin>>x>>y;
        p[x][y]=-1;
    }
    
    // 对于每一行，向右照射灯光
    for(int i=1; i<=h; i++)
    {
        f=0; // 初始没有光
        for(int j=1; j<=w; j++)
        {
            if(p[i][j]==1) f=1; // 有灯泡
            else if(p[i][j]==-1) f=0; // 有障碍物
            
            if(f&&(!z[i][j])) // 可以被光照到但未标记
            {
                z[i][j]=1; // 标记
                ++ans;
            }
        }
    }
    // 对于每一行，向左照射灯光
    for(int i=1; i<=h; i++)
    {
        f=0;
        for(int j=w; j>=1; j--)
        {
            if(p[i][j]==1) f=1;
            else if(p[i][j]==-1) f=0;
            
            if(f&&(!z[i][j]))
            {
                z[i][j]=1;
                ++ans;
            }
        }
    }
    // 对于每一列，向下照射灯光
    for(int j=w; j>=1; j--)
    {
        f=0;
        for(int i=1; i<=h; i++)
        {
            if(p[i][j]==1) f=1;
            else if(p[i][j]==-1) f=0;
            
            if(f&&(!z[i][j]))
            {
                z[i][j]=1;
                ++ans;
            }
        }
    }
    // 对于每一列，向上照射灯光
    for(int j=w; j>=1; j--)
    {
        f=0;
        for(int i=h; i>=1; i--)
        {
            if(p[i][j]==1) f=1;
            else if(p[i][j]==-1) f=0;
            
            if(f&&(!z[i][j]))
            {
                z[i][j]=1;
                ++ans;
            }
        }
    }
    
    // 输出答案
    std::cout<<ans<<std::endl;
    
    return 0;
}

```
