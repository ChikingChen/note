# Educational Codeforces Round 174 (Rated for Div. 2) E

```c++
#include <bits/stdc++.h>
using i64 = long long;

void Solve() {
    std::string s;
    std::cin >> s;

    int n = s.size();

    int a[2], ab[2];
    std::cin >> a[0] >> a[1] >> ab[0] >> ab[1];

    std::vector<int> AA[2], AB[2];
    
    for (int i = 0, j = 1; i < n; i = j, j ++) {
        while (j < n && s[j] != s[j - 1]) j ++;

        if (j - i == 1) {
            a[s[i] - 'A'] --;
        }else {
            if (s[i] == s[j - 1]) {
                AA[s[i] - 'A'].push_back(j - i);
            }else {
                AB[s[i] - 'A'].push_back(j - i);
            }
        }
    }

    for (auto &V : AB) {
        std::ranges::sort(V, std::greater());
    }

    for (int i = 0; i < 2; i ++ ) { // 使用AB来消除ABAB
        while (ab[i] && AB[i].size()) {
            int V = AB[i].back();
            AB[i].pop_back();

            int del = std::min(V / 2, ab[i]);
            ab[i] -= del, V -= del * 2;

            if (V) AB[i].push_back(V);
        }
    }
    
    for (int i = 0; i < 2; i ++ ) { // 使用BA来消除ABAB
        while (AB[i].size() && ab[1 - i]) {
            int V = AB[i].back();
            AB[i].pop_back();

            int del = std::min(V / 2 - 1, ab[1 - i]);
            ab[1 - i] -= del;
            V -= del * 2;

            if (V > 2) AB[i].push_back(V);
            else a[0] --, a[1] --;
        }
    }

    for (int i = 0; i < 2; i ++ ) { // 使用AB来消除ABABA
        for (int j = 0; j < 2; j ++ ) {
            while (ab[i] && AA[j].size()) {
                int V = AA[j].back();
                AA[j].pop_back();
    
                int del = std::min(V / 2, ab[i]);
                ab[i] -= del;
                V -= del * 2;
    
                if (V > 1) AA[j].push_back(V);
                else a[j] --;
            }
        }
    }

    for (int i = 0; i < 2; i ++ ) { // 使用A和B来消除ABAB和ABABA
        for (auto V : AA[i]) {
            a[i] -= (V + 1) / 2;
            a[1 - i] -= V / 2;
        }

        for (auto V : AB[i]) {
            a[i] -= V / 2;
            a[1 - i] -= V / 2;
        }
    }

    if (a[0] < 0 || a[1] < 0) {
        std::cout << "NO\n";
    }else {
        std::cout << "YES\n";
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    int T;
    std::cin >> T;
    while (T --) {
        Solve();
    }
    return 0;
}
```

- a[0], a[1], ab[0], ab[1]

  a的计数，b的计数，ab的计数，ba的计数

- AA[0], AA[1], AB[0], AB[1]

  ABABA类型的计数，BABAB类型的计数，ABAB类型的计数，BABA类型的计数

一个字符串中如果出现了连续相同字符的情况那么这种情况下，前后应该被分为两块，因为分割点不可能被同时消去（参考ABBA）。这种情况下，分割开后分为三类，ABAB，ABABA和A，第三类直接使用普通的A或者B删去，第一类首先尽量使用对应的两位AB删去（ABAB使用AB，BABA使用BA），对于剩余的可以使用相反的两位AB删去，剩下的使用单位A和B来删去；第二类比较灵活，无论是AB还是BA都可以删去，最后使用单位A和B删去