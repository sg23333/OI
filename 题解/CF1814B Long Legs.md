## CF1814B Long Legs

### 题意
给定起点 $(0,0)$，终点 $(x,y)$，初始步长为 $1$，你可以执行以下三种操作：

1. 移动到 $(x+m, y)$。
2. 移动到 $(x, y+m)$。
3. 步长 $m \rightarrow m+1$。

求从 $(0,0)$ 到 $(x,y)$ 的最少操作次数。

### 思路

假设最终步长为 $m$，则需要的操作次数为：
$$
f(m) = \left\lceil \frac{x}{m} \right\rceil + \left\lceil \frac{y}{m} \right\rceil +(m-1) \tag 1
$$


此时 $f(m)$ 是离散的，不好分析，不妨把上取整先去掉，即：
$$
f(m)= \frac{x+y}{m} +m-1 \tag 2
$$
目的是求 $f(m)$ 的最小值。

可以推出来 $f^′(m)$ 如下：
$$
f^′(m)= - \frac{x+y}{m^2}+1 \tag 3
$$
在 $m=\sqrt{x+y}$ 的时候，$(1)$ 式近似可以取到最小值，但是可能会出现误差，可以把 $m$ 附近的数都枚举一下取最优情况就可以了。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int t,a,b;
int ans;
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>t;
    while(t--){
        cin>>a>>b;
        int m=sqrt(a+b);
        ans=1e18;
        for(int i=max(m-100,1ll);i<=m+100;i++){
            ans=min(ans,(int)(ceil(a*1.0/i))+(int)(ceil(b*1.0/i))+i-1);
        }
        cout<<ans<<'\n';
    }
    return 0;
}
```

[提交记录](https://codeforces.com/contest/1814/submission/320317226)
