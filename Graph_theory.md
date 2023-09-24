## 最小生成树

### Prim 算法

```C++
#include <bits/stdc++.h>

using namespace std;

#define INF 1000000 // 无穷大
#define MAXN 21     // 顶点个数最大值

int n, m;             // 顶点个数、边数
int Edge[MAXN][MAXN]; // 邻接矩阵
int lowcost[MAXN];
int nearvex[MAXN];

void prim(int u0)
{
    int i, j;
    int sumweight = 0;
    for (i = 1; i <= n; i++)
    {
        lowcost[i] = Edge[u0][i];
        nearvex[i] = u0;
    }
    nearvex[u0] = -1;
    for (i = 1; i < n; i++)
    {
        int min = INF;
        int v = -1;
        // 在 lowcost 数组（的 nearvex[] 值为 -1 的元素中）找最小值
        for (j = 1; j <= n; j++)
        {
            if (nearvex[j] != -1 && lowcost[j] < min)
            {
                v = j;
                min = lowcost[j];
            }
        }
        if (v != -1) // v == -1 表示没找到权值最小的边
        {
            // printf("%d %d %d\n", nearvex[v], v, lowcost[v]);
            nearvex[v] = -1;
            sumweight += lowcost[v];
            for (j = 1; j <= n; j++)
            {
                if (nearvex[j] != -1 && Edge[v][j] < lowcost[j])
                {
                    lowcost[j] = Edge[v][j];
                    nearvex[j] = v;
                }
            }
        }
    }
    printf("%d\n", sumweight);
}

int main()
{
    int i, j;
    int u, v, w;           // 边的起点和终点及权值
    scanf("%d%d", &n, &m); // 读入顶点个数 n 和边数 m
    memset(Edge, 0, sizeof(Edge));
    for (i = 1; i <= m; i++)
    {
        scanf("%d%d%d", &u, &v, &w); // 读入边的起点和终点
        Edge[u][v] = Edge[v][u] = w; // 构造邻接矩阵
    }
    for (i = 1; i <= n; i++)
    {
        for (j = 1; j <= n; j++)
        {
            if (i == j)
                Edge[i][j] = 0;
            else if (Edge[i][j] == 0)
                Edge[i][j] = INF;
        }
    }
    prim(1); // 从顶点 1 出发构造最小生成树
    return 0;
}
```

### Kruskal 算法

```C++
#include <bits/stdc++.h>

using namespace std;

#define MAXN 11 // 顶点个数的最大值
#define MAXM 20 // 边的个数的最大值

struct edge
{
    int u, v, w; // 边的顶点、权值
} edges[MAXM];   // 边的组数

int parent[MAXN]; // parent[i] 为顶点 i 所在集合对应的树中的根结点
int n, m;         // 顶点个数、边的个数
int i, j;         // 循环变量

void UFset() // 初始化
{
    for (i = 0; i < n; i++)
        parent[i] = -1;
}

int Find(int x) // 查找并返回结点 x 所属集合的根结点
{
    int s; // 查找位置
    for (s = x; parent[s] >= 0; s = parent[s])
        ;
    while (s != x) // 优化方案——压缩路径，使后续的查找操作加速
    {
        int tmp = parent[x];
        parent[x] = s;
        x = tmp;
    }
    return s;
}

// 将两个不同集合的元素进行合并，使两个集合中任意两个元素都联通
void Union(int R1, int R2)
{
    int r1 = Find(R1), r2 = Find(R2);  // r1 为 R1 的根结点
    int tmp = parent[r1] + parent[r2]; // 两个集合结点个数之和（负数）
    // 如果 R2 所在树结点个数 > R1 所在树结点个数（注意 parent[r1] 是负数）
    if (parent[r1] > parent[r2]) // 优化方案——加权法则
    {
        parent[r1] = r2;
        parent[r2] = tmp;
    }
    else
    {
        parent[r2] = r1;
        parent[r1] = tmp;
    }
}

/* int cmp(const void *a, const void *b) // 实现从小到大排序的比较函数
{
    edge aa = *(const edge *)a;
    edge bb = *(const edge *)b;
    return aa.w - bb.w;
} */
int cmp(edge aa, edge bb) // 实现从小到大排序的比较函数
{
    return aa.w < bb.w;
}

void Kruskal()
{
    int sumweight = 0; // 生成树的权值
    int num = 0;       // 已选用的边的数目
    int u, v;          // 选用边的两个顶点
    UFset();           // 初始化 parent 数组
    for (i = 0; i < m; i++)
    {
        u = edges[i].u;
        v = edges[i].v;
        if (Find(u) != Find(v))
        {
            printf("%d %d %d\n", u, v, edges[i].w);
            sumweight += edges[i].w;
            num++;
            Union(u, v);
        }
        if (num >= n - 1)
            break;
    }
    printf("%d\n", sumweight);
}

int main()
{
    int u, v, w;           // 边的起点和终点及权值
    scanf("%d%d", &n, &m); // 读入顶点个数 n
    for (i = 0; i < m; i++)
    {
        scanf("%d%d%d", &u, &v, &w); // 读入边的起点和终点
        edges[i].u = u;
        edges[i].v = v;
        edges[i].w = w;
    }
    // qsort(edges, m, sizeof(edges[0]), cmp); // 对边按权值从小到大排序
    sort(edges, edges + m, cmp); // 对边按权值从小到大排序
    Kruskal();
    return 0;
}
```

## 次小生成树

```C++
#include <bits/stdc++.h>
using namespace std;
#define mst(x, n) memset(x, n, sizeof(x));
int debug = 0;
typedef long long ll;
const int inf = 0x3f3f3f3f;
const ll maxn = 105;

int N, M;
struct Edge
{
    int u, v, w;
} edges[maxn * maxn];

int par[maxn], rak[maxn];
void init(int n)
{
    for (int i = 0; i <= n; i++)
        par[i] = i, rak[i] = 0;
}
int Find(int x)
{
    if (x == par[x])
        return x;
    else
        return par[x] = Find(par[x]);
}
bool isSame(int x, int y)
{
    return Find(x) == Find(y);
}

bool cmp(const Edge &a, const Edge &b)
{
    return a.w < b.w;
}
int maxd[maxn][maxn];
bool vis[maxn * maxn];
vector<int> mst[maxn];

void Kruskal()
{
    sort(edges + 1, edges + 1 + M, cmp);
    init(N);
    mst(maxd, 0);
    mst(mst, 0);
    mst(vis, 0);
    for (int i = 0; i <= N; ++i)
        mst[i].push_back(i);

    int sum = 0;
    for (int i = 1; i <= M; i++)
    {
        int u = Find(edges[i].u), v = Find(edges[i].v), w = edges[i].w;
        if (!isSame(u, v))
        {
            sum += w, vis[i] = true;
            for (int j = 0; j < mst[u].size(); ++j)
                for (int k = 0; k < mst[v].size(); ++k)
                    maxd[mst[u][j]][mst[v][k]] = maxd[mst[v][k]][mst[u][j]] = w;
            par[u] = v;
            for (int j = 0; j < mst[u].size(); ++j)
                mst[v].push_back(mst[u][j]);
        }
    }
    int csum = inf;
    for (int i = 1; i <= M; ++i)
        if (!vis[i])
            csum = min(csum, sum + edges[i].w - maxd[edges[i].u][edges[i].v]);
    printf("%d %d\n", sum, csum);
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T--)
    {
        scanf("%d%d", &N, &M);
        for (int i = 1; i <= M; ++i)
            scanf("%d%d%d", &edges[i].u, &edges[i].v, &edges[i].w);
        Kruskal();
    }
    return 0;
}
```
