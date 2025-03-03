# Codeforces Round 997 (Div. 2)

```c++
#include <bits/stdc++.h>
using ll = long long;
using namespace std;

int main() {
    int t;
    cin >> t;

    while (t--) {
        int n;
        cin >> n;

        vector<int> a(n);
        for (int i = 0; i < n; i++) {
            cin >> a[i];
            a[i]--;
        }

        ll res = 1LL * n * (n + 1) >> 1;
        for (int t = 0; t < 10; t++) {
            vector<int> pre(n + 1);
            for (int i = 0; i < n; i++) {
                pre[i + 1] = pre[i] + (a[i] >= t ? 1 : -1);
            }
            map<int, int> mp;
            for (int l = 0, r = 0; r < n; r++) {
                if (a[r] == t) {  // 钦定较大那部分的最小值为t
                    for (; l <= r; l++) {
                        mp[pre[l]]++;  // 把这一块一起加进去
                    }
                }
                res -= mp[pre[r + 1]];
            }
        }
        cout << res << endl;
    }

    return 0;
}
```

