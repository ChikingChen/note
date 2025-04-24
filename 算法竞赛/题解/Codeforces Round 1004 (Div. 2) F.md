# Codeforces Round 1004 (Div. 2) F

```c++
int n;
cin >> n;

map<int, LL> mp;
mp[0] = 1;
for (int i = 0; i < n; i++) {
    int x;
    cin >> x;

    map<int, LL> nmp;
    for (auto [k, v] : mp) {
        nmp[k ^ x] = v; // 普通转移
        if (k == 0) { // 两者相等的转移
            nmp[x] = v * 3;
            nmp[x] %= mod;
        }
    }
    if (mp.count(x)) { // 
        nmp[x] += (nmp[0] << 1);
        nmp[x] %= mod;
    }
    mp = nmp;
}

LL res = 0;
for (auto [_, x] : mp) {
    res += x;
    res %= mod;
}
cout << res << endl;
```

同一dp[i]中的不同状态代表两个相同的数字和唯一一个不同的数字之间的异或和

两个map，分别为nmp和mp，mp为dp[i - 1]的数据，nmp为dp[i]的数据在a[i]和状态不同的时候分为两种情况，三者相同（状态为0）时候任意选一个，原有状态乘上3；三者不同（状态不为0）则直接继承

a[i]和状态相同的时候为特殊情况，原有的状态为0的结果乘2加到状态为a[i]的结果上去（0,0,x->0,0,0）（0,0,x->0,x,x）

```c++
#include <bits/stdc++.h>

#define cerr(x) cerr << #x << " == " << (x) << endl;
#define cerr2(x, y) cerr << #x << " == " << (x) << ", " << #y << " == " << (y) << endl;

using namespace std;
using LL = long long;

constexpr LL inf = 0x3f3f3f3f3f3f3f3f;
constexpr LL mod = 1e9 + 7;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr), cout.tie(nullptr);

    int t;
    cin >> t;

    while (t--) {
        int n;
        cin >> n;

        map<int, LL> mp;
        mp[0] = 1;
        int pre = 0;
        for (int i = 0; i < n; i++) {
            int x;
            cin >> x;

            if (mp.count(pre)) {
                mp[pre] *= 3;
                mp[pre] %= mod;
            }
            if (mp.count(x ^ pre)) {
                mp[pre] += (mp[x ^ pre] << 1);
                mp[pre] %= mod;
            }
            pre ^= x;
        }

        LL res = 0;
        for (auto [_, x] : mp) {
            res += x;
            res %= mod;
        }
        cout << res << endl;
    }

    return 0;
}
```

主要的支出在for auto的循环上，从前i-1个的前缀和转移到前i个的前缀和，前面的dp\[i]\[x$\oplus$a[i]] = dp\[i]\[x]可以优化成dp[x]在原地不变而在所有其余特殊计算上都加上一个异或前缀和，首先是三者相等（原状态为0），更新过后的状态就是pre；其次是原状态与新输入的数字相等，这种情况下现状态则为a[i]$\oplus$pre