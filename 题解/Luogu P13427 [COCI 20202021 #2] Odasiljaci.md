## P13427 [COCI 2020/2021 #2] Odasiljaci

### 题意

在二维平面上给你 $n$ 个点的坐标。你需要找到一个最小的半径 $r$，为每个点都画上一个半径为 $r$ 的圆，使得这些圆都联通。

### 题解

注意到 $n \le1000$，可以支持 $O(n^2)$ 的做法。

题目其实等价于，连 $n$ 条边使得整个图联通，其中最长的边是多少。

这样就转换为了最小生成树问题。

最后的答案就是最长的边长的一半（因为一条边是两个点同时连的，半径是边长的一半时，半径的最大值最小）。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n;
int fa[1005];
int dis[500005];
int ans;
int cnt;
struct lyl{
    int x,y;
}a[1005];

struct edge{
    int u,v;
    int d;
};
bool operator<(edge x,edge y){
    return x.d<y.d;
}
vector<edge> b; 
int Find(int x){
    if(fa[x]==x){
        return x;
    }
    return fa[x]=Find(fa[x]);
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i].x>>a[i].y;
    }
    for(int i=1;i<=n;i++){
        for(int j=i+1;j<=n;j++){
            int dx=a[i].x-a[j].x;
            int dy=a[i].y-a[j].y;
            b.push_back({i,j,dx*dx+dy*dy});
        }
    }
    sort(b.begin(),b.end());
    for(int i=1;i<=n;i++){
        fa[i]=i;
    }
    int tot=0;
    for(auto [u,v,d]:b){
        int fx=Find(u);
        int fy=Find(v);
        if(fx!=fy){
            fa[fx]=fy;
            ans=d;
            tot++;
            if(tot==n-1){
                break;
            }
        }
    }
    cout<<fixed<<setprecision(7)<<sqrt((double)ans)/2.0<<'\n';
    return 0;
}

```

