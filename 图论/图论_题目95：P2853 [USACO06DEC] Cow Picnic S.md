## 题目95：P2853 [USACO06DEC] Cow Picnic S

## 题目描述

The cows are having a picnic! Each of Farmer John's K (1 ≤ K ≤ 100) cows is grazing in one of N (1 ≤ N ≤ 1,000) pastures, conveniently numbered 1...N. The pastures are connected by M (1 ≤ M ≤ 10,000) one-way paths (no path connects a pasture to itself).

The cows want to gather in the same pasture for their picnic, but (because of the one-way paths) some cows may only be able to get to some pastures. Help the cows out by figuring out how many pastures are reachable by all cows, and hence are possible picnic locations.

$K(1 \le K \le 100)$ 只奶牛分散在 $N(1 \le N \le 1000)$ 个牧场．现在她们要集中起来进餐。牧场之间有 $M(1 \le M \le 10000)$ 条有向路连接，而且不存在起点和终点相同的有向路．她们进餐的地点必须是所有奶牛都可到达的地方。那么，有多少这样的牧场可供进食呢？

## 输入格式

Line 1: Three space-separated integers, respectively: K, N, and M


Lines 2..K+1: Line i+1 contains a single integer (1..N) which is the number of the pasture in which cow i is grazing.


Lines K+2..M+K+1: Each line contains two space-separated integers, respectively A and B (both 1..N and A != B), representing a one-way path from pasture A to pasture B.

## 输出格式

Line 1: The single integer that is the number of pastures that are reachable by all cows via the one-way paths.

## 样例 #1

### 样例输入 #1

```
2 4 4
2
3
1 2
1 4
2 3
3 4
```

### 样例输出 #1

```
2
```

## 提示

The cows can meet in pastures 3 or 4.

<img src ="https://cdn.jsdelivr.net/gh/pingguo1987/CSP-NOIP-GESP-/image/pic/图论/图论_题目95：P2853 [USACO06DEC] Cow Picnic S/image-20250325122213164.png" />

**思路分析**

**暴力枚举**

`k` 的最大值为`100`，节点个数最大值`1000` ，边的个数最大值`10000`  ，从每个奶牛的初始位置出发进行`dfs`, 一共跑`K` 次。最后统计每个节点经过的次数，如果次数大于等于`K` 说明这个节点是合法的。

**程序代码**

```c++
// https://www.luogu.com.cn/problem/P2853
// P2853 [USACO06DEC] Cow Picnic S
#include<iostream>
#include<list>
#include<vector>
#include<cstring>
using namespace std;

const int K = 105;
const int N = 1005;

// 奶牛的初始位置所在
int loc[K];
// 记录节点被访问的次数
int count1[N];
// dfs时记录节点是否被访问
bool vis[N];
// 邻接表建图
vector<list<int>> graph(N);
// 返回值，记录合法的个数
int cnt = 0;

void dfs(int num)
{
    if(!vis[num])
    {
        vis[num] = true;
        count1[num] ++;
        for(const auto & g:graph[num])
        {
            dfs(g);
        }
    } 

}

int main()
{
    int k,n,m;
    cin>>k>>n>>m;

    for(int i=0;i<k;i++)
    {
        cin>>loc[i];
    }
    int a,b;
    for(int i=1;i<=m;i++)
    {
        cin>>a>>b;
        graph[a].push_back(b);
    }

    // 对所有的k 跑dfs,看看从当前k 开始能经过哪些k,找出哪些所有k都经过的节点
    for(int i=0;i<k;i++)
    {
        memset(vis,false,sizeof(vis));
        dfs(loc[i]);
    }

    for(int i=1;i<=n;i++)
    {
        if(count1[i] >= k)
        {
            cnt++;
        }
    }
    cout<<cnt<<endl;
    return 0;
}

```

**复杂度**

**时间复杂度：** `O(k*m)`  `m` 为边数,`k` 代表奶牛数

**空间复杂度：** `O(n*m)` 

更多相关知识：https://github.com/pingguo1987/CSP-NOIP-GESP-

---

欢迎关注我的公众号**wangsir 聊信息学**，原创技术文章第一时间推送。

<center>
    <img src="https://cdn.jsdelivr.net/gh/pingguo1987/CSP-NOIP-GESP-/image/pic/公众号-扫码版.png">
</center>
