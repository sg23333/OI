> 2025.6.16

## CF1295D Same GCDs

### 题意

给定 $a,m$，求有多少 $m$（$0 \le x < m$），满足：
$$
\gcd(a,m)=\gcd(a+x,m)
$$

### 思路

设 $\gcd(a,m)=g$，$a'=\frac{a}{g}$，$x'=\frac{x}{g}$，$m'=\frac{m}{g}$，则
$$
\begin{align*}
\gcd(a+x,m) &= g\\
\gcd(a'+x',m') &= 1 \\
\gcd((a'+x') \bmod {m'},m') &= 1
\end{align*}
$$
因为
$$
x' \in[0,m)
$$
所以
$$
(a'+x')\bmod {m'} \in[0,m)
$$
发现最后求的就是 $\phi(m)$

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int t;
int phi(int x){
    int res=x;
    for(int i=2;i*i<=x;i++){
        if(x%i==0){
            while(x%i==0)x/=i;
            res-=res/i;
        }
    }
    if(x>1)res-=res/x;
    return res;
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>t;
    while(t--){
        int a,m;
        cin>>a>>m;
        int g=__gcd(a,m);
        cout<<phi(m/g)<<'\n';
    }
    return 0;
}

```

