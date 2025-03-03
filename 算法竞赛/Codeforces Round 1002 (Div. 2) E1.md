```c++
#include <bits/stdc++.h>
using i64 = long long;

void Solve() {
    int n, m;
    std::cin >> n >> m;

    std::vector<int> a(n * m), b(n * m);
    for (int i = 0; i < n * m; i ++ ) {
        std::cin >> a[i];
    }
    for (int i = 0; i < n * m; i ++ ) {
        std::cin >> b[i];
    }

    int Pa = 0, Pb = 0;
    int ans = n * m;
    bool flag = true;

    while (flag) {
        while (Pb != n * m && a[Pa] != b[Pb]) Pb ++;
        if (Pb == n * m) break;

        do {
            if (a[Pa] != b[Pb]) {
                flag = false;
                break;
            }
            Pa ++, Pb ++;
        }while (Pa % m != 0 && Pa / m == Pb / m);
        
        if (flag) ans = n * m - Pa;
    }
    
    std::cout << ans << "\n";

}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    int tt; std::cin >> tt;
    while (tt --) Solve();
    return 0;
}
```

矩阵A经过魔改之后成为矩阵B，因此必然其中存在原有的元素，矩阵B中按次序找到矩阵A中的元素，剩下的都是经过操作塞进来的

如何按次序找？

双指针做法，分别指向矩阵A的第一个（p1）和矩阵B的第一个（p2），p2找到矩阵B中和矩阵A第一个元素相同的元素而后持续向下对比，直至找到A矩阵下一行的起始（矩阵A一行的起始可以塞入数字），过程中如果双指针在AB矩阵的同一行发现不同，则之后的数据都不需要继续检查（该位置被塞入了额外的数字，则说明后面的数字都是额外塞入的）