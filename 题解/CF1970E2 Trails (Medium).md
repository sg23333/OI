> 2025/6/25

## CF1970E2 Trails (Medium)

### 题意

有 $m$ 个小屋，每个小屋通过若干条短路径和长路径与湖边的集合点相连。哈利每天从一个小屋出发，先走一条路径到湖边，再走一条路径到另一个小屋（可是同一个小屋），但这两条路径中至少有一条必须是短路径。已知哈利从小屋 $1$ 出发，连续走 $n$ 天，问有多少种不同的路线，结果对 $10^9+7$ 取模。

### 数据范围

$m \le 100,n \le 10^9$。

### 思路

这个题对比[简化版](https://www.luogu.com.cn/problem/CF1970E1)的区别就是 $n$ 的大小从 $10^3$ 到了 $10^9$。

先想一下简化版的思路。

令 $t_i$ 是小屋 $i$ 到湖边的路径数量和（也就是 $s_i+l_i$）。

$f_{i,j}$ 表示第 $i$ 天到小屋 $j$ 的路线数，可以由 $f_{i-1,k}$ 转移而来（$1 \le k \le m$），所有的路径数为 $t_j \times t_k$，但是不能全部是短路径，所以要减去 $s_j \times s_k$，则转移方程如下：
$$
f_{i,j}= \sum_{k=1}^{m} f_{i-1,k}\times(t_j \times t_k-s_j \times s_k)
$$

发现系数 $t_j \times t_k-s_j \times s_k$ 是不变的，考虑矩阵加速。

我们可以一个矩阵 $A$：
$$
A_i=[f_{i,1},f_{i,2},\dots,f_{i,m}]
$$
另外有一个矩阵 $B$，满足：
$$
A_{i+1}=A_i \times B
$$
可以推出 $B$：
$$
B=\left[
\begin{array}{cccc}
t_1 t_1 - l_1 l_1 & t_1 t_2 - l_1 l_2 & \cdots & t_1 t_m - l_1 l_m \\
t_2 t_1 - l_2 l_1 & t_2 t_2 - l_2 l_2 & \cdots & t_2 t_m - l_2 l_m \\
\vdots & \vdots & \ddots & \vdots \\
t_m t_1 - l_m l_1 & t_m t_2 - l_m l_2 & \cdots & t_m t_m - l_m l_m \\
\end{array}
\right]
$$
最后 $A_n=A_0 \times B^n$，答案就是 $\sum_{i=1}^{m} f_{n,i}$。

### 代码

```cpp
//矩阵的两维和题解中是相反的
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
const int MOD=1e9+7;
int n,m;
int s[105],l[105];
int t[105];

struct Matrix{
    int m[105][105],n;
    void init(int size){
        n=size;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                m[i][j]=(i==j);
            }
        }
    }
    Matrix operator*(const Matrix &b)const{
        Matrix res;
        res.n=n;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                res.m[i][j]=0;
                for(int k=1;k<=n;k++){
                    res.m[i][j]+=m[i][k]*b.m[k][j];
                    res.m[i][j]%=MOD;
                }
            }
        }
        return res;
    }
};
Matrix matrix_qpow(Matrix base,int exp){
    Matrix res;
    res.init(base.n);
    while(exp>0){
        if(exp&1){
            res=res*base;
        }
        base=base*base;
        exp>>=1;
    }
    return res;
}
int ans;
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>m>>n;
    for(int i=1;i<=m;i++){
        cin>>s[i];
    }
    for(int i=1;i<=m;i++){
        cin>>l[i];
        t[i]=s[i]+l[i];
    }
    Matrix A,B;
    A.n=B.n=m;
    B.m[1][1]=1;
    for(int i=1;i<=m;i++){
        for(int j=1;j<=m;j++){
            A.m[i][j]=t[i]*t[j]-l[i]*l[j];
            A.m[i][j]%=MOD;
        }
    }
    B=B*matrix_qpow(A,n);
    for(int i=1;i<=m;i++){
        ans+=B.m[1][i];
        ans%=MOD;
    }
    cout<<ans;
    return 0;
}
```

[提交记录](https://codeforces.com/contest/1970/submission/325995806)