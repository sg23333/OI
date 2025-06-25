> 2024-12-29

## Luogu B4100 [CSP-X2023 山东] 赚钱

### 题意

给你一个长度为 $n$ 序列 $a$，对任意 $1 \le j < i \le n$，求 $a_i-a_j$ 的最大值。

### 思路

以为 $n \le 2 \times 10^5$，显然暴力 $O(n^2)$ 会爆，考虑优化。

贪心地想，对于每个 $a_i$，它的最大收益只跟它前面的最小值有关，可以在遍历时找到维护前面的最小值 $minn$，每次对 $a_i-minn$ 取最大值即可。

注意到答案可能小于 $0$，所以 $ans$ 要赋初值为极小。

时间复杂度：$O(n)$。

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
const int INF=1e18;
int ans=-INF,mi=INF;
int n,a[200010];
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    for(int i=1;i<=n;i++){
        ans=max(ans,a[i]-mi);//维护答案
        mi=min(mi,a[i]);//维护最小值
    }
    cout<<ans<<'\n';
    return 0;
}
```