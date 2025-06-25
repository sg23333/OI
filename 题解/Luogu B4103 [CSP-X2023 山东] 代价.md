> 2024-12-30

# Luogu B4103 [CSP-X2023 山东] 代价

二分做法。

### 题意

有一个长度为 $n$ 的序列 $a$，每次可以花费 $A$ 的代价将 $a_i+1$，也可以花费 $B$ 的代价将 $a_i-1$，求将 $a$ 序列全部变成相同的一个数的最小代价。

### 思路

注意到 $n \le 10^5,a_i \le 10^9$，可以使用 $O(n \log a_i)$ 的做法。

考虑二分值域。

设序列中最小值为 $minn$，最大值为 $maxn$。

贪心的想，最终的答案一定在 $[minn,maxn]$ 之间。

所以把左边界 $l$ 赋为 $minn$，右边界 $r$ 赋为 $maxn$，进行二分。

$O(n)$ 的枚举答案时 $mid$ 和 $mid+1$ 时的代价 $f(mid)$ 和 $f(mid+1)$，如果 $f(mid)<f(mid+1)$ ，则可以尝试缩小 $mid$，否则反之。

总时间复杂度：$O(n\log a_i)$。

注意当 $minn=maxn$ 时要特判输出 $0$。
### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
int n,a,b,v[100010],l=1e18,r,ret;
int f(int x){
    int s=0;
    for(int i=1;i<=n;i++){
        if(v[i]<x){
            s+=(x-v[i])*a;
        }
        else{
            s+=(v[i]-x)*b;
        } 
    }
    return s;
}
signed main(){
    cin>>n>>a>>b;
    for(int i=1;i<=n;i++){
        cin>>v[i];
        l=min(l,v[i]);
        r=max(r,v[i]);
    }
    if(l==r){
        cout<<0<<endl;
        return 0;
    }
    while(l<=r){
        int mid=(l+r)/2;
        if(f(mid)<=f(mid+1)){
            ret=mid;
            r=mid-1;
        }
        else{
            l=mid+1;
        }
    }
    cout<<f(ret)<<endl;
    return 0;
}

```