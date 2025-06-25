> 2025/4/16

# SP32079 ADAGF - Ada and Greenflies

### 题意

给定一个序列 $a$，求所有子区间的最大公约数之和，即：
$$
\sum_{1 \le i \le j \le n} \gcd(a_i, a_{i+1}, \ldots, a_j)
$$

### 思路

我们采用**分治**的方法来解决这个问题。

设当前处理的区间为 $[l, r]$，可以将其划分为 $[l,mid]$ 和 $[mid+1, r]$，那么子区间可以被分为三类：

1. 完全在左侧，即被 $[l, mid]$ 包含。
2. 完全在右侧，即被 $[mid+1, r]$ 包含。
3. 横跨中点 $mid$。

前两类可以递归求解，我们重点处理第三类，也就是横跨中点的区间，形式为：
$$
\sum_{i=l}^{mid} \sum_{j=mid+1}^{r} \gcd(a_i, a_{i+1}, \ldots, a_j)
$$
考虑以 $[i,mid]$ 为左段（从 $mid$ 向左扫描），定义 $g_i = \gcd(a_i, a_{i+1}, \ldots, a_{mid})$。显然可以递推得到：$g_i = \gcd(g_{i+1}, a_i)$。

如果连续两个位置的 $g_i$ 相同，那么这两个区间对右侧区间的贡献也是一样的。为了避免重复计算，我们将相同的 $g_i$ 用一个 `pair` 存储，`first` 表示当前的 $\gcd$ 值，`second` 表示这个 $\gcd$ 值连续出现的次数。

注意到 $\gcd$ 每次要么不变，要么至少减半，因此不同的 $\gcd$ 值最多只有 $O(\log a_i)$ 种。

右边区间也可以以相同方式处理，接着我们就可以枚举左侧每种 $\gcd$ 与右侧每种 $\gcd$ 的组合，贡献为：
$$
\gcd⁡(L.\text{first},R.\text{first}) \times L.\text{second} \times R.\text{second}
$$
最后把三部分加起来即可。

### 时间复杂度分析

- 分治复杂度是 $O(\log n)$；
- 每一层中，左右两侧扫描 $O(n)$，每个位置最多生成 $\log a_i$ 个不同 $\gcd$；
- 枚举左右组合为 $O(\log^2 n)$ 。

所以总时间复杂度为：$O(n \log n+\log^3 n)$。

可以通过此题。

### Code

```c++
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n;
int a[300010];
int solve(int l,int r){
    if(l==r){
        return a[l];
    }
    int m=(l+r)/2;
    int res=solve(l,m)+solve(m+1,r);
    vector<pair<int,int>>L,R;
    int now=0;
    for(int i=m;i>=l;i--){
        now=__gcd(now,a[i]);
        if(L.empty()||L.back().first!=now){
            L.push_back({now,1});
        }
        else{
            L.back().second++;
        }
    }
    now=0;
    for(int i=m+1;i<=r;i++){
        now=__gcd(now,a[i]);
        if(R.empty()||R.back().first!=now){
            R.push_back({now,1});
        }
        else{
            R.back().second++;
        }
    }
    for(auto[g1,c1]:L){
        for(auto[g2,c2]:R){
            res+=__gcd(g1,g2)*c1*c2;
        }
    }
    return res;
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    cout<<solve(1,n)<<'\n';
    return 0;
}
```

[提交记录](https://www.luogu.com.cn/record/213804172)