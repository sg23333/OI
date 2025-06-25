> 2025/03/06

# Luogu P11848 [TOIP 2023] 房屋推荐

### 题意

给定平面上的 $n$ 个房子和 $m$ 个地铁站，每个房子有坐标 $(x, y)$ 和租金 $r$，编号 $i$，每个地铁站有坐标 $(x, y)$。对于每个房子，计算它到最近地铁站的距离，并按照以下规则对房子排序：

1. 距离小的优先。
2. 距离相等的租金 $r$ 低的优先。
3. 租金 $r$ 相等的编号 $i$ 低的优先。

求排序后每个房子的编号。

### 思路

看见 $n \le 10^5$，$m \le 10^3$，对于每个房子 $i$，枚举每个地铁站 $j$，时间复杂度也完全可以接受。

注意 $r_i \le 10^9$，最小值的初值一定要设的足够大。

枚举复杂度：$O(nm)$。

排序复杂度：$O(n \log n)$。

总时间复杂度：$O(nm+n \log n)$。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;

int n,m;

struct lxl{
    int x,y;
}b[100005];
struct lyl{
    int x,y;
    int r;
    int id;
    int val;
}a[100005];
bool operator<(lyl x,lyl y){
    if(x.val!=y.val){
        return x.val<y.val;
    }
    else if(x.r!=y.r){
        return x.r<y.r;
    }
    else{
        return x.id<y.id;
    }
}

signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        int x,y,r;
        cin>>x>>y>>r;
        a[i].x=x;
        a[i].y=y;
        a[i].r=r;
        a[i].id=i;
    }
    for(int j=1;j<=m;j++){
        int x,y;
        cin>>x>>y;
        b[j].x=x;
        b[j].y=y;
    }
    for(int i=1;i<=n;i++){
        int ret=LLONG_MAX;
        for(int j=1;j<=m;j++){
            int dx=a[i].x-b[j].x;
            int dy=a[i].y-b[j].y;
            int now=dx*dx+dy*dy;
            ret=min(ret,now);
        }
        a[i].val=ret;
    }
    sort(a+1,a+1+n);
    for(int i=1;i<=n;i++){
        cout<<a[i].id<<'\n';
    }
    return 0;
}
```