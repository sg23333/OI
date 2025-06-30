## CF1932F Feed Cats

### 题意

你在玩一个游戏，这个游戏有 $n$ 步。你有 $m$ 只猫，每只猫有特定的饲养时间 $[l_i,r_i]$。如果你在第 $x$ 步决定饲养，那么所有满足 $l_i\le x\le r_i$ 的猫都会被饲养；或者你不决定饲养，那么无事发生。但是如果一只猫被饲养了两次及以上，它就会死亡。请问在没有猫死亡的情况下，最多有多少只猫被饲养了至少一次？

### 题解

考虑 DP。

$f_i$ 表示从 $1$ 遍历到 $i$ 可以饲养的猫的个数。

令 $l_i$ 为覆盖 $i$ 的区间最靠左的左端点，$sum_i$ 为区间 $[l_i,i]$ 交区间的个数，则：
$$
f_i=\max(f_{i-1},f_{l_i-1}+sum_i)
$$

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int t;
int n,m;
struct lyl{
    int l,r;
}e[100010];
int f[100010];
bool operator<(lyl a,lyl b){
    if(a.l==b.l){
        return a.r>b.r;
    }
    else{
        return a.l<b.l;
    }
}
int sum[100010];
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>t;
    while(t--){
        cin>>n>>m;
        for(int i=1;i<=n;i++){
            sum[i]=0;
        }
        f[0]=0;
        for(int i=1;i<=m;i++){
            cin>>e[i].l>>e[i].r;
            sum[e[i].l]++;
            sum[e[i].r+1]--; 
        }
        sort(e+1,e+1+m);
        for(int i=1;i<=n;i++){
            sum[i]+=sum[i-1];
        }
        int pos=0;
        for(int i=1;i<=n;i++){
            f[i]=f[i-1];
            while(i>e[pos].r){
                pos++;
            }
            if(i<e[pos].l){
                continue;
            }
            else{
                f[i]=max(f[i],f[e[pos].l-1]+sum[i]);
            }
        }
        cout<<f[n]<<'\n';
    }
    return 0;
}
```

[提交记录](https://codeforces.com/contest/1932/submission/326597207)