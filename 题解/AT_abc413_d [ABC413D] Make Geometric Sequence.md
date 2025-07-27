## AT_abc413_d [ABC413D] Make Geometric Sequence

### 题意

给你一个序列 $a$，确保 $a_i$ 不为 $0$，判断是否存在一个 $a$ 的排列满足等比数列。

### 题解

先考虑 $a_i>0$：

显然先把数列从小到大排序，判断即可，发现直接求公比可能会出现精度损失，可以把除法转换成乘法，对于等比数列，一定有 $a_{i-1} \times a_{i+1}={a_i}^2$。

然后考虑如果存在 $a_i<0$：

先进行特判：

1. $|{正数的个数-负数的个数}|$ 只能是 $0$ 或 $1$。
2. 数列中绝对值相同的数字出现次数只能是 $1$ 或 $n$。

接下来用一个 `map` 存下每个数字的正负：

+ 若 $a_i>0$，$mp_{a_i}=1$。
+ 若 $a_i<0$，$mp_{a_i}=-1$。

可以发现，现在满足是等比数列的条件是：
$$
mp_{a_{i-1}} \times mp_{a_{i+1}} \times a_{i-1} \times a_{i+1}={a_i}^2
$$
判断即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int t;
int n;
int a[200010];
map<int,int> vis,mp;
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>t;
    while(t--){
        cin>>n;
        int cnt1=0,cnt2=0;
        vis.clear();
        mp.clear();
        for(int i=1;i<=n;i++){
            cin>>a[i];
            if(a[i]>0){
            	cnt1++;
            	mp[a[i]]++;
				vis[a[i]]=1;
			}
            else if(a[i]<0){
            	cnt2++;
            	a[i]*=-1;
            	mp[a[i]]++;
            	vis[a[i]]=-1;
			}
        }
        int fl=0;
		sort(a+1,a+1+n);
		for(int i=1;i<=n;i++){
			if(mp[a[i]]>1&&mp[a[i]]!=n){
				fl=1;
			}
		}
		if(!fl){
			if(cnt1==0||cnt2==0){
				//不用考虑
			}
			else if(abs(cnt1-cnt2)>1){
				fl=1;
			}
		}
        if(fl){ 
            cout<<"No"<<'\n';
            continue; 
        }
        for(int i=2;i<n;i++){
            if(a[i]*a[i]!=vis[a[i-1]]*vis[a[i+1]]*a[i-1]*a[i+1]){
                fl=1;
                break;
            }
        }
        if(fl){
            cout<<"No"<<'\n';
        }
        else{
            cout<<"Yes"<<'\n';
        }
    }
    return 0;
}
```

[提交记录](https://atcoder.jp/contests/abc413/submissions/67342548)