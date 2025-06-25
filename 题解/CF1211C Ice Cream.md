> 2025/3/31

# CF1211C Ice Cream

> 此篇题解包含 kotlin 和 c++ 代码。

### 题意

给定 $n$ 天的冰淇淋购买计划，每天要吃的范围是 $a_i$ 到 $b_i$ 份，价格是 $c_i$，需要在 $n$ 天内吃掉恰好 $k$ 份冰淇淋，求最小花费。若无法满足条件，输出 $-1$。

### 思路

考虑贪心。

先计算出每天吃的最少的冰淇淋的和 $sum$。

如果 $sum>k$，就一定不行，输出 $-1$。

否则求出剩下的冰淇淋个数 $num$，先按照 $c_i$ 的大小从小到大排序，$c_i$ 小的先吃，并且尽可能多吃，吃到 $num=0$ 为止。

注意会爆 `int`。

时间复杂度 $O(n \log n)$。

### Code

#### kotlin

```kotlin
import java.util.*
data class lyl(
    val x:Long,
    val y:Long,
    val z:Long
)
fun main() {
    val scanner=Scanner(System.`in`);
    val n=scanner.nextInt();
    val k=scanner.nextLong();
    val a=Array(n){
        lyl(scanner.nextLong(),scanner.nextLong(),scanner.nextLong());
    }
    var sum=0L;
    for((x,y,z) in a){
        sum+=x;
    }
    if(sum>k){
        println(-1);
        return;
    }
    var ans=0L;
    for((x,y,z) in a){
        ans+=x*z;
    }
    var num=k-sum;
    a.sortBy {it.z};
    for((x,y,z) in a){
        if(num<=0){
            break;
        }
        ans+=minOf(num,y-x)*z;
        num-=minOf(num,y-x);
    }
    if(num>0){
        println(-1);
    }
    else{
        println(ans);
    }
}

```

#### c++

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n';
using namespace std;
int n,k;
struct lyl{
    int x,y,z;
}a[200010];
bool operator<(lyl a,lyl b){
    return a.z<b.z;
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n>>k;
    for(int i=0;i<n;i++){
        cin>>a[i].x>>a[i].y>>a[i].z;
    }
    int sum=0;
    for(int i=0;i<n;i++){
        sum+=a[i].x;
    }
    if(sum>k){
        cout<<-1<<'\n';
        return 0;
    }
    int ans=0;
    for(int i=0;i<n;i++){
        ans+=a[i].x*a[i].z;
    }
    int num=k-sum;
    sort(a,a+n);
    for(int i=0;i<n&&num>0;i++){
        ans+=min(num,a[i].y-a[i].x)*a[i].z;
        num-=min(num,a[i].y-a[i].x);
    }
    if(num>0){
        cout<<-1<<'\n';
    }
    else{
        cout<<ans<<'\n';
    }
    return 0;
}
```

[提交记录](https://codeforces.com/contest/1211/submission/313221920)