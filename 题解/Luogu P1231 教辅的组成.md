> 2025.6.18

## Luogu P1231 教辅的组成

### 题意

有若干教辅，一个完整的教辅包括书本，答案和练习册，现在你已知

- 共有 $N_1$ 本书、$N_2$ 本练习册和 $N_3$ 份答案。
- 若干书可能与哪些练习册对应（$M_1$ 个可能对应关系）。
- 若干书可能与哪些答案对应（$M_2$ 个可能对应关系）。

求最多有多少本完整的教辅。

### 思路

考虑网络流，按照源点 $\to$ 练习册 $\to$ 书本 $\to$ 答案 $\to$ 汇点的顺序连边。

样例如下：

![P1231_1.png](https://s2.loli.net/2025/06/18/iKCSeLjtIfk5Noh.png)

但是这样可能会导致一本书被匹配多次，所以考虑拆点，把书拆成入点和出点，并把边权设为 $1$，就可以避免这个问题了。

![P1231_2.png](https://s2.loli.net/2025/06/18/PLvVTxYoptmaBR7.png)

然后直接跑最大流即可。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
const int INF=1e18;

int n;
int n1,n2,n3;
int m1,m2;
int s,t;
struct lyl{
    int to;
    int w;
    int nxt;
}e[2000010];
int tot=1;
int head[2000010];
void add(int u,int v,int w){
    e[++tot]={v,w,head[u]};
    head[u]=tot;
}
int maxflow;
int now[2000010];
int dis[2000010];
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
    cin>>n1>>n2>>n3;
    cin>>m1;
    n=n2+2*n1+n3+2;
    //源点：1
    //练习册:2--n2+1
    //书（左）:n2+2--n2+n1+1
    //书（右）:n2+n1+2--n2+2*n1+1;
    //答案:n2+2*n1+2--n2+2*n1+n3+1
    //汇点:n2+2*n1+n3+2;
    s=1;
    t=n2+2*n1+n3+2;
    for(int i=2;i<=n2+1;i++){
        add(1,i,1);
        add(i,1,0);
    }
    for(int i=1;i<=m1;i++){
        int x,y;
        cin>>x>>y;
        add(y+1,x+n2+1,1);
        add(x+n2+1,y+1,0);   
    }
    cin>>m2;
    for(int i=1;i<=m2;i++){
        int x,y;
        cin>>x>>y; 
        add(x+n1+n2+1,y+n2+2*n1+1,1);
        add(y+n2+2*n1+1,x+n1+n2+1,0);
    }
    for(int i=1;i<=n3;i++){
        add(n2+2*n1+1+i,t,1);
        add(t,n2+2*n1+1+i,0);
    }
    for(int i=1;i<=n1;i++){
        add(n2+1+i,n2+n1+1+i,1);
        add(n2+n1+1+i,n2+1+i,0);
    }

    while(bfs()){
        maxflow+=dfs(s,INF);
    }
    cout<<maxflow<<'\n';
    return 0;
}
```

