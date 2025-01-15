## 题目64：Shichikuji and Power Grid

[Shichikuji and Power Grid](https://vjudge.net/problem/CodeForces-1245D#author=GPT_zh)

## 题面翻译

已知一个平面上有 $n$ 个城市，需要个 $n$ 个城市均通上电。  
一个城市有电，必须在这个城市有发电站或者和一个有电的城市用电缆相连。

在一个城市建造发电站的代价是 $c[i]$，将 $i$ 和 $j$ 两个城市用电缆相连的代价是 $k[i]+k[j]$ 乘上两者的曼哈顿距离。

求最小代价的方案。

## 题目描述

Shichikuji is the new resident deity of the South Black Snail Temple. Her first job is as follows:

There are $ n $ new cities located in Prefecture X. Cities are numbered from $ 1 $ to $ n $ . City $ i $ is located $ x_i $ km North of the shrine and $ y_i $ km East of the shrine. It is possible that $ (x_i, y_i) = (x_j, y_j) $ even when $ i \ne j $ .

Shichikuji must provide electricity to each city either by building a power station in that city, or by making a connection between that city and another one that already has electricity. So the City has electricity if it has a power station in it or it is connected to a City which has electricity by a direct connection or via a chain of connections.

- Building a power station in City $ i $ will cost $ c_i $ yen;
- Making a connection between City $ i $ and City $ j $ will cost $ k_i + k_j $ yen per km of wire used for the connection. However, wires can only go the cardinal directions (North, South, East, West). Wires can cross each other. Each wire must have both of its endpoints in some cities. If City $ i $ and City $ j $ are connected by a wire, the wire will go through any shortest path from City $ i $ to City $ j $ . Thus, the length of the wire if City $ i $ and City $ j $ are connected is $ |x_i - x_j| + |y_i - y_j| $ km.

Shichikuji wants to do this job spending as little money as possible, since according to her, there isn't really anything else in the world other than money. However, she died when she was only in fifth grade so she is not smart enough for this. And thus, the new resident deity asks for your help.

And so, you have to provide Shichikuji with the following information: minimum amount of yen needed to provide electricity to all cities, the cities in which power stations will be built, and the connections to be made.

If there are multiple ways to choose the cities and the connections to obtain the construction of minimum price, then print any of them.

## 输入格式

First line of input contains a single integer $ n $ ( $ 1 \leq n \leq 2000 $ ) — the number of cities.

Then, $ n $ lines follow. The $ i $ -th line contains two space-separated integers $ x_i $ ( $ 1 \leq x_i \leq 10^6 $ ) and $ y_i $ ( $ 1 \leq y_i \leq 10^6 $ ) — the coordinates of the $ i $ -th city.

The next line contains $ n $ space-separated integers $ c_1, c_2, \dots, c_n $ ( $ 1 \leq c_i \leq 10^9 $ ) — the cost of building a power station in the $ i $ -th city.

The last line contains $ n $ space-separated integers $ k_1, k_2, \dots, k_n $ ( $ 1 \leq k_i \leq 10^9 $ ).

## 输出格式

In the first line print a single integer, denoting the minimum amount of yen needed.

Then, print an integer $ v $ — the number of power stations to be built.

Next, print $ v $ space-separated integers, denoting the indices of cities in which a power station will be built. Each number should be from $ 1 $ to $ n $ and all numbers should be pairwise distinct. You can print the numbers in arbitrary order.

After that, print an integer $ e $ — the number of connections to be made.

Finally, print $ e $ pairs of integers $ a $ and $ b $ ( $ 1 \le a, b \le n $ , $ a \ne b $ ), denoting that a connection between City $ a $ and City $ b $ will be made. Each unordered pair of cities should be included at most once (for each $ (a, b) $ there should be no more $ (a, b) $ or $ (b, a) $ pairs). You can print the pairs in arbitrary order.

If there are multiple ways to choose the cities and the connections to obtain the construction of minimum price, then print any of them.

## 样例 #1

### 样例输入 #1

```
3
2 3
1 1
3 2
3 2 3
3 2 3
```

### 样例输出 #1

```
8
3
1 2 3 
0
```

## 样例 #2

### 样例输入 #2

```
3
2 1
1 2
3 3
23 2 23
3 2 3
```

### 样例输出 #2

```
27
1
2 
2
1 2
2 3
```

## 提示

For the answers given in the samples, refer to the following diagrams (cities with power stations are colored green, other cities are colored blue, and wires are colored red):

<img src ="https://cdn.jsdelivr.net/gh/pingguo1987/CSP-NOIP-GESP-/image/pic/图论/图论_题目64：Shichikuji and Power Grid/251d50cda099f4e4be2994b6b01574cc32a17cd3.png" />

For the first example, the cost of building power stations in all cities is $ 3 + 2 + 3 = 8 $ . It can be shown that no configuration costs less than 8 yen.

For the second example, the cost of building a power station in City 2 is 2. The cost of connecting City 1 and City 2 is $ 2 \cdot (3 + 2) = 10 $ . The cost of connecting City 2 and City 3 is $ 3 \cdot (2 + 3) = 15 $ . Thus the total cost is $ 2 + 10 + 15 = 27 $ . It can be shown that no configuration costs less than 27 yen.



**思路分析**

最小生成树

**巧妙转换**

我们可以假设只有一个额外地点建电站，将这个电站假设为`n+1`号城市，将`n+1`号城市与前`n`个城市连线，线的权值为花费的代价`c[i]`。

使用``kruskal`跑最小生成树，如果最小生成树中选中与`n+1`节点连的边，说明要在`n+1`点的对端点城市建立发电站。

**程序代码**

```c++
// https://www.luogu.com.cn/problem/CF1245D
// Shichikuji and Power Grid
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

const int N = 2005;
struct Node{
    int x;
    int y;
};

Node node[N];
int c[N];
int k[N];


struct Edge{
    int from;
    int to;
    long long val;
    Edge(int f,int t,long long v) : from(f),to(t),val(v){}
};

// 从小到大排序
bool cmp(const Edge &a,const Edge &b)
{
    return a.val < b.val;
}

int father[N];

void init()
{
    for(int i=0;i<N;i++)
    {
        father[i] = i;
    }
}

int find(int v)
{
    if(v != father[v])
    {
        father[v] = find(father[v]);
    }
    return father[v];
}

bool issame(int v,int u)
{
    v = find(v);
    u = find(u);

    return v == u;
}

void join(int v,int u)
{
    v = find(v);
    u = find(u);

    if(v != u)
    {
        father[v] = u;
    }
}

int main()
{
    int n;
    cin>>n;

    // 读数据
    for(int i=1;i<=n;i++)
    {
        cin>>node[i].x;
        cin>>node[i].y;
    }

    for(int i=1;i<=n;i++)
    {
        cin>>c[i];
    }

    for(int i=1;i<=n;i++)
    {
        cin>>k[i];
    }

    // 构建边
    vector<Edge> edge;
    for(int i=1;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            long long dis = 1ll*(abs(node[i].x - node[j].x) + abs(node[i].y - node[j].y)) * (k[i] + k[j]);
            Edge cur(i,j,dis);
            edge.push_back(cur);
        }
    }
    // 将建立发电站的位置抽象成一个点，使这些点最终变成n+1个节点
    for(int i=1;i<=n;i++)
    {
        Edge last(n+1,i,c[i]);
        edge.push_back(last);
    }

    sort(edge.begin(),edge.end(),cmp);
    // 初始化并查集
    init();

    // 最小花费
    long long ans = 0;
    // 建立发电站的位置
    vector<int> station;

    //铺设电缆的城市
    vector<Edge> cityedge;

    for(const auto & e : edge)
    {
        if(!issame(e.from,e.to))
        {
            join(e.from,e.to);
            // 这是建立发电站的位置
            if(e.from == n+1)
            {
                station.push_back(e.to);
            }else
            {
                cityedge.push_back(e);
            }
            ans += e.val;
        }
    }
    // 打印花费
    cout<<ans<<endl;
    // 打印发电站个数及其位置
    cout<<station.size()<<endl;
    for(int i=0;i<station.size();i++)
    {
        cout<<station[i]<<" ";
    }
    cout<<endl;

    // 打印铺设电缆的个数
    cout<<cityedge.size()<<endl;
    for(int i=0;i<cityedge.size();i++)
    {
        cout<<cityedge[i].from<<" "<<cityedge[i].to<<endl;
    }

    return 0;
}
```

**复杂度**

**时间复杂度：** `O(eloge)` e 为边的数量 ，本地中`e==n^2`

**空间复杂度：** `O(n^2)`  

更多相关知识：https://github.com/pingguo1987/CSP-NOIP-GESP-

---

欢迎关注我的公众号**wangsir 聊信息学**，原创技术文章第一时间推送。

<center>
    <img src="https://cdn.jsdelivr.net/gh/pingguo1987/CSP-NOIP-GESP-/image/pic/公众号-扫码版.png">
</center>
