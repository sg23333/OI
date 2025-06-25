> 2024-12-23

# CF2001D Longest Max Min Subsequence

### 题意

给你一个序列 $a$，输出 $a$ 的最长不重复子序列，若存在长度相同的子序列，则输出将奇数位取反后字典序最小的一个。

### 思路

首先子序列的最长长度一定是序列 $a$ 中不同数字的个数。

为了找到符合题意的子序列，则要让奇数位最大，偶数位最小。

那么最值应该在哪一些数里面取呢？

维护一个 `last` 数组，存的是每个元素最后出现的位置，从前往后遍历，如果当前的位置是这个数字最后一次出现并且前面没有选过，那么这个数字是肯定要选的，但是当前不一定要选这个数。

```c++
   a:5  2 1 7 9 7  2 5 5 2
last:9 10 3 6 5 6 10 9 9 10
```

当遍历到 $1$ 时，这是 $1$ 最后一次出现，但可以让它前面的数字先进入子序列，奇数位要尽可能大，所以选前三位中最大的数字 $5$，之后就不能选 $5$ 之前的了，下一次在 $2$ 和  $1$ 中选一个最小值 $1$，注意每次选完之后都要将选的数打一个标记，这样就算之后这个数最后一个出现也不用选了。

可以看出这是在维护一个区间最值，如果暴力枚举时间复杂度是 $O(n)$，总的复杂度就是 $O(n^2)$，但可以用优先队列实现 $O(\log n)$ 维护~~当然也可以用单调队列维护，但这个题可以过  $\log$ 所以就没用~~。

这样总的复杂度就是 $O(n \log n)$，可以通过此题。

注意多测要清空。

### Code

```c++
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"songge888"<<'\n'
using namespace std;
int T,n,a[300010],Last[300010];
int vis[300010];
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
 	cin>>T;
 	while(T--){
	 	cin>>n;
		for(int i=1;i<=n;i++){
		    cin>>a[i];
		}
		for(int i=1;i<=n;i++){
		    Last[i]=1e18;
		}
		for(int i=1;i<=n;i++){
		    Last[a[i]]=i;
		}
		    
		priority_queue<int,vector<int>,greater<int>> q(Last+1,Last+1+n);
		priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> q1,q2;
		for(int i=0;i<=n+10;i++){
		    vis[i]=0;
		}
		for(int i=0;i<=q.top();i++){
		    q1.push({-a[i],i});
		    q2.push({a[i],i});
		}
		vector<int> ans;
		int now=0,tot=1;
		while(!q2.empty()){
		    int x,pos;
		    if(tot%2){
		    	x=q1.top().first;
		    	pos=q1.top().second; 
			}
			else{
				x=q2.top().first;
		    	pos=q2.top().second;
			}
		        
		    if(tot%2){
		        q1.pop();
		        x*=-1;
		    }
			else{
		        q2.pop();
		    }
		        
		    ans.push_back(x);
		    now=pos+1;
		    vis[x]=1;
		    tot++;
		    while(!q.empty()&&q.top()!=1e18&&vis[a[q.top()]]){
		        int j=q.top();
		        q.pop();
		        for(int k=j+1;k<=min(q.top(),n);k++){
		            q1.push({-a[k],k});
		            q2.push({a[k],k});
		        }
		    }
		    while(!q1.empty()&&(vis[-q1.top().first]||q1.top().second<now)){
		        q1.pop();
		    }
		    while(!q2.empty()&&(vis[q2.top().first]||q2.top().second<now)){
		        q2.pop();
		    }
		}
		cout<<ans.size()<<'\n';
		for(auto x:ans){
		    cout<<x<<' ';
		}
		cout<<'\n';
	}
	
    return 0;
}
```
[提交记录](https://codeforces.com/contest/2001/submission/297769938)