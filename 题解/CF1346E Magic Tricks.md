> 2025/03/26

# CF1346E Magic Tricks

> **注意：此题只能用 Kotlin 语言提交。**

### 题意

给定 $n$ 个球，其中一个是特殊球，初始位置为 $k$。有 $m$ 次交换操作，每次交换两个位置上的球。你可以选择是否执行某些交换，以使特殊球最终到达某个位置。

要求计算每个位置 $i$，使特殊球到达该位置所需阻止交换次数，如果无法到达则输出 $-1$。

### 思路

考虑动态规划，定义 $dp_{i,j}$ 表示前 $i$ 个操作后，特殊球在 $j$ 的最小代价。

假设第 $i$ 次交换为 $(u_i, v_i)$，则状态转移方程为：
$$
dp_{i,{u_i}}=min(dp_{i-1,{v_i}},dp_{{i-1},{u_i}}+1)\\
 dp_{i,{v_i}}=min(dp_{i-1,{u_i}},dp_{{i-1},{v_i}}+1)
$$
对于其他位置，$dp$ 值不变。

由于 $dp$ 只依赖于上一步的状态，因此可以使用**滚动数组**优化。

最终时间复杂度为 $O(m)$。

### Code

```kotlin
import java.util.*

fun main(){
    val scanner=Scanner(System.`in`);
    val n=scanner.nextInt();
    val m=scanner.nextInt();
    val k=scanner.nextInt();
    val dp=IntArray(n+1){m+1};
    dp[k]=0;
    repeat(m){
        val a=scanner.nextInt();
        val b=scanner.nextInt();
        val aa=minOf(dp[a]+1,dp[b]);
        val bb=minOf(dp[b]+1,dp[a]);
        dp[a]=aa;
        dp[b]=bb;
    }
    for(j in 1..n){
        if(dp[j]>m){
            print(-1);
        }
        else{
            print(dp[j]);
        }
        print(" ");
    }
    println();
}
```