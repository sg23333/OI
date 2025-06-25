> 2025/03/03

## Luogu P6082 [JSOI2015] salesman

### 题意

给你一颗树，根节点是 $1$，每个节点有一个最大访问次数 $num_i$ 和权值 $val_i$（$num_1=INF$），每个点的权值只记录一次，求从根结点出发，最大的权值和。

### 思路

有一棵树，如图：

![graph _8_.png](https://s2.loli.net/2025/03/03/6rfYca3TpN5JvwW.png)

考虑点 $3$，想要让点 $3$ 访问所有的子节点，$3$ 至少要访问 $4$ 次。

$2\to 3$（$1$次）。

$3\to 5,5\to 3$（$2$次）。

$3\to 4,4\to 3$（$3$次）。

$3\to 7,7\to 3$（$4$次）。

显然，从点 $i$ 到 $i$ 所有的子节点需要 $cnt_i+1$ 次访问机会（$cnt_i$ 指 $i$ 的子节点的个数）。

那么如果一个点到不了它所有的子节点，那么就只去收益最大的点。

怎么求受益最大的点呢？

考虑树形 DP。

$dp_u$ 表示 $u$ 点的最大收益，$dp_u=a_u+\sum_{i=1}^{\min({cnt_u},{num_u})} dp_v$。

可以先求出所有的 $dp_v$，再从大到小排序，使用优先队列即可。

题目还要判断路径是否唯一，考虑 $2$ 种情况：

- 如果可以选的点中如果一个点的权值是 $0$，那么对答案就没有影响，答案不唯一。
- 如果选的最小的一个点有 $2$ 个，答案也不唯一。

注意 $dp_i<0$ 的点不能选。

时间复杂度 $O(n \log n)$。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n;
int dp[100010];
int val[100010],num[100010];
vector<int> g[100010],p[100010];
bool cmp(int x,int y){
	return x>y;
}
int flag;
void dfs(int u,int fa){
	dp[u]=val[u];
	priority_queue<pair<int,int>> q;
	for(auto v:g[u]){
		if(v==fa){
			continue;
		}
		dfs(v,u);
		q.push({dp[v],v}); 
	}
	int las=0;
	for(int i=1;i<num[u];i++){
		if(q.empty()){
			break;
		}
		auto [now,j]=q.top();
		q.pop();
		if(now<0){
			break;
		}
		dp[u]+=now;
		las=now;
		if(now==0){
			flag=1;
		}
	} 
	if(!q.empty()&&q.top().first==las){
		flag=1;
	}
}
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	for(int i=1;i<n;i++){
		cin>>val[i+1];
	}
	num[1]=100000000000000000000;
	for(int i=1;i<n;i++){
		cin>>num[i+1];
	}
	for(int i=1;i<n;i++){
		int u,v;
		cin>>u>>v;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	dfs(1,0);
	cout<<dp[1]<<'\n';
	if(flag){
		cout<<"solution is not unique";
	}
	else{
		cout<<"solution is unique";
	}
	return 0;
}
```