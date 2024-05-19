# 最小生成树算法


### (1) Prim算法

取图中任意一个顶点 v 作为生成树的根，之后往生成树上添加新的顶点 w。在添加的顶点 w 和已经在生成树上的顶点v 之间必定存在一条边，并且该边的权值在所有连通顶点 v 和 w 之间的边中取值最小。之后继续往生成树上添加顶点，直至生成树上含有 n 个顶点为止。

<!--more-->

该算法和Dijkstra算法有点类似，在生成树的构造过程中，图中 n 个顶点分属两个集合：已落在生成树上的顶点集 X 和尚未落在生成树上的顶点集V-X ，则应在所有连通X中顶点和V-X中顶点的边中选取权值最小的边。

### (二) 代码演示

```cpp
#include<cstring>
#include <algorithm>
#include <stdio.h>
#include <vector>
using namespace std;

const int INF = INT_MAX;

const int MAX_V = 105;//最大顶点数，可以自己改

int cost[MAX_V][MAX_V]; //表示u,v之间的权值

int mincost[MAX_V]; //表示从集合x出发到每个顶点最小权值

bool used[MAX_V]; //表示顶点是否在集合x中

int V; //顶点数

int prim()
{
    for (int i = 0; i < V; i++)
    {
        mincost[i] = INF;
        used[i] = false;
    }
    mincost[0] = 0;
    int res = 0;
    while (true)
    {
        int v = -1;
        //从不属于集合x的顶点中选取从x到权值最小的顶点
        for (int u = 0; u < V; u++)
        {
            if (!used[u] && (v == -1 || mincost[u] < mincost[v]))
                v = u;
        }
        if (v == -1)
            break;
        used[v] = true;//把顶点v加入集合x
        res += mincost[v];//把边的长度加到结果
        for (int u = 0; u < V; u++)
        {
            mincost[u] = min(mincost[u], cost[u][v]);
        }
    }
    return res;
}

```

### Kruskal算法

Kruskal算法相对更简单，按照边权值从小到大排序即可，依次添加，如果产生环，那么不添加该边，使用并查集即可判断。

