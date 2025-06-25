> 2025-01-15

# Luogu P10459 Raid

做这个题之前可以先看一下这个题：[P7883 平面最近点对（加强加强版）](https://www.luogu.com.cn/problem/P7883)。

### 题意

给你两种点，让你求两种点之间最短的距离。

### 思路

考虑分治。

不用在线，可以先把询问离线下来，再对 $x$ 坐标从小到大排序。

对于每个询问的区间 $[l,r]$，可以拆分成 $[l,mid]$ 和 $[mid+1,r]$。

合并的时候：

- 计算中线位置。
- 找出距离中线不超过当前最小距离的点。
- 计算这些点之间的最短距离。

```c++
void solve(int l,int r){
    if(l==r){
        return;
    }
    int mid=(l+r)/2;
    solve(l,mid);
    solve(mid+1,r);
    double mid1=(e[mid].x+e[mid+1].x)/2;
    int p=0;
    for(int i=l;i<=r;i++){
        if(abs(mid1-e[i].x)<=ans){
            q1[++p]=e[i];
        }
    }
    sort(q1+1,q1+p+1,cmp);
    for(int i=1;i<=p;i++){
        for(int j=i+1;j<=p;j++){
            if(q1[i].id==q1[j].id){
            	continue;
			}
			if(q1[j].y-q1[i].y>ans){
				break;
			}
            ans=min(ans,sqrt((q1[i].x-q1[j].x)*(q1[i].x-q1[j].x)+(q1[i].y-q1[j].y)*(q1[i].y-q1[j].y)));
        }
    }
}
```

就像这样。

还有一些优化，比如：

```c++
if(q1[j].y-q1[i].y>ans){
	break;
}
```

对于每次距离中线不超过当前最小距离的点的 $y$ 坐标进行从小到大排序。

这样当后面有点的 $y$ 和之前的点的 $y$ 的差值比 $ans$ 还大的时候，直接 `break` 就好了。

还有就是题目要求两种点必须不同时才计算，可以对每个点记录一个 `id`，`id` 相等时直接跳过就可以了。

分治的复杂度是 $O(\log n)$（一共有 $\log n$ 层），每次分治时的复杂度是 $O(n \log n)$（遍历 $O(n)$，排序 $O(n\log n)$）。

总时间复杂度：$O(n\log^2 n)$。

### 完整代码

```c++
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"___songge888___"<<'\n';
using namespace std;

int n,t;
double ans=2147483647.0;
struct lyl{
    double x,y;
    int id;
}e[200010],q1[200010];
bool operator <(lyl a,lyl b){
	if(a.x==b.x){
		return a.y<b.y;
	}
	else{
		return a.x<b.x;
	}
}
bool cmp(lyl a,lyl b){
    return a.y<b.y;
}
void solve(int l,int r){
    if(l==r){
        return;
    }
    int mid=(l+r)/2;
    solve(l,mid);
    solve(mid+1,r);
    double mid1=(e[mid].x+e[mid+1].x)/2;
    int p=0;
    for(int i=l;i<=r;i++){
        if(abs(mid1-e[i].x)<=ans){
            q1[++p]=e[i];
        }
    }
    sort(q1+1,q1+p+1,cmp);
    for(int i=1;i<=p;i++){
        for(int j=i+1;j<=p;j++){
            if(q1[i].id==q1[j].id){
            	continue;
			}
			if(q1[j].y-q1[i].y>ans){
				break;
			}
            ans=min(ans,sqrt((q1[i].x-q1[j].x)*(q1[i].x-q1[j].x)+(q1[i].y-q1[j].y)*(q1[i].y-q1[j].y)));
        }
    }
}
signed main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    cin>>t;
    while(t--){
        ans=2147483647.0;
        cin>>n;
        for(int i=1;i<=n;i++){
            cin>>e[i].x>>e[i].y;
            e[i].id=1;
        }
        for(int i=n+1;i<=2*n;i++){
            cin>>e[i].x>>e[i].y;
            e[i].id=0;
        }
        sort(e+1,e+2*n+1);
        solve(1,2*n);
        cout<<fixed<<setprecision(3)<<ans<<'\n';
    }
    return 0;
}
```