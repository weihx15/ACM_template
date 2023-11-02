## 整数拆分

```C++
// HDU 4651
// 把数n 拆成几个数（小于等于n）相加的形式，问有多少种拆法。
const int MOD = 1e9 + 7;
int dp[100010];
void init()
{
    memset(dp, 0, sizeof(dp));
    dp[0] = 1;
    for (int i = 1; i <= 100000; i++)
    {
        for (int j = 1, r = 1; i - (3 * j * j - j) / 2 >= 0; j++, r *= -1)
        {
            dp[i] += dp[i - (3 * j * j - j) / 2] * r;
            dp[i] %= MOD;
            dp[i] = (dp[i] + MOD) % MOD;
            if (i - (3 * j * j + j) / 2 >= 0)
            {
                dp[i] += dp[i - (3 * j * j + j) / 2] * r;
                dp[i] %= MOD;
                dp[i] = (dp[i] + MOD) % MOD;
            }
        }
    }
}
int main()
{
    int T;
    int n;
    init();
    scanf("%d", &T);
    while (T--)
    {
        scanf("%d", &n);
        printf("%d\n", dp[n]);
    }
    return 0;
}
```

## 线性筛

```C++
const int MAX_SIEVE_NUM = 1e5 + 10;
bool is_prime[MAX_SIEVE_NUM];            // true 表示素数，false 表示非素数
int prime[MAX_SIEVE_NUM];                // 存储素数
void EulerPrime(int MAX = MAX_SIEVE_NUM) // 欧拉筛法
{
    mst(is_prime, true);
    is_prime[0] = is_prime[1] = false;
    int x = 0;
    for (int i = 2; i <= MAX; i++)
    {
        if (is_prime[i]) prime[x++] = i;
        for (int j = 0; j < x; j++)
        {
            if (i * prime[j] > MAX) break;
            is_prime[i * prime[j]] = false;
            if (i % prime[j] == 0) break;
        }
    }
}
```

## 最大公因数与最小公倍数

### 最大公约数

> 任何非零整数和零的最大公约数为它本身。

$\displaystyle \gcd(a,b)=\gcd(a-b,b)(a\geqslant b)$
$\displaystyle \gcd(a,b)=\gcd(a \,\text{mod}\, b,b )$
$\displaystyle \gcd(ka,kb)=k\,\gcd(a,b)$
$\displaystyle \gcd(a,b,c)=\gcd(\gcd(a,b),c)$
$\displaystyle \gcd(k,ab)=1\Longleftrightarrow\gcd(k,a)=1\,\&\&\,\gcd(k,b)=1$

```c++
int gcd(int a, int b)
{
    if (a == b)
        return a;
    if ((a & 1) == 0 && (b & 1) == 0)
        return gcd(a >> 1, b >> 1) << 1;
    else if ((a & 1) == 0 && (b & 1) != 0)
        return gcd(a >> 1, b);
    else if ((a & 1) != 0 && (b & 1) == 0)
        return gcd(a, b >> 1);
    else
    {
        int big = a > b ? a : b;
        int small = a < b ? a : b;
        return gcd(big - small, small);
    }
}
```

### 最小公倍数

$\displaystyle \forall a,b\in N,\gcd(a,b)\times\mathrm{lcm}(a,b)=a\times b$

## 费马小定理

若 $\displaystyle p$ 是质数，则对于任意的整数 $\displaystyle a$ 都有 $\displaystyle a^{p}\equiv a\pmod{p}$。若 $\displaystyle \gcd(a,p)=1$，即 $\displaystyle a$ 不是 $\displaystyle p$ 的倍数，则有 $\displaystyle a^{p-1}\equiv 1\pmod{p}$。

## 裴蜀（Bézout）定理

定理：设 $\displaystyle a,b$ 是不全为零的整数，存在无穷多组整数对 $\displaystyle (x,y)$，满足不定方程 $\displaystyle ax+by=d$，其中 $\displaystyle d=\gcd(a,b)$，即 $\displaystyle ax+by=\gcd(a,b)$

- 推论1：$\displaystyle \gcd(a,b)\mid c\Longleftrightarrow\exists x,y\in Z,ax+by=c$
- 推论2：$\displaystyle \forall a,b,z\in\mathbb{Z}^{*},\gcd(a,b)=1,\exists x,y\in\mathbb{Z},ax+by=ab-a-b+z$，即两互质的数 $\displaystyle a,b$，表示不出的最大的数为 $\displaystyle ab-a-b$ 。

## 扩展欧几里德算法

利用欧几里得算法求解 $\displaystyle ax+by=\gcd(a,b)$ 中的 $\displaystyle x$ 和 $\displaystyle y$ 。
若 $\displaystyle d\mid c$，不妨设 $\displaystyle c=kd$，则有 $\displaystyle a(kx)+b(ky)=c$，否则原方程无整数解。

```C++
// 在gcd的过程上增加了拓展
inline int exgcd(int a, int b, int &x, int &y)
{
    if (b == 0)
    {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, x, y);
    int z = x;
    x = y, y = z - y * (a / b);
    return d;
}
```

## 排列

### 全排列

`next_permutation()`

### 错位排列

$\displaystyle f[0]=1,f[1]=0,f[2]=1$
$\displaystyle f[n]=(n-1)*\left(f[n-1]+f[n-2]\right)$
