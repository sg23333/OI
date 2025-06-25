> 2024-12-23

## Luogu P5690 [CSP-S2019 江西] 日期

### 题意
给一个日期 $\texttt{MM-DD}$，每次可以改其中一位，求使日期合法的最小修改次数。
### 思路
`if` 题，注意特判。
- $2$ 月只有 $28$ 天。
- $31$ 天的月份有 $1,3,5,7,8,10,12$。
- $30$ 天的月份有 $4,6,9,11$。
### Code
```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
signed main(){
	int d,m;
    scanf("%lld-%lld",&m,&d);
    if(d==29||d==30){
        if(m!=2&&m>0&&m<=12){
        	cout<<0;
        	return 0;
		}
        cout<<1;
        return 0;
    }
    if(d>0&&d<=28){
        if(m>0&&m<=12){
        	cout<<0;
        	return 0;
		}
        cout<<1;
        return 0;
    }
    if(d==31){
        if(m==2||m==4||m==6||m==9||m==11||m>=13&&m<=19){
        	cout<<1;
        	return 0;
		}
		if(m==1||m==3||m==5||m==7||m==8||m==10||m==12){
        	cout<<0;
        	return 0;
		}
        if(m%10==4||m%10==6||m%10==9){
        	cout<<2;
        	return 0;
		}
        cout<<1;
        return 0;
    }
    if(m==0||m>12){
    	cout<<2;
    	return 0;
	}
    cout<<1;
    return 0;
}
```