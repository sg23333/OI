> 2025-01-10

# Luogu P11545 [Code+#5] 割集确定

### 题意

给定一个 $n$ 个城市的网络，城市之间有 $m$ 条双向通道。每个城市都有情报中心，当一个城市的情报中心被歼灭后，与其相连的正常城市会报告"无法发送信息"。现在给出 $q$ 次询问，每次给出若干条错误信息，要求确定被歼灭的情报中心数量。

### 思路

如果收到"城市 $x$ 无法给城市 $y$ 发送信息"的错误报告，说明 $x$ 是正常的（能发出报告），$y$ 是被歼灭的（导致无法发送）。

将原图转换成树。

先用 DFS 预处理出每个点 $i$ 的父节点 $fa_i$ 和子树大小 $Size_i$。

对于每条错误信息（$x$ 无法给 $y$ 发送）：

- 如果 $x$ 是 $y$ 的儿子（$fa_x=y$）：$ans$ 减去 $Size_x$（$x$ 正常，其子树都正常）。
- 如果 $y$ 是 $x$ 的儿子（$fa_y=x$）：$ans$ 加上 $Size_y$（$y$ 被歼灭，其子树都被歼灭）。

这样答案有时候会是负数（实际上是计算的正常的点的数量）。

举个例子：

![](https://cdn.luogu.com.cn/upload/image_hosting/a6etfghb.png)

有 $2 \to 1$ 和 $3 \to 1$ 两条错误。

这时候 $ans=-3-2=-5$，但是实际上只有 $1$ 被摧毁了，所以当 $ans<0$ 时，$ans$ 要加上 $n$。

时间复杂度 $O(m+\sum_{i=1}^q c_i)$。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m,q,ans;
struct lyl{
	int u,v;
}qu[3000010]; 
vector<int> g[3000010];
int Size[3000010],fa[3000010];
void dfs(int u){
	Size[u]=1;
	for(auto v:g[u]){
		if(!fa[v]){
			fa[v]=u; 
			dfs(v);
			Size[u]+=Size[v];
		}
	}
}
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m;
	for(int i=1;i<=m;i++){
		cin>>qu[i].u>>qu[i].v;
		g[qu[i].u].push_back(qu[i].v);
		g[qu[i].v].push_back(qu[i].u);
	}
	fa[1]=-1;
	dfs(1);
	cin>>q;
	while(q--){
		ans=0;
		int c;
		cin>>c;
		while(c--){
			int op,u,v;
			cin>>op;
			if(op>0){
				u=qu[op].u;
				v=qu[op].v;
			}
			else{
				u=qu[-op].v;
				v=qu[-op].u;
			}
			if(fa[u]==v){
				ans-=Size[u];
			}
			if(fa[v]==u){
				ans+=Size[v];
			}	
		}
		if(ans<0){
			ans+=n;
		}
		cout<<ans<<'\n';
	}
	return 0;
}
```