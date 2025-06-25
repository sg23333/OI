> 2025/03/07

# AT_nikkei2019ex_g 回文スコア

### 题意

给你若干个字符，从中选择字符组成若干个回文串，每个回文串 $i$ 的得分是 ${len_i}^2$，求最大的 $\sum len_i$。

### 思路

考虑贪心。

由于 ${len_i}^2$ 随长度 ${len_i}$ 增长很快，优先构造尽可能长的回文串是合理的。

可以在一开始先记录每个字符出现的次数，每一次构造回文串，对于每个字符，都有以下两种情况：

- 个数是奇数，如果当前串是空的，全部填进去（中心的字符可以是 $1$ 个），如果当前串不空，就留下一个。
- 个数是偶数，全部填进去。

记录个数 $res$，每次 $ans$ 累加 $res^2$ 即可。

时间复杂度 $O(len)$。

### Code

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
string s;
int cnt[55];
int check(){
    for(int i=1;i<=26;i++){
        if(cnt[i]){
            return 1;
        }
        
    }
    return 0;
}
int len;
int ans;
signed main() {
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>s;
    len=s.size();
    for(int i=0;i<len;i++){
        cnt[s[i]-'a'+1]++;
    }
    while(check()){
        int flag=0;
        int res=0;
        for(int i=1;i<=26;i++){
            if(cnt[i]%2){
                if(flag){
                    res+=cnt[i]-1;
                    cnt[i]=1;
                }
                else{
                    flag=1;
                    res+=cnt[i];
                    cnt[i]=0;
                }
            }
            else{
                res+=cnt[i];
                cnt[i]=0;
            }
        }
        ans+=res*res;
    }
    cout<<ans;
    return 0;
}

```