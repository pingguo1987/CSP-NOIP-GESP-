## 题目80：P2865 [USACO06NOV] Roadblocks G

[P2865 [USACO06NOV] Roadblocks G](https://www.luogu.com.cn/problem/P2865)

## 题面翻译

贝茜把家搬到了一个小农场，但她常常回到 FJ 的农场去拜访她的朋友。贝茜很喜欢路边的风景，不想那么快地结束她的旅途，于是她每次回农场，都会选择第二短的路径，而不象我们所习惯的那样，选择最短路。  
贝茜所在的乡村有 $R(1\le R \le 10^5)$ 条双向道路，每条路都联结了所有的 $N(1\le N\le 5000)$ 个农场中的某两个。贝茜居住在农场 $1$，她的朋友们居住在农场 $N$（即贝茜每次旅行的目的地）。  
贝茜选择的第二短的路径中，可以包含任何一条在最短路中出现的道路，并且，一条路可以重复走多次。当然咯，第二短路的长度必须严格大于最短路（可能有多条）的长度，但它的长度必须不大于所有除最短路外的路径的长度。

## 题目描述

Bessie has moved to a small farm and sometimes enjoys returning to visit one of her best friends. She does not want to get to her old home too quickly, because she likes the scenery along the way. She has decided to take the second-shortest rather than the shortest path. She knows there must be some second-shortest path.

The countryside consists of R (1 ≤ R ≤ 100,000) bidirectional roads, each linking two of the N (1 ≤ N ≤ 5000) intersections, conveniently numbered 1..N. Bessie starts at intersection 1, and her friend (the destination) is at intersection N.

The second-shortest path may share roads with any of the shortest paths, and it may backtrack i.e., use the same road or intersection more than once. The second-shortest path is the shortest path whose length is longer than the shortest path(s) (i.e., if two or more shortest paths exist, the second-shortest path is the one whose length is longer than those but no longer than any other path).

## 输入格式

Line 1: Two space-separated integers: N and R


Lines 2..R+1: Each line contains three space-separated integers: A, B, and D that describe a road that connects intersections A and B and has length D (1 ≤ D ≤ 5000)

## 输出格式

Line 1: The length of the second shortest path between node 1 and node N

## 样例 #1

### 样例输入 #1

```
4 4
1 2 100
2 4 200
2 3 250
3 4 100
```

### 样例输出 #1

```
450
```



## 提示

Two routes: 1 -> 2 -> 4 (length 100+200=300) and 1 -> 2 -> 3 -> 4 (length 100+250+100=450)

**思路分析**

` Dijkstra` 算法求解最短路 和次短路

更新最短之前，次短路 = 当前最短路，然后更新最短路。

如果新的路径大于最短路，小于次短路，那么，只需要更新次短路。

**程序代码**

```c++
//https://www.luogu.com.cn/problem/P2865
// P2865 [USACO06NOV] Roadblocks G
// #include<iostream>
// #include<queue>
// #include<vector>
// #include<list>
#include<bits/stdc++.h>
using namespace std;

class cmp{
    public:
        bool operator()(const pair<int,int> &a,const pair<int,int>&b)
        {
            return a.second > b.second;
        }
};


int main()
{
    int n,r;
    cin>>n>>r;

    vector<list<pair<int,int>>> graph(n+1);
    int u,v,w;

    // 使用邻接表存图
    for(int i=0;i<r;i++)
    {
        cin>>u>>v>>w;
        graph[u].push_back({v,w});
        graph[v].push_back({u,w});
    }

    // 使用dijkstra 算法求最短路和次短路
    priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> pqu;

    // 定时最短距离
    vector<int> minDist(n+1,INT_MAX);
    // 定时次短路
    vector<int> secondDist(n+1,INT_MAX);

    // 从1 到N 的次短路
    minDist[1] = 0;
    pqu.push({1,0});

    while(!pqu.empty())
    {
        pair<int,int> cur = pqu.top();
        pqu.pop();
        // 如果到当前节点的最小值 大于 当前节点最小值，说明已经在有其它节点把这个secondDist[cur.first] 更新过
        if(cur.second > secondDist[cur.first] && cur.second > minDist[cur.first]) continue;

        int curx = cur.first;
        int curc = cur.second;

        for(const auto & g : graph[cur.first])
        {
            // 从cur.first 到的点
            int x = g.first;
            // 从cur.fist 到点x 需要的长度
            int c = g.second;
            if(curc + c < minDist[x])
            {
                // 最短路变成次短路
                secondDist[x] = minDist[x];
                // 最短路更新
                minDist[x] = curc + c;
                pqu.push({x,minDist[x]});
            }
            // 严格次短路 curc + c > minDist[x]
            // 非严格次短路 curc + c >= minDist[x]
            if(curc + c < secondDist[x] && curc + c > minDist[x])
            {
                // 更新次短路
                secondDist[x] = curc + c;
                pqu.push({x,secondDist[x]});
            }
        }
    }

    cout<<secondDist[n]<<endl;
    return 0;
}
```

**复杂度**

**时间复杂度：** `O(mlog(m))` ，`m` 为边数

**空间复杂度：** `O(m)` 

更多相关知识：https://github.com/pingguo1987/CSP-NOIP-GESP-

---

欢迎关注我的公众号**wangsir 聊信息学**，原创技术文章第一时间推送。

<center>
    <img src="https://cdn.jsdelivr.net/gh/pingguo1987/CSP-NOIP-GESP-/image/pic/公众号-扫码版.png">
</center>
