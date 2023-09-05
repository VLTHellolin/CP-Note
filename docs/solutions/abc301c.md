---
comments: true
---

# ABC301C AtCoder Cards 题解

[洛谷题面](https://www.luogu.com.cn/problem/AT_abc301_c) | [AtCoder 题面](https://atcoder.jp/contests/abc301/tasks/abc301_c) | [AC 记录](https://www.luogu.com.cn/record/110510119) | $\texttt{\color{968d81}*380}$

首先，我们搞出来 $S$ 和 $T$ 中各种字符的数量（和 `@` 的数量）。注意也要同时搞出来字符集。

``` cpp
string s,t;
int sa,ta;
map<char,int>ss,tt;
set<char>al;

for(char i:s){
    if(i=='@')++sa;
    else{
        ++ss[i];
        al.insert(i);
    }
}
for(char i:t){
    if(i=='@')++ta;
    else{
        ++tt[i];
        al.insert(i);
    }
}
```

再枚举字符集中的字符，若数量不同则考虑花费一定量的 `@` 来补成一样的。

``` cpp
for(char i:al){
    int sc=ss[i], tc=tt[i];
    if(sc<tc){
        if(qwq.find(i)==-1){
            cout<<"No"<<endl;
            return;
        }
        else sa-=tc-sc;
    }
    if(sc>tc){
        if(qwq.find(i)==-1){
            cout<<"No"<<endl;
            return;
        }
        else ta-=sc-tc;
    }
}
```

完整代码如下：

``` cpp
// 珍爱账号，请勿贺题
#include <bits/stdc++.h>
using namespace std;
#define int long long

string s,t;
int sa,ta;
map<char,int>ss,tt;
set<char>al;
string atc="atcoder";
void solve(){
    cin>>s>>t;

    for(char i:s){
        if(i=='@')++sa;
        else{
            ++ss[i];
            al.insert(i);
        }
    }
    for(char i:t){
        if(i=='@')++ta;
        else{
            ++tt[i];
            al.insert(i);
        }
    }

    for(char i:al){
        int sc=ss[i], tc=tt[i];
        if(sc<tc){
            if(atc.find(i)==-1){
                cout<<"No"<<endl;
                return;
            }
            else sa-=tc-sc;
        }
        if(sc>tc){
            if(atc.find(i)==-1){
                cout<<"No"<<endl;
                return;
            }
            else ta-=sc-tc;
        }
    }

    cout<<((sa==ta&&sa>=0)?"Yes":"No")<<endl;
}

int32_t main(){
    ios::sync_with_stdio(0);
    cin.tie(0);

    solve();

    return 0;
}
```