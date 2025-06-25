> 2025-01-10

## Luogu P1967 [NOIP2013 提高组] 货车运输

### 题意

给你一片森林，求森林中两点的路径中，最小边权的最大值，若两点不连通，输出 $-1$。

### 思路

先假设点 $x$ 和 $y$ 连通（$x$ 和 $y$ 在一个子图上）。

为了让路径上的最小边权最大，则这个路径一定在两点所在树的最大生成树（MST）上，让我们来利用反证法简要证明一下。

假设有两个点 $s$ 和 $t$ 之间在最大生成树上的路径的最小边权为 $E$，如果存在另外一条路径使得路径上的最小路径 $E'>E$，那么在一开始用 Kruskal 求最大生成树的时候，$E'$ 就被选了，所以最优路径一定在最大生成树上。

我们现在只保留最大生成树。

$x$ 到 $y$ 的路径为 $x \to \text{LCA}(x,y) \to y$。

LCA 可以用倍增求。

但是如果暴力求路径上的最小值，当树是一条链的时候会被卡到 $O(n)$，显然会寄。

考虑优化。

开一个数组 $dis$，$dis_{i,j}$ 表示 $i$ 的第 $2^j$ 个祖宗和 $i$ 之间的最小边权。

当枚举到 $i$ 的第 $2^j$ 个祖宗（下设为 $k$）时：

$$dis_{i,j}=\min(dis_{i,j-1},dis_{k,j-1})$$

跟 LCA 求 $fa$ 很像啊，那就一起维护吧。

但是还没完。

一开始我们假设：

> 先假设点 $x$ 和 $y$ 连通（$x$ 和 $y$ 在一个子图上）。

那么如果两个点不连通呢？

用一个并查集维护每个点所在的连通块就行了。

时间复杂度 $O(n \log n)$。

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
const int INF=1e18;
int n,m,q;
int ans;
struct node{
	int v,w;
};
struct lyl{
	int u,v,w;
}a[50010];
bool operator <(lyl x,lyl y){
	return x.w>y.w;
}
vector<node> g[50010];
int faa[50010];//维护并查集的，不是维护LCA
int vis[50010];
int dis[50010][55];
int depth[50010],fa[50010][55],lg[50010];
void dfs(int now,int fath,int d){
	vis[now]=1;
	dis[now][0]=d;
	fa[now][0]=fath;
	depth[now]=depth[fath]+1;
	for(int i=1;i<=lg[depth[now]];i++){
		fa[now][i]=fa[fa[now][i-1]][i-1];
		dis[now][i]=min(dis[now][i-1],dis[fa[now][i-1]][i-1]);
	}
	for(auto [v,w]:g[now]){
		if(v!=fath){
			dfs(v,now,w);
		}
	}
	return ;
}
int LCA(int x,int y){
	ans=INF;
	if(depth[x]<depth[y]){
		swap(x,y);
	}
	while(depth[x]>depth[y]){
		ans=min(ans,dis[x][lg[depth[x]-depth[y]]-1]);
		x=fa[x][lg[depth[x]-depth[y]]-1];
	}	
	if(x==y){
		return ans;
	}
	for(int k=lg[depth[x]]-1;k>=0;k--){
		if(fa[x][k]!=fa[y][k]){
			ans=min(ans,min(dis[x][k],dis[y][k]));
			x=fa[x][k],y=fa[y][k];
		}
	}
	ans=min(ans,min(dis[x][0],dis[y][0]));
	return ans;
}
int Find(int x){
	if(faa[x]==x){
		return x;
	}
	else{
		return faa[x]=Find(faa[x]);
	}
}
void add(int u,int v){
	faa[u]=v;
}
int cnt;
void ku(){
	for(int i=1;i<=m;i++){
		if(Find(a[i].u)!=Find(a[i].v)){
			add(Find(a[i].u),Find(a[i].v));
			g[a[i].u].push_back({a[i].v,a[i].w});
			g[a[i].v].push_back({a[i].u,a[i].w});
			cnt++;
		}
		if(cnt==n-1){
			break;
		}
	}
	return ;
}
signed main(){
//	freopen("input.txt","r",stdin);
	
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n;i++){
    	faa[i]=i;
	}
	for(int i=1;i<=n;i++){
		lg[i]=lg[i-1]+(1<<lg[i-1]==i);
	}
    for(int i=1;i<=m;i++){
    	cin>>a[i].u>>a[i].v>>a[i].w;
	}
	sort(a+1,a+1+m);
	ku();
//	bug
	for(int i=1;i<=n;i++){
		if(!vis[i]){
			dfs(i,0,0); 
		}
	}
	
	cin>>q;
	while(q--){
		int s,t;
		cin>>s>>t;
		if(Find(s)!=Find(t)){
			cout<<-1<<'\n';
		}
		else{
			cout<<LCA(s,t)<<'\n';
		}
	}
	return 0;
}
```