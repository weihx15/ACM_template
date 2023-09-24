```C++
// #pragma GCC optimize(2)
// #pragma G++ optimize(2)
#include <bits/stdc++.h>

using namespace std;

typedef double lf;
typedef long long ll;
typedef long double llf;
typedef pair<int, int> pii;
typedef unsigned long long ull;

const lf pi = acos(-1);
const int inf = INT_MAX >> 1;
const ll INF = INT64_MAX >> 1;
const int mod = (1 ? 1000000007 : 998244353);
const int dx[8] = {0, 0, 1, -1, 1, 1, -1, -1}, dy[8] = {1, -1, 0, 0, 1, -1, 1, -1};

#define endl "\n"
#define len(a) (a).size()
#define gcd(a, b) __gcd((a), (b))
#define lcm(a, b) (a) / gcd(a, b) * (b)
#define mst(a, b) memset(a, b, sizeof a)
#define debug(a) cout << #a << " = " << a << "\n"
#define sync std::ios::sync_with_stdio(false);cin.tie(0)

template <class T> vector<T> &operator<<(vector<T> &a, const T &b) { a.push_back(b); return a; }
template <typename T> inline void read(T &t) {
    int f = 0, c = getchar(); t = 0;
    while (!isdigit(c)) f |= c == '-', c = getchar();
    while (isdigit(c)) t = t * 10 + c - 48, c = getchar();
    if (f) t = -t;
}
template <typename T> void print(T x) { 
    if (x < 0) x = -x, putchar('-');
    if (x > 9) print(x / 10);
    putchar(x % 10 + 48);
}
int mul(int a, int b, int m = mod) { return 1ll * a * b % m; }
template <class T> int qpow(int a, T b, int m = mod) {
    int ans = 1;
    for (; b; a = mul(a, a, m), b >>= 1) if (b & 1)
        ans = mul(ans, a, m);
    return ans;
}

int main()
{
    

    return 0;
}
```
