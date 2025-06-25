> 2024-10-15

## CF1526C1 Potions (Easy Version)

### 前置知识：01 背包

给定 $n$ 种物品和一背包。物品 $i$ 的重量是 $w_i$，价值是 $v_i$，背包的容量为 $c$。  
对于每种物品 $i$ 只有两种选择，装入或者不装入。  
则第 $i$ 个物品，背包的重量为 $j$ 时，可以得到状态转移方程：
$$DP_{i,j}=\max (DP_{i-1,j},DP_{i-1,j-w_i}+v_i)$$

### 题意

给你一个长度为 $n$ 的序列 $A$，要求你找出最长的一个**子序列**使得这个子序列任意前缀和都非负。

### 思路
观察到数据范围 $n \le 2000$，并且对于每瓶药水，只有**喝**和**不喝**两种选择，可以想到用 **01 背包**来做，$j$ 表示喝了 $j$ 瓶药，$DP_j$表示喝了 $j$ 瓶药后的健康值，其中：
$$DP_j=\max(DP_{j-1}+a_i,DP_j)$$ 
最后从后往前遍历，当遇到第一个大于 $0$ 的 $DP_i$，则 $i$ 是可以喝的最大的瓶数。
### 关键代码
```
for(int i=n;i>=0;i--){
	if(dp[i]>=0){
        printf("%d",i);
        break;
    } 
}
```
- 外层循环遍历每一个药水。  
- 内层循环从 $i$ 向下遍历，确保不会覆盖当前的状态。  
- 如果饮用当前药水后健康值仍然非负，则更新 $DP_j$。

### Code

```
#include<bits/stdc++.h>
#define bug cout<<"!!!!!!!!!!!!!!!!!!!!!!!"<<endl;
#define int long long
using namespace std;
const long long INF=-2e15;
long long n,a[2222],dp[2222];
signed main(){
	scanf("%lld",&n);
	for(int i=1;i<=n;i++){
		scanf("%lld",&a[i]); 
		dp[i]=INF;
	}
	for(int i=1;i<=n;i++){
		for(int j=i;j>0;j--){
			if(dp[j-1]+a[i]<0){
				continue;
			}
			
			dp[j]=max(dp[j-1]+a[i],dp[j]);
		}
	}
	for(int i=n;i>=0;i--){
		if(dp[i]>=0){
			printf("%d",i);
			break;
		} 
	}
	return 0;
}
```

[提交记录](https://codeforces.com/problemset/submission/1526/286213191)