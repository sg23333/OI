## CF1970E1 Trails (Easy)

### 题意

有 $m$ 个小屋，每个小屋通过若干条短路径和长路径与湖边的集合点相连。哈利每天从一个小屋出发，先走一条路径到湖边，再走一条路径到另一个小屋（可是同一个小屋），但这两条路径中至少有一条必须是短路径。已知哈利从小屋 $1$ 出发，连续走 $n$ 天，问有多少种不同的路线，结果对 $10^9+7$ 取模。

### 数据范围

$m \le 100,n \le 10^3$。

### 思路

令 $t_i$ 是小屋 $i$ 到湖边的路径数量和（也就是 $s_i+l_i$）。

考虑 DP。

$f_{i,j}$ 表示第 $i$ 天到小屋 $j$ 的路线数，可以由 $f_{i-1,k}$ 转移而来（$1 \le k \le m$），所有的路径数为 $t_j \times t_k$，但是不能全部是短路径，所以要减去 $s_j \times s_k$，则转移方程如下：
$$
f_{i,j}= \sum_{k=1}^{m} f_{i-1,k}\times(t_j \times t_k-s_j \times s_k)
$$

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
const int mod=1e9+7;
int n,m;
int f[1010][110];
int t[110];
int s[110],l[110];
int ans;
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>m>>n;//m,n是反的
    for(int i=1;i<=m;i++){
        cin>>s[i];
    }
    for(int i=1;i<=m;i++){
        cin>>l[i];
        t[i]=l[i]+s[i];
    }
    f[0][1]=1;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            int cnt=0;
            for(int k=1;k<=m;k++){
                cnt+=(t[j]*t[k]-l[j]*l[k])*f[i-1][k];
                cnt%=mod;
            }
            f[i][j]=cnt;
        }
    }
    for(int i=1;i<=m;i++){
        ans+=f[n][i];
        ans%=mod;
    }
    cout<<ans<<'\n';
    return 0;
}
```

[提交记录](https://codeforces.com/contest/1970/submission/325999999)
