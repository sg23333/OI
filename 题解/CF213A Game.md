## CF213A Game

### 题意

一个环上有 $3$ 个点，每个点上有若干任务，这些任务有可能有依赖关系，每次顺时针走需要花费 $1$ 的代价，逆时针走需要花费 $2$ 的代价，每个任务需要花费 $1$ 的代价。可以从任意点出发，保证数据有解，求完成任务最小代价。

### 题解

题目要求总的时间最小，也就是求最小的移动代价，不难发现，电脑之间的移动是环状的：

![graph.png](https://s2.loli.net/2025/07/26/ECcWBsuVIOLofJw.png)

贪心地想，走两步的情况相当于走两次一步，但是不能在中间的点进行操作，所以肯定是走一步更好。

考虑拓扑排序，和普通的拓扑排序不同的是，这个要分别对三台电脑开三个队列，如果当前这个队列空了，就继续操作下一个，直到三个队列都为空。

因为从不同的电脑开始会有不同的结果，所以要都试一遍，最后取答案最小值即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define double long double
#define bug cout<<"___songge888___"<<'\n'
using namespace std;
int n;
int a[222];

queue<int> q[8];
int in[422];

int sum;
vector<int> g[222];
int ans=1e18;
void topo(int op){
    while(!q[1].empty()||!q[2].empty()||!q[3].empty()){
        while(!q[op].empty()){
            int u=q[op].front();
            q[op].pop();
            for(auto v:g[u]){
                --in[v];
                if(in[v]==0){
                    q[a[v]].push(v);
                }
            }
        }
        sum++;
        op=(op==3)?1:op+1;
    }
    ans=min(ans,sum);
}
void init(){
    for(int i=0;i<=3;i++){
        q[i]=q[i+4];
    }
    for(int i=1;i<=n;i++){
        in[i]=in[i+n];
    }
    sum=0;
}
signed main(){
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    for(int i=1;i<=n;i++){
        int k;
        cin>>k;
        for(int j=1;j<=k;j++){
            int x;
            cin>>x;
            g[x].push_back(i);
            in[i]++;
            in[i+n]++;
        }
    }
    for(int i=1;i<=n;i++){
        if(in[i]==0){
            q[a[i]].push(i);
            q[a[i]+4].push(i);
        }
    }
    for(int num=1;num<=3;num++){
        topo(num);
        init();
    }
    cout<<ans+n<<'\n';
    return 0;
}

```

