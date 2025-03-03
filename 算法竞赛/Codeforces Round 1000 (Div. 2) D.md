```c++
#include<bits/stdc++.h>
using namespace std;

#define int long long

typedef pair<int, int> pii;
typedef long long LL;
typedef unsigned long long uLL;
typedef __int128_t i128;

const int inf32 = 1e9;
const LL inf64 = 1e18;

signed main()
{
    cin.tie(0); cout.tie(0);
    ios::sync_with_stdio(0);

    int T = 1;
    cin >> T;
    while (T--) {
        int n, m;
        cin >> n >> m;
        vector<int> a(n + 5, 0), b(m + 5, 0);
        for (int i = 1; i <= n; i++) cin >> a[i];
        for (int i = 1; i <= m; i++) cin >> b[i];

        int kmax = 0;
        if (max(n, m) >= min(n, m) * 2) kmax = min(n, m);
            else kmax = (n + m) / 3;

        sort(a.begin() + 1, a.begin() + 1 + n);
        sort(b.begin() + 1, b.begin() + 1 + m);
        vector<int> tota(n + 5, 0), totb(m + 5, 0);
        int l = 1, r = n, cnt = 0;
        while (r - l >= 1) {
            tota[++cnt] = a[r] - a[l];
            ++l, --r;
        }
        l = 1, r = m, cnt = 0;
        while (r - l >= 1) {
            totb[++cnt] = b[r] - b[l];
            ++l, --r;
        }

        cout << kmax << "\n";

        int cnta = 0, cntb = 0, ans = 0;
        for (int i = 1; i <= kmax; i++) {
            if (cnta * 2 + cntb + 2 <= n && cntb * 2 + cnta + 2 <= m) { // 
                if (tota[cnta + 1] > totb[cntb + 1]) ans += tota[++cnta];
                    else ans += totb[++cntb];
            } else if (cnta * 2 + cntb + 2 > n) {
                if (cnta * 2 + cntb + 1 <= n) {
                    ans += totb[++cntb];
                } else {
                    ans -= tota[cnta--];
                    ans += totb[++cntb];
                    ans += totb[++cntb];
                }
            } else {
                if (cntb * 2 + cnta + 1 <= m) {
                    ans += tota[++cnta];
                } else {
                    ans -= totb[cntb--];
                    ans += tota[++cnta];
                    ans += tota[++cnta];
                }
            }

            cout << ans << ' ';
        }

        cout << "\n";
    }

    return 0;
}
```

- cnta * 2 + cntb + 2 <= n && cntb * 2 + cnta + 2 <= m 无论上边还是下边都能加双点

  选加的好的一边

- cnta * 2 + cntb + 2 > n 上边加不了双点

  - cnta * 2 + cntb + 1 <= n 上边可以加单点
  - 上边连单点都加不了

- 下边加不了双点

  - 下边可以加单点
  - 下边连单点都加不了

如果上边或者下边连单点都加不了，那就再加不了单点的边上减去一个双点，而在另一条边上加上两个双点