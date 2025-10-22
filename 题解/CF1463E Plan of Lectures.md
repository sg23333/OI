## CF1463E Plan of Lectures

### 题意

要求你构造一个长度为 $n$ 的排列，要满足：

1. $a_i$ 出现在 $i$ 之前，如果 $a_i=0$ 代表这个数没有限制。仅对条件一保证一定有解。

2. 有 $k$ 个特殊对 $(i,j)$，要求满足 $i$ 在排列中一定在 $j$ 的左一位。询问是否存在这样的排列。

保证恰好存在一个 $i$ 使得 $a_i=0$。

### 题解

先只考虑第一种条件，可以直接用拓扑排序做。

再考虑加上第二种情况，对于每一组 $(i,j)$，实际上就是把 $i,j$ 缩成了一个点，可以用并查集维护。

具体实现上，可以对每一个点都记录一个后继，可以确保缩点之后，每个块内的点的顺序时不变的；并查集合并的时候，注意把靠后的点并到靠前的点，这样方便记录。

### 代码

```
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,k;
int a[300010];
vector<int> g[300010];
int fa[300010];
int vis[300010];
int nxt[300010];
vector<int> ve;
int val[300010];
int in[300010];
vector<int> ans;
int Find(int x){
    return fa[x]==x?x:fa[x]=Find(fa[x]);
}

void merge(int x,int y){
    fa[Find(x)]=Find(y);
}
void topu(){
    queue<int> q;
    for(int i=1;i<=n;i++){
        int id=Find(i);
        if(id==i&&!in[id]){
            q.push(id);
        }
    }
    while(!q.empty()){
        int u=q.front();
        q.pop();
        ans.push_back(u);
        for(auto v:g[u]){
            in[v]--;
            if(!in[v]){
                q.push(v);
            }
        }
    }
    return ;
}

signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>k;
    for(int i=1;i<=n;i++){
        fa[i]=i;
    }
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    for(int i=1;i<=k;i++){
        int x,y;
        cin>>x>>y;
        if(Find(x)==Find(y)){
            cout<<0<<'\n';
            return 0;
        }
        merge(y,x);
        nxt[x]=y;
    }
    for(int i=1;i<=n;i++){
        if(i==Find(i)){
            ve.push_back(i);
            int las=i;
            while(nxt[las]){
                val[nxt[las]]=val[las]+1;
                las=nxt[las];
            }
        }
    }
    for(int i=1;i<=n;i++){
        if(a[i]){
            int u=Find(a[i]);
            int v=Find(i);
            if(u==v){
                if(val[a[i]]>val[i]){
                    cout<<0<<'\n';
                    return 0;
                }
            }

            else{
                //u -> v
                g[u].push_back(v);
                in[v]++;
            }
        }
    }
    topu();
    if(ans.size()!=ve.size()){
        cout<<0<<'\n';
    }
    else{
        for(auto i:ans){
            cout<<i<<' ';
            int las=nxt[i];
            while(las){
                cout<<las<<' ';
                las=nxt[las];
            }
        }
    }
    return 0;
}
```

