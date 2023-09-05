---
comments: true
---

# ABC302C Almost Equal 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc302_c) | [AtCoder 题面](https://atcoder.jp/contests/abc302/tasks/abc302_c) | [AC 记录](https://www.luogu.com.cn/record/111041076) | $\texttt{\color{f2b373}*469}$

要求全排列，我们可以用标准库函数 `prev_permutation`（求前一个）和 `next_permutation`（求后一个），接受参数 `first` 和 `last`，也可以带比较函数 `comp`。

这两个函数会返回将 `[first, last)` 范围内所有以 `<`（或 `comp`）比较的字典序的上/下一个排列，若存在则返回 `true` 并变换排列，若已是第一个/最后一个排列，返回 `false`，并变换排列为最后一个/第一个排列。

``` cpp
vector<int>a;
...
// 若要使用 next_permutation，先保证这个范围是第一个排列（也就是升序）
sort(a.begin(),a.end());
// 要使用 do while 循环，因为第一个排列也是排列
do{
    ...
}while(next_permutation(a.begin(),a.end()));
```

判断是否成立是 $O(NM)$ 的复杂度，所以总结下来时间复杂度为 $O(NM\cdot N!)$，可以通过本题。

（题解文本部分参考了 [cppreference](https://zh.cppreference.com/w/cpp/algorithm/next_permutation)）

``` cpp
// 珍爱账号，拒绝贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

vector<string>s;
int n,m;
bool f;
bool judge(){
    for(int i=0;i<s.size()-1;i++){
        int p=0;
        for(int j=0;j<s[i].size();j++){
            p+=s[i][j]!=s[i+1][j];
        }
        if(p!=1)return 0;
    }
    return 1;
}
void solve(){
    cin>>n>>m;
    s.resize(n);
    for(string&i:s)cin>>i;
    sort(s.begin(),s.end());
    do{
        if(judge()){
            cout<<"Yes"<<endl;
            return;
        }
    }while(next_permutation(s.begin(),s.end()));
    cout<<"No"<<endl;
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```
