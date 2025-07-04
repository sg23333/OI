## CF1106D Lunar New Year and a Wander

### 题意

给定一张 $n$ 个点，$m$ 条双向边的图，从 $1$ 号点出发，沿双向边行走（可以重复经过一个点）。当经过一个之前未经过的点时，记录下其编号。这些编号组成一个长度为 $n$ 的序列。求字典序最小的序列。

### 题解

因为可以重复经过点，所以每一步可以走向的新点就是与已经走过的点相邻的点，可以使用一个优先队列维护。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
vector<int> g[100010];
vector<int> ans;
priority_queue<int,vector<int>,greater<int>> q;
int vis[100010];
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    for(int i=1;i<=m;i++){
        int u,v;
        cin>>u>>v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    q.push(1);
    vis[1]=1;
    while(!q.empty()){
        int u=q.top();
        ans.push_back(u);
        q.pop();
        for(auto v:g[u]){
            if(vis[v]){
                continue;
            }
            vis[v]=1;
            q.push(v);
        }
    }
    for(auto i:ans){
        cout<<i<<' ';
    }
    return 0;
}
```

