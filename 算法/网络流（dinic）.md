## 网络流（dinic）

### 前言

$EK$ 算法是一种网络流算法，时间复杂度是 $O(nm^2)$，可以解决边数为 $10^3$ 左右的网络流问题，但是当边数增大的时候（达到 $10^4$ 级别），就只能使用 $Dinic$ 算法了。

### Dinic

$EK$ 每次只能遍历一条增广路，会浪费很多的时间，所以 $Dinic$ 解决了这个问题，每次可以找出多条增广路。

### 实现

$Dinic$ 通过构建**分层图**来加快增广路径的查找过程。具体来说，我们使用 BFS 为残量网络构建一个分层图，然后在这个分层图上用 DFS 寻找尽可能多的增广路，直到无法找到新的增广路为止。

用 $d_x$ 表示节点 $x$ 在分层图中的深度（即从源点 $s$ 出发的最短路径长度）。构建分层图时，只保留从当前节点 $u$ 到邻接节点 $v$ 的正向残量容量 $c_{uv}>0$ 且 $d_v = d_u + 1$ 的边。

整个流程可以分为以下几步：

1. **构建分层图（BFS）**：
   - 从源点 $s$ 开始，进行 BFS，标记每个节点的深度 $d_x$；
   - 若汇点 $t$ 无法被访问到，则说明当前已无法继续增广，算法结束。
2. **寻找增广路（DFS）**：
   - 从源点 $s$ 出发，使用 DFS 在分层图上寻找所有可能的增广路；
   - 每次沿着符合条件的边走，直到到达汇点 $t$，然后更新路径上的所有边的残量；
   - 使用当前弧优化（即记住每个点当前扫到哪个边），避免重复扫描无用边。
3. **重复执行以上两步**，直到无法再构建出新的分层图（即 $t$ 不可达）。

### 时间复杂度

Dinic 算法的时间复杂度为：

- 一般图上是 $O(n^2 m)$；
- 若所有边的容量均为 $1$（单位网络），则复杂度可以优化到 $O(\sqrt{n} m)$；
- 对于二分图匹配类问题，其复杂度也为 $O(\sqrt{n} m)$。

### Code（P3376）

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
const int INF=1e18;
int n,m,s,t;
struct lyl{
    int to;
    int w;
    int nxt;
}e[200010];
int tot=1;
int head[200010];
void add(int u,int v,int w){
    e[++tot]={v,w,head[u]};
    head[u]=tot;
}
int maxflow;
int now[200010];//当前弧优化
int dis[200010];//分层
int bfs(){
    for(int i=1;i<=n;i++){
        dis[i]=INF;
    }
    queue<int> q;
    q.push(s);
    dis[s]=0;
    now[s]=head[s];
    while(!q.empty()){
        int u=q.front();
        q.pop();
        dis[s]=0;
        now[s]=head[s];
        for(int i=head[u];i;i=e[i].nxt){
            int v=e[i].to;
            if(e[i].w>0&&dis[v]==INF){
                q.push(v);
                now[v]=head[v];
                dis[v]=dis[u]+1;
                if(v==t){
                    return 1;
                }
            }
        }
    }
    return 0;
}
int dfs(int u,int sum){
    if(u==t){
        return sum;
    }
    int k,res=0;
    for(int i=now[u];i&&sum;i=e[i].nxt){
        now[u]=i;
        int v=e[i].to;
        if(e[i].w>0&&(dis[v]==dis[u]+1)){
            k=dfs(v,min(e[i].w,sum));
            if(k==0){
                dis[v]=INF;
                
            }
            e[i].w-=k;
            e[i^1].w+=k;
            res+=k;
            sum-=k;
        }
        
    }
    return res;
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m>>s>>t;
    for(int i=1;i<=m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        add(u,v,w);
        add(v,u,0);
    }
    while(bfs()){
        maxflow+=dfs(s,INF);
    }
    cout<<maxflow<<'\n';
    return 0;
}
```

