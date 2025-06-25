> 2025-01-19

## AT_abc389_d [ABC389D] Squares in Circle

数学题。

### 题意

给你一个铺满单位长度为 $1$ 的平面，给你一个圆的半径 $r$，求以一个正方形的中心为圆心，以 $r$ 为半径的圆，最多可以包含几个完整的正方形。

### 思路

假设有这样一个图：
![QQ20250119-081733.png](https://s2.loli.net/2025/01/19/cDNp6SjqTyLKx8d.png)

如果以 $B$ 点为圆心，半径是 $4$ 的话，图就长这样：

![QQ5119-081733.png](https://s2.loli.net/2025/01/19/CFy9DsIhb6LgQZJ.png)

可以发现，中间有个十字的部分是一定会被包含的。

![444.png](https://s2.loli.net/2025/01/19/sC3SgWw5rKZeUqY.png)

记答案为 $ans$，不妨先把 $ans$ 加上 $4\times (r-1)+1$。

然后发现剩下可以分成完全相同的四部分，我们已左下角的来举例：

第一层 `for` 循环，表示横着是第 $i$ 排。

第二层 `for` 循环，表示竖着是第 $j$ 排。

![445.png](https://s2.loli.net/2025/01/19/JEL9RqSyZlu5k3Y.png)

想这样，发现复杂度是 $O(r^2)$ 的，会超时，考虑优化。

发现随着 $i$ 增大，$j$ 是单调递减的。

所以可以记录一个 $pos$ 每次减一下，就不用每次都重新算了。

每次判断 $(pos+0.5)^2+(i+0.5)^2$ 和 $r^2$ 的大小就可以了。

时间复杂度 $O(r)$。

### Code

```c++
#include<bits/stdc++.h>
#define int long long
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int r;
int ans=1;
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>r;
	ans+=4*(r-1);
	int pos=r-1;
	int ans1=0;
	for(int i=1;i<r;i++){
		while((pos*1.0+0.5)*(pos*1.0+0.5)+(i*1.0+0.5)*(i*1.0+0.5)>r*r&&pos>0){
			pos--;
			
		}
		ans1+=pos;
	}
	ans+=4*ans1;
	cout<<ans;
	return 0;
}
```