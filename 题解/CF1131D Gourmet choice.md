## CF1131D Gourmet choice

### 题意

构造序列 $a,b$，满足每个 $a_i,b_j$ 之间的大小关系。

### 题解

考虑拓扑排序，把小的向大的连边，如果相等就用并查集并到一起。

答案记录在 `ans` 数组里。

从最小的开始（入度为 $0$），让 $ans_i=1$，之后每一对 $u \to v$，$ans_v=ans_u+1$，因为一个点要同时满足多个条件，所以要取一个最大值。

记录一个 `vis` 数组，把所有的遍历到的点记录下来，最后如果有点没有被遍历过就输出 No。

否则输出答案。

要注意因为用并查集合并了相等的点，所以实现的时候都要用并查集里的编号。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
vector<int> g[2111];
int fa[2111];
int vis[2111];
int in[2111];
char c[1111][1111];
int ans[2111];
int Find(int x){
    return fa[x]==x ? x :Find(fa[x]);
}
void merge(int x,int y){
    fa[Find(x)]=Find(y);
}
void topu(){
    queue<int> q;
    for(int i=1;i<=n+m;i++){
        if(fa[i]==i&&!in[i]){
            q.push(i);
            ans[i]=1;
			vis[i]=1;
        }
    }
    while(!q.empty()){
        int u=q.front();
        q.pop();
        for(auto v:g[u]){
			in[v]--;
            ans[v]=max(ans[v],ans[u]+1);
            if(!in[v]){
                vis[v]=1;
                q.push(v);
            }
        }
    }
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n+m;i++){
        fa[i]=i;
    }
    for(int i=1;i<=n;i++){
    	for(int j=1;j<=m;j++){
    		cin>>c[i][j];
		}
	}
	for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++){
			if(c[i][j]=='='){
				merge(i,n+j);
			}
		}
	}
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
        	char k=c[i][j];
            if(k=='<'){
            	in[Find(n+j)]++;
                g[Find(i)].push_back(Find(n+j));
            }
            else if(k=='>'){
            	in[Find(i)]++;
                g[Find(n+j)].push_back(Find(i));
            }
        }
    }
    topu();
    for(int i=1;i<=n+m;i++){
        if(!vis[Find(i)]){
            cout<<"No"<<'\n';
            return 0;
        }
    }
    cout<<"Yes"<<'\n';
    for(int i=1;i<=n;i++){
        cout<<ans[Find(i)]<<' ';
    }
    cout<<'\n';
    for(int i=1;i<=m;i++){
        cout<<ans[Find(n+i)]<<' ';
    }
    return 0;
}
```

