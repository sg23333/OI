> 2025-01-16

## Luogu P1073 [NOIP2009 提高组] 最优贸易

upd $2025/1/17$：修改错别字
### 题意概括

给你一个长度为 $n$ 的序列 $a_n$，代表权值。

从节点 $1$ 到节点 $n$ 有若干条路径，路径上任意取两个点 $x$ 和 $y$，满足：

- $x$ 比 $y$ 先访问。
- $a_y-a_x$ 尽可能地大。 

 求最大的 $a_y-a_x$。

### 思路

考虑分层图。

什么是分层图？

> 分层图是一种在图论中用于解决特定问题的建模方法。它通过将原始图复制成多层结构，每一层代表不同的状态或决策次数，从而在这些层之间建立特定的连接，以满足问题的约束条件。

就以这个题举例。

我们需要进行**买入**和**卖出**两次操作，当买了在第 $i$ 个城市买了水晶球时，代价是 $-a_i$，卖出的代价是 $a_i$，不买不卖就是 $0$。

我们可以建三层图，第一层代表没买也没卖，第二层代表买了但没卖，第三层代表买了也卖了。

如图：

![graph _5_-topaz-upscale-4x.png](https://s2.loli.net/2025/01/13/kqiUHstVyWcPAev.png)

在这个图上跑最长路就好了。

#### 关于建图：

对于第 $i$ 个点，对 $i$ 和 $i+n$ 连一条边权为 $-a_i$ 的边，$i+n$ 和 $i+2 \times n$ 连一条边权为 $a_i$ 的边。($1$)

对于每一对 $u,v$，$u$ 和 $v$ 连一条边权 $0$ 的边，$u+n$ 和 $v+n$ 连一条边权 $0$ 的边，$u+2\times n$ 和 $v+2\times n$ 连一条边权 $0$ 的边。($2$)

（上面的图都能体现）。

**参考代码：**

```c++
for(int i=1;i<=n;i++){
	cin>>w[i];
	g[i].push_back({i+n,-w[i]});
	g[i+n].push_back({i+2*n,w[i]});
}//第一种情况
g[u].push_back({v,0});
g[u+n].push_back({v+n,0});
g[u+2*n].push_back({v+2*n,0});
//第二种情况
```

时间复杂度：$O(3nm)$。

### Code

```c++
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
struct lyl{
	int v,w;
};
vector<lyl> g[500010];
int w[500010],vis[500010],dis[500010];
queue<int> q;
void spfa(int s){
	q.push(s);
	dis[s]=0;
	vis[s]=1;
	while(!q.empty()){
		int u=q.front();
		q.pop();
		vis[u]=0;
		for(auto [v,w]:g[u]){
			if(dis[v]<dis[u]+w){
				dis[v]=dis[u]+w;
				if(!vis[v]){
					vis[v]=1;
					q.push(v);
				}
			}
		}
	}
	return ;
}
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m;
	for(int i=1;i<=n;i++){
		cin>>w[i];
		g[i].push_back({i+n,-w[i]});
		g[i+n].push_back({i+2*n,w[i]});
	}
	for(int i=1;i<=m;i++){
		int u,v,op;
		cin>>u>>v>>op;
		if(op==1){
			g[u].push_back({v,0});
			g[u+n].push_back({v+n,0});
			g[u+2*n].push_back({v+2*n,0});
		}
		else{
			g[u].push_back({v,0});
			g[u+n].push_back({v+n,0});
			g[u+2*n].push_back({v+2*n,0});
			g[v].push_back({u,0});
			g[v+n].push_back({u+n,0});
			g[v+2*n].push_back({u+2*n,0});
		}
	}
	memset(dis,128,sizeof dis);
	spfa(1);
	cout<<max(dis[n],dis[3*n]);
	return 0;
}

```