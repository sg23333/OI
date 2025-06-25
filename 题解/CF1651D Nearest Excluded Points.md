> 2024-11-20

# CF1651D Nearest Excluded Points

### 题意

给定 $n$ 个点 $(x,y)$，求每个点周围距离与这个点的哈夫曼距离最小的点（不包括这 $n$ 个点）。

### 思路

如图：

![](https://cdn.luogu.com.cn/upload/image_hosting/l5g4tpyz.png)

对于每个询问中的点，可以分成两种情况：

- 周围有不是询问中的点

以点 $E$ 为例，在它的周围有 $(1,3)$，$(2,4)$，$(3,3)$，将点 $E$ 的答案设为其中的一个点即可，即：
若一个点周围有不是询问中的点，这个点的答案为它周围其中一个不是询问中的点。

- 周围都是询问中的点

以点 $A$ 为例，它周围无非询问的点，于是向外搜索，搜到一个点（例如点 $E$ ），搜出来的点如果已经有答案了，则原本的点的答案就是搜出来的点的答案，即：
若一个点周围都是询问中的点，这个点的答案为它周围其中一个的点的答案。

知道了思路，接下来想想可以怎么做，对于这 $n$ 个点，可以先考虑第一种情况，开一个 `vis` 数组，如果这个点周围有空白的点，直接在这个点上打上标记，再把这个点丢进一个队列中。
接下来考虑第二种情况，遍历队列中的每一个点，如果队列中的点周围有要询问的点并且那个点没有被标记，则那个点的答案就是当前队列里这个点的答案，更新完答案后打上标记，将刚刚标记的点丢进队列中，然后一直循环这个操作，用 **BFS** 就可以了。

注意数据范围 $n \le 2 \times 10^5$，所以存图时要用 `map`。

时间复杂度：$O(n\log n)$

### Code

```c++
#include<bits/stdc++.h>
#define bug cout<<"!!!!!!!!_bug_!!!!!!!!!"<<'\n';
#define int long long
using namespace std;
struct node{
	int x,y;
}a[200010],ans[200010];
map<pair<int,int>,int> mp;
queue<int> q;
int n;
int dx[]={0,0,1,-1};
int dy[]={1,-1,0,0};
int vis[200010];
void bfs(){
	while(!q.empty()){
		int now=q.front();
		q.pop();
		for(int i=0;i<4;i++){
			int xx=a[now].x+dx[i];
			int yy=a[now].y+dy[i];
			if(mp[{xx,yy}]&&!vis[mp[{xx,yy}]]){
				ans[mp[{xx,yy}]]=ans[now];
				vis[mp[{xx,yy}]]++;
				q.push(mp[{xx,yy}]);
			}
		}
	}
    return ;
}
signed main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i].x>>a[i].y;
		mp[{a[i].x,a[i].y}]=i;
	}
	for(int i=1;i<=n;i++){//第一种情况
		for(int j=0;j<4;j++){
			int xx=a[i].x+dx[j],yy=a[i].y+dy[j];
			if(!mp[{xx,yy}]){
				ans[i]={xx,yy};
				vis[i]++;
				q.push(i);
				break;
			}
		} 
	}
	bfs();//第二种情况
	for(int i=1;i<=n;i++){
		cout<<ans[i].x<<' '<<ans[i].y<<'\n';
	}
	return 0;
}

```

[提交记录](https://codeforces.com/contest/1651/submission/292470852)