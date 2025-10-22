## AT_abc419_d [ABC419D] Substr Swap

### 题意

有 $s,t$ 两个字符串，每次交换 $s,t$ 的 $[l,r]$，求最后的 $s$。

### 题解

发现对于每一个位置，交换偶数次就相当于没有交换，所以只需要知道每个位置被交换过几次。

问题就转换成了区间加，通过差分实现即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
string s,t;
int cha[500010];
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    cin>>s>>t;
    while(m--){
        int l,r;
        cin>>l>>r;
        cha[l]++;
        cha[r+1]--;

    }
    for(int i=1;i<=n;i++){
        cha[i]+=cha[i-1];
        if(cha[i]%2==0){
            cout<<s[i-1];
        }
        else{
            cout<<t[i-1];
        }
    }
    return 0;
}
```

