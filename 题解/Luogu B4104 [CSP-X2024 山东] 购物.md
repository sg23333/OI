> 2024-12-26

## Luogu B4104 [CSP-X2024 山东] 购物

### 题意

有一个序列 $a$，表示第 $1$ 到 $n$ 件商品的价格，$m$ 为一张优惠券能买的最大商品数，$w$ 为一张优惠券的价格。我们需要最小化购买所有商品的总费用。

### 思路

考虑**贪心**。  

题目没有购买顺序的要求，可以先把价格全存下来，为了让优惠卷的价值最大，考虑把 $a$ 序列从大到小排序，每次先选价值最大的 $m$ 个商品，如果这 $m$ 个的价值和 $sum$ 大于优惠卷的价格 $w$，就可以用优惠卷来买。  

注意最后不足 $m$ 个时也可以用优惠卷。  

时间复杂度 $O(n \log n)$。  

详见代码。

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
int n,m,w; 
int a[200010];
int sum,ans;
int tot;
bool cmp(int x,int y){
	return x>y;
} 
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m>>w;
    for(int i=1;i<=n;i++){
    	cin>>a[i];
	}
	sort(a+1,a+1+n,cmp);
	for(int i=1;i<=n;i++){
		sum+=a[i];
		tot++;
		if(tot==m){
			if(sum>w){
				ans+=w;
			}
			else{
				ans+=sum;
			}
			tot=0;
			sum=0;
		}
		
		
	}
	if(sum>w){
		ans+=w;
	}
	else{
		ans+=sum;
	}
	cout<<ans;
	return 0;
}


```