## AT_abc416_e [ABC416E] Development

### 题意

A 国有 $n$ 座城市，$m$ 条双向道路，每条道路连接两个城市并有一定的通行时间。有 $k$ 个城市设有机场，任意两个有机场的城市之间可在 $t$ 小时内到达。

接下来有 $q$ 次操作：

1. 添加一条道路。
2. 新增一个机场。
3. 询问所有城市对之间的最短路径总和。

### 题解

注意到 $n \le 500,q\le 1000$，也就是说大概可以接受每次询问 $O(n^2)$。

一开始输入完整张图后，可以先跑一遍 Floyd，对于机场，先不考虑两两连边，而是把所有机场先存到一个数组里。

接下来处理操作：

1. 让我们先考虑普通的 Floyd 的实现，分别枚举 $k,i,j$，以 $k$ 为中间节点转移。

   现在可以把给出的边 $(u,v,w)$ 当作那个 $k$，原本是 $i \to k \to j$ 转移，现在变为 $i \to u \to v \to j$ 或者是 $i \to v \to u \to j$。

2. 把新增的机场存起来。

3. 枚举 $i,j$，则 $i \to j$ 的最短距离有两种情况：

   + 不使用机场，则就是刚才用 Floyd 求出的答案。
   + 使用机场，先记录每个点与它最近的机场 $d_i$，最后的答案就是 $i \to d_i+t+d_j \to j$。

时间复杂度 $O(qn^2)$。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
int k,t,q;
vector<int> d;
int dis[555][555];
int di[555];
void add(int u,int v,int o){
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            if(dis[i][u]+o+dis[v][j]<dis[i][j]){
                dis[i][j]=dis[i][u]+o+dis[v][j];
            }
        }
    }
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    memset(di,0x3f,sizeof di);
    memset(dis,0x3f,sizeof dis);
    for(int i=1;i<=n;i++){
        dis[i][i]=0;
    }
    for(int i=1;i<=m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        dis[u][v]=dis[v][u]=min(dis[u][v],w);
    }
    cin>>k>>t;
    for(int i=1;i<=k;i++){
        int x;
        cin>>x;
        d.push_back(x);

    }
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(dis[i][k]+dis[k][j]<dis[i][j]){
                    dis[i][j]=dis[i][k]+dis[k][j];
                }
            }
        }
    }
    cin>>q;
    while(q--){
        int op;
        cin>>op;
        if(op==1){
            int u,v,w;
            cin>>u>>v>>w;
            add(u,v,w);
            add(v,u,w);
        }
        else if(op==2){
            int x;
            cin>>x;
            d.push_back(x);
        }
        else{
            int ans=0;
            for(int i=1;i<=n;i++){
            
                for(auto j:d){
                    di[i]=min(di[i],dis[i][j]);
                }
            }
            
            for(int i=1;i<=n;i++){
                for(int j=1;j<=n;j++){
                    int pos=dis[i][j];
                    if(d.size()){
                        dis[i][j]=min(dis[i][j],di[i]+t+di[j]);
                    }
                    if(dis[i][j]<0x3f3f3f3f3f3f3f3f){
                        ans+=dis[i][j];
                    }
                }
            }
            cout<<ans<<'\n';
        }
    }
    return 0;
}
```

[提交记录](https://atcoder.jp/contests/abc416/submissions/67954469)