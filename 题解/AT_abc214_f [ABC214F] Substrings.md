> 2025-02-18

# AT_abc214_f [ABC214F] Substrings

### 题意

给你一个字符串 $S$，求去除若干个不相邻的字符后得到的不同的字符串的个数。

### 思路

考虑 DP，$dp_{i,j}$ 表示前 $i$ 位以字母 $j$ 为结尾的不同字符串的个数。

对于 $S_i$：

+ 若 $S_i=j$，$dp_{i,j}= \sum_{k=1}^{26} dp_{i-2,k} +1$。
+ 若 $S_i \ne j$，$dp_{i,j}=dp_{i-1,j}$。

最后答案就是 $\sum_{i=1}^{26} dp_{n,i}$。

时间复杂度比 $O(n)$ 大一点，但是实际上达不到，可以通过此题。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
string s;
int n;
int dp[200010][30];
int get(char c){
	return c-'a'+1;
}
int ans;
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>s;
	n=s.size();
	s=' '+s;
//	cout<<get(s[1])<<'\n';
	dp[1][get(s[1])]=1;
	for(int i=2;i<=n;i++){
		for(int j=1;j<=26;j++){
			if(get(s[i])==j){
				for(int k=1;k<=26;k++){
					dp[i][j]+=dp[i-2][k];
				}
				dp[i][j]++;
			}
			else{
				dp[i][j]=dp[i-1][j];
			}
			dp[i][j]%=1000000007;
		} 
	}
	for(int i=1;i<=26;i++){
//		cout<<ans<<'\n';
		ans+=dp[n][i];
		ans%=1000000007;
	}
	cout<<ans;
	return 0;
}

```

[提交记录](https://atcoder.jp/contests/abc214/submissions/62890195)