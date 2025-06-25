> 2024-12-26

## Luogu B4107 [CSP-X2024 山东] 刷题

### 题意

给定 $n$ 道题目，每道题的完成时间为 $a_i$，计划在 $m$ 天内按照题目编号顺序完成所有题目。每天可以求助一次好友，好友可以帮忙完成当天耗时最长的一道题目，被帮忙完成的题目不计入当天的总耗时。

求做题时间最长的一天所耗时的最小值。

### 思路

发现 $n\le 10^5$，要使用一种 $O(n \log n)$ 的算法。

求最大值最小，考虑二分+贪心。

二分做题时间最长的一天的耗时 $x$。

$l$ 初始为 $0$（理论最小值）。

$r$ 初始为所有 $a_i$ 的和（全题目加起来做一天的情况）。

假设遍历到了第 $i$ 道题，记录所选题目区间的最大值 $maxn$，如果总时间 $sum$ 减去 $maxn$ 小于 $x$，说明今天不能再做题了，下一天从第 $i$ 道题开始做，当做完所有题后，如果天数大于 $m$，则说明 $x$ 要增加，否则反之。

以样例为例：

```
1 2 3 3
```

一开始 $l=0,r=9,mid=4$。

前 $3$ 题 $sum=6,maxn=3$，$sum-maxn<mid$。

第 $4$ 题下一天做，这是合法的。

然后 $l=0,r=3,mid=1$。

这样不合法。

再然后 $l=2,r=3,mid=2$。

同样不合法。

最后 $l=r=3$，输出 $3$，结束。

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"songge888"<<'\n';
#define int long long
using namespace std;
int n,m,a[100010]; 
int l,r,ret;
bool check(int x){
	int num=1;
	int sum=0;
	int maxn=0;
	for(int i=1;i<=n;i++){
		maxn=max(maxn,a[i]);
		if(sum+a[i]-maxn<=x){
			sum+=a[i];
		}
		else{
			num++;
			sum=a[i];
			maxn=a[i];
		}
	}
	return (num<=m);
}
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n;i++){
    	cin>>a[i];
    	r+=a[i];
	}
	while(l<=r){
		int mid=(l+r)/2;
		if(check(mid)){
			ret=mid;
			r=mid-1;
		}
		else{
			l=mid+1;
		}
	}
	cout<<ret;
	return 0;
}


```