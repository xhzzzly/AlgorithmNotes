# ST算法

ST算法（区间最值，区间LCD）



## 作用

区间最大值或最小值查询、区间最大公约数查询



## 原理

以倍增法为基础，构造数组 `st[20][N]`，注意此处第一维并不需要很大，因为是以2的次方倍增，开太大还会爆。$st_{i,j}$ 表示的是数组下标从 j 开始，长度为 $2^i$ 的区间，存放的是区间内的最大/最小/相应的值



## 代码示范

```cpp
// For only one case
// #pragma GCC optimize(2)
#include <bits/stdc++.h>
#define rep(a, b, c) for (int a = (b); a <= (c); ++a)
#define per(a, b, c) for (int a = (b); a >= (c); --a)
#define ft first
#define sd second
#define pb push_back
#define eb emplace_back
#define all(x) x.begin(), x.end()
#define all2(x) x.begin() + 1, x.end()
#define chmin(a, b) if (a > (b)) a = (b)
#define chmax(a, b) if (a < (b)) a = (b)
#define lowbit(x) ((x) & -(x))
using namespace std;
using pii = pair<int, int>;
using ll = long long;

constexpr int N = 1e5 + 3;
int st[20][N];

ll pw2[20];
int num2;

void init(int n) {
    pw2[0] = 1;
    num2 = 1;
    while (true) {
        pw2[num2] = pw2[num2 - 1] * 2;
        if (pw2[num2] > n) break;
        ++num2;
    }
}

int l2(int val) {
    int l = 0, r = num2;
    while (l < r) {
        int mid = (l + r + 1) >> 1;
        if (pw2[mid] > val) r = mid - 1;
        else l = mid;
    }
    return l;
}

void solve() {
    int n, m;
    cin >> n >> m;
    init(n);
    rep(i, 1, n) cin >> st[0][i];
    int wdth = 2, pw = 1;
    while (wdth <= n) {
        rep(i, 1, n - wdth + 1) st[pw][i] = max(st[pw - 1][i], st[pw - 1][i + wdth / 2]);
        wdth *= 2;
        ++pw;
    }
    rep(_, 1, m) {
        int l, r;
        cin >> l >> r;
        pw = l2(r - l + 1);
        cout << max(st[pw][l], st[pw][r - pw2[pw] + 1]) << "\n";
    }
}

signed main() {
    ios::sync_with_stdio(false); cin.tie(nullptr); cout.tie(nullptr);

    int T = 1;

    // cin >> T;

    while (T--) {
        solve();
    }
    return 0;
}
```

