
## 汉诺塔公式

### 三柱汉诺塔

$H_{3}=2^{n}-1$

### 限制三柱汉诺塔

- 禁止隔柱移动，从A->C

$H_{3}=3^{n}-1$

- 只能顺时针转，从A->B
$H(n)=\frac{1}{6}\left(-\left(-\sqrt{3}-3\right)\left(1-\sqrt{3}\right)^{n}+\left(\sqrt{3}+3\right)\left(\sqrt{3}+1\right)^{n}-6\right)$

- 随机汉诺塔，从任意一个状态到随机另一个状态的期望步数...

$H_{R}(n)=\frac{466}{885}\cdot 2^{n}-\frac{1}{3}-\frac{3}{5}\cdot \left(\frac{1}{3}\right)^{n}+\left(\frac{12}{59}+\frac{18}{1003}\sqrt{17}\right)\left(\frac{5+\sqrt{17}}{18}\right)^{n}+\left(\frac{12}{59}-\frac{18}{1003}\sqrt{17}\right)\left(\frac{5-\sqrt{17}}{18}\right)^{n}$
### 四柱汉诺塔

$H_{4}=1-2^{t-2}\left(t^{2}-3t-2n+4\right)$
其中，$t=\left\lfloor\sqrt{2n}\right\rceil$

## C++ 取整函数

- ceil() 向上取整
- floor() 向下取整
- round() 四舍五入

## DFS

```C++
function dfs(当前状态, 一系列其他的状态量){
	if(当前状态 == 目的状态){
        ···
    }
    for(···寻找新状态){
        if(状态合法){
            vis[访问该点]；
            dfs(新状态);
            ?是否需要恢复现场->vis[恢复访问]
        } 
    }
    if(找不到新状态){
        是否需要创建新规则？{
            创建并对当前状态进行访问vis;
            继续搜索;
            恢复现场/恢复访问vis;
        }
    }
}
```

## BFS

```C++
void bfs(起始点)
{
	将起始点放入队列中;
	标记起点已访问;
	while(队列不为空)
	{
		访问队列中首元素x;
		删除队首元素;
		for(x所有相邻点)
		{
			if(该点未被访问过且合法)
				将该点加入队列末尾; 
		}
	}
	队列为空，广搜结束; 
}
```

## 进制转换

```C++
#include <cstdio>
const int N = 1e5 + 3;
int n, m, x, tmp, ansl;
char a[N], ans[N];
inline int n_to_ten()
{
    int x = 0;
    for (int i = 0; a[i]; ++i)
    {
        x *= n;
        if (a[i] >= 'A' && a[i] <= 'F')
            x += (a[i] - 'A' + 10);
        else
            x += (a[i] - '0');
    }
    return x;
}
inline void ten_to_m()
{
    do
    {
        tmp = x % m;
        if (tmp < 10)
            ans[++ansl] = '0' + tmp;
        else
            ans[++ansl] = 'A' + tmp - 10;
        x /= m;
    } while (x);
}
int main()
{
    scanf("%d%s%d", &n, a, &m);
    x = n_to_ten();
    ten_to_m();
    for (int i = ansl; i; --i)
        printf("%c", ans[i]);
    puts("");
}
```

## bitset

```C++
#include <iostream>
#include <bitset>
#include <cstring>
using namespace std;
int main()
{
    bitset<23> bit(string("11101001"));
    cout << bit << endl;
    bit = 233;
    cout << bit << endl;
    return 0;
}

//对于一个叫做bit的bitset：
bit.size()      // 返回大小（位数）
bit.count()     // 返回1的个数
bit.any()       // 返回是否有1
bit.none()      // 返回是否没有1
bit.set()       // 全都变成1
bit.set(p)      // 将第p + 1位变成1（bitset是从第0位开始的！）
bit.set(p, x)   // 将第p + 1位变成x
bit.reset()     // 全都变成0
bit.reset(p)    // 将第p + 1位变成0
bit.flip()      // 全都取反
bit.flip(p)     // 将第p + 1位取反
bit.to_ulong()  // 返回它转换为unsigned long的结果
bit.to_ullong() // 返回它转换为unsigned long long的结果
bit.to_string() // 返回它转换为string的结果
```

## ST 表

```C++
ll n, m, f[N][21], a[N];
inline void ST()
{
    over(i, 1, n)
        f[i][0] = a[i];                                               // 初始化,以自己为起点2^0=1长的区间就是他自己
    for (ll j = 1; (1 << j) <= n; ++j)                                // 枚举区间长度
        for (ll i = 1; i + (1 << j) - 1 <= n; ++i)                    // 枚举起点
            f[i][j] = max(f[i][j - 1], f[i + (1 << (j - 1))][j - 1]); // 取最大值
}
inline ll query(ll l, ll r)
{
    ll k = trunc(log2(r - l + 1));
    return max(f[l][k], f[r - (1 << k) + 1][k]); // 因为已经用区间DP初始化过了，所以直接比较输出即可
}
int main()
{
    scanf("%lld%lld", &n, &m);
    over(i, 1, n) scanf("%lld", &a[i]);
    ST();
    over(i, 1, m)
    {
        ll x, y;
        scanf("%lld%lld", &x, &y);
        printf("%lld\n", query(x, y));
    }
    return 0;
}
```

## 并查集

```C++
int find(int x)
{
    return p[x] == x ? p[x] : p[x] = find(p[x]);
}

void merge(int x, int y)
{
    int a = find(x);
    int b = find(y);
    if (a != b)
        p[a] = b;
}
```

## 背包

### 01背包

```C++
#include <algorithm>
#include <cstdio>
int T, n, i, j, v[110], w[110], f[1010];
int main()
{
    scanf("%d%d", &T, &n);
    for (i = 1; i <= n; i++)
        scanf("%d%d", &v[i], &w[i]);
    for (i = 1; i <= n; i++)
        for (j = T; j >= v[i]; j--)
            f[j] = std::max(f[j], f[j - v[i]] + w[i]);
    printf("%d", f[T]);
}
```

### 完全背包

```C++
#include <bits/stdc++.h>
using namespace std;
int T, n, ans, a[105], dp[30005];
int main()
{
    scanf("%d", &T);
    while (T--)
    {
        memset(dp, -127, sizeof(dp));
        scanf("%d", &n);
        dp[0] = ans = 0;
        for (int i = 1; i <= n; i++)
            scanf("%d", &a[i]);
        for (int i = 1; i <= n; i++)
            for (int j = a[i]; j <= 25000; j++)
                dp[j] = max(dp[j], dp[j - a[i]] + 1);
        for (int i = 1; i <= n; ++i)
            ans += dp[a[i]] == 1;
        printf("%d\n", ans);
    }
}
```

## 进制转换

### 任意2-36进制数转化为10进制数

```C++
int Atoi(string s,int radix)
{
    int ans=0;
    for(int i=0;i<s.size();i++)
    {
        char t=s[i];
        if(t>='0'&&t<='9') ans=ans*radix+t-'0';
        else ans=ans*radix+t-'a'+10;  //输入为小写字母时
    }
    return ans;
}
```

### 将10进制数转换为任意的n进制数,结果为字符串类型

```C++
#include <algorithm>
string intToA(int n,int radix)
{
    string ans="";
    do{
        int t=n%radix;
        if(t>=0&&t<=9) ans+=(t+'0');
        else ans+=(t-10+'a');
        n/=radix;
    }while(n!=0);  //使用do{}while()以防止输入为0的情况
    reverse(ans.begin(),ans.end());
    return ans;
}
```

### itoa()函数（可以将一个10进制数转换为任意的2-36进制字符串）

```C++
#include<cstdio> 
#include<cstdlib> 
int main()  
{  
    int num = 10;  
    char str[100];  
    _itoa(num, str, 2);  //c++中一般用_itoa，用itoa也行,
    printf("%s\n", str);  
    return 0;  
}
```