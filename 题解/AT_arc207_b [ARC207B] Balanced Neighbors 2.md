### 题意

构造一个无向图 $G = (V, E)$，对于 $\exists X$，使 $\forall v \in V$，均满足：

$$\sum_{u \in S_v} u = X$$

其中，$S_v=\{u\in V \mid 0<d(v,u) \leq 2\}$，$d(v,u)$ 表示顶点 $v$ 和 $u$ 在图 $G$ 中的最短路径距离。

### 思路

分类讨论奇数和偶数。

1. 偶数

   先以 $n=6$ 举例。

   考虑构造出一个完全图：

   <img src="https://s2.loli.net/2025/10/22/6AGz3dY1QXD8bVy.png" height="200">

   令 $S$ 为编号和，$v_i$ 为每个点的答案，$v_i=S-i$（每个点都可以通过一步走到其他点）。

   发现 $v_i+v_{n-i+1}=2S-n-1$，是一个固定的值，只要保证 $v_i$ 和 $v_{n-i+1}$ 的值相同，所有的 $v_i$ 就相同，也就是要求在构造出来的图中，$i$ 和 $n-i+1$ 不能在两步之内联通即可。

   把点 $i$ 和点 $n-i+1$ 称为对应点。

   考虑构造一个二分图，$1,3,5,\dots,n-1$ 为左部点，$2,4,6,\dots,n$ 为右部点：

   <img src="https://s2.loli.net/2025/10/22/Ie5yYzrZCfvQ6wi.png" height="250">

   之后对每个左部点和右部点连边（对应点除外）：

   <img src="https://s2.loli.net/2025/10/22/kNl8t417uVzH9j2.png" height="250">

   构造后发现，每个左部点都可以用一步到达所有右部点（对应点除外），右部点同理。所以每个点 $i$ 都可以在两步之内走到除对应点之外的所有点。

   此时 $v_i=S-n-1$。

   

2. 奇数

   在偶数的基础上拓展。

   以 $n=7$ 为例。

   <img src="https://s2.loli.net/2025/10/22/XfFWszHhBrcZImw.png" height="350">

   

   

   点 $1-6$ 已经按照偶数的规律排好，此时 $v_{1-6}=S-2n$，$v_7=0$，只要把 $7$ 向一侧的所有点连边，此时 $v_i=S-n$，满足题意。

   <img src="https://s2.loli.net/2025/10/22/y5IXO7HgpzrSmZ9.png" height="250">

综上：偶数时将左部点连向所有不对应的右部点，奇数时多出的一个点向一侧的所有点连边即可。

时间复杂度：$O(n^2)$。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n;
vector<pair<int,int> >ans;
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    if(n<=5){
        cout<<-1;
    }
    else if(n%2==0){
        for(int i=1;i<=n/2;i++){
            for(int j=1;j<=n/2;j++){
                int x=i*2-1;
                int y=j*2;
                if(x+y==n+1){
                    continue;
                }
                ans.push_back({x,y});
                // cout<<x<<' '<<y<<'\n';
            }
        }
        cout<<ans.size()<<'\n';
        for(auto [x,y]:ans){
            cout<<x<<' '<<y<<'\n';
        }
    }
    else{
        n--;
        for(int i=1;i<=n/2;i++){
            for(int j=1;j<=n/2;j++){
                int x=i*2-1;
                int y=j*2;
                if(x+y==n+1){
                    continue;
                }
                ans.push_back({x,y});
                // cout<<x<<' '<<y<<'\n';
            }
        }
        for(int i=1;i<=n/2;i++){
            ans.push_back({i*2-1,n+1});
            // cout<<i<<' '<<n+1<<'\n';
        }
        cout<<ans.size()<<'\n';
        for(auto [x,y]:ans){
            cout<<x<<' '<<y<<'\n';
        }
    }
    return 0;
}
```