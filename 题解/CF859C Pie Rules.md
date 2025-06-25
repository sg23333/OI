> 2024-12-18

# CF859C Pie Rules

### 题意

Alice 和 Bob 在长度为 $n$ 的序列 $a$ 中选数，Bob 先选，对于每次选数。

- 选了这个数，把这个数加给自己，下次对方选。
- 不选这个数，把这个数加给对方，下次自己选。

求 Alice 和 Bob 最后的分数。

### 思路

考虑 DP，$dp_{i}$ 表示在区间 $(i,n)$ 中通过最优选法的得分，$sum_i$ 表示后缀和。

- 如果当前不选这个数，分数不变，$dp_{i}=dp_{i+1}$。
- 如果当前选了这个数，$dp_{i}=sum_{i+1}-dp_{i+1}+a_i$，以 Bob 为例，Bob 选了第 $i$ 个数，加上 $a_i$，下一次让 Alice 选，大小为 $dp_{i+1}$，用 $sum_{i+1}-dp_{i+1}$ 就是 Bob 在 $(i+1,n)$ 得到的分数，Alice 同理。

化简得：

$$dp_i= \max (dp_{i+1},sum_i-dp_{i+1})$$

时间复杂度 $O(n)$。

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
int n;
int a[114],sum[114],dp[114]; 
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
    	cin>>a[i];
	}
	for(int i=n;i>=1;i--){
		sum[i]=sum[i+1]+a[i];
	}
	for(int i=n;i>=1;i--){
		dp[i]=max(dp[i+1],sum[i]-dp[i+1]);
	}
	cout<<sum[1]-dp[1]<<' '<<dp[1];
	return 0;
}
```

[提交记录](https://codeforces.com/contest/859/submission/297042732)