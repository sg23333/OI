> 2024-08-01

## CF1771C Hossam and Trainees

### 题目大意

在一个序列 $a_i$ 中，寻找不互质的两个数。

### 思路

对每个数进行质因子分解，对质因子进行记录，如果有任意一个质因子出现两次及以上，则说明有至少两个数有共同的质因子，故存在两个数不互质。

题目说 $a_i \le 10^9 $，显然直接分解 $a_i$ 是不行的，但是 $\sqrt{10^9}\approx31623$ ，所以可以寻找 $[1,\sqrt{a_i}]$ 的质因子，最后一个比 $\sqrt{a_i}$ 大的进行特判就可以了。

### AC Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
int t,sum[32222],prime[32222],vis[32222],cnt=0;
map<int,int>mp;//map存储质因子出现的次数 
void ola(){
    for(int i=2;i<=32220;i++){
        if(!vis[i]){
            prime[++cnt]=i;
        }
        for(int j=1;j<=cnt&&i*prime[j]<=32220;j++){
            vis[i*prime[j]]=1;
            if(i%prime[j]==0){
                break;
            }
        }
    }
}
bool check(long long x){	
	for(int j=1;j<=cnt&&x>=prime[j];j++){
		if(x%prime[j]==0){
			if(mp[prime[j]]){//这个因子已经出现过了，直接返回即可 
				return 1;
			}
			mp[prime[j]]=1;
			while(x%prime[j]==0){
				x/=prime[j];
			}
		}
	}
	if(x>1){//进行特判 
		if(mp[x]){//同上 
			return 1;
		}
		mp[x]=1;
	}
	return 0;
}
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);cout.tie(0);
	cin>>t;
	ola();
	while(t--){
		int n,a[100005];
		cin>>n;
		mp.clear();
		int flag=0;
		for(int i=1;i<=n;i++){
			cin>>a[i];
		}
		for(int i=1;i<=n;i++){
			if(check(a[i])){
				flag=1;
			}
		}
		if(!flag){
			cout<<"No"<<endl;	
		}
		else{
			cout<<"Yes"<<endl;
		}
	}
	
	return 0;
}
```