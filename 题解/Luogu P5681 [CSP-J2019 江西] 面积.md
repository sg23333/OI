> 2024-12-23

## Luogu P5681 [CSP-J2019 江西] 面积

### 题意

给定 $a,b,c$，求 $a^2$ 和 $b \times c$ 谁更大。

### 思路

直接模拟即可。
注意 $a,b,c \le 10^9$，要开 `long long`。

### Code

```c++
#include<bits/stdc++.h>
using namespace std;
int main(){
	long long a,b,c;
	cin>>a>>b>>c;
	long long s1=a*a;
	long long s2=b*c;
	if(s1>s2){
		cout<<"Alice";
	}
	else{
		cout<<"Bob";
	}
} 
```