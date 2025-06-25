# P1931 套利

### 题意

给你一些货币的兑换关系，判断是否能通过换汇的方式获利。

### 思路

使用 `floyed` 判断即可。

###代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,m;
int cnt;
map<string,int> mp;
double dis[110][110];
int flag;
int pos;
signed main(){
//    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    while(cin>>n&&n){
        cnt=0;
        flag=0;
        pos++;
        for(int i=1;i<=n+1;i++){
            for(int j=1;j<=n+1;j++){
                if(i==j){
                    dis[i][j]=1;
                }
                else{
                    dis[i][j]=0;
                }
            }
        }
        
        for(int i=1;i<=n;i++){
        	
            string s;
            cin>>s;
            mp[s]=++cnt;
        }
        cin>>m;
        for(int i=1;i<=m;i++){
            string a,b;
            double x;
            cin>>a>>x>>b;
            dis[mp[a]][mp[b]]=x;
        }
        for(int k=1;k<=cnt;k++){
            for(int i=1;i<=cnt;i++){
                for(int j=1;j<=cnt;j++){
                    dis[i][j]=max(dis[i][k]*dis[k][j],dis[i][j]);
                }
            }
        }
        for(int i=1;i<=cnt;i++){
            if(dis[i][i]>1){
                flag=1;
                break;
            }
        }
        cout<<"Case "<<pos<<": ";
        if(flag){
            cout<<"Yes"<<'\n';
        }
        else{
            cout<<"No"<<'\n';
        }
    }

    return 0;
}
```

