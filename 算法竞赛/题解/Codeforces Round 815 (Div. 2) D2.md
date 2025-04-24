# Codeforces Round 815 (Div. 2) D2

```c++
#include<iostream>
#include<cstring>
using namespace std;
using LL = long long;
const int maxn = 3e5 + 5, INF = 0x3f3f3f3f;
struct Node{
    int ch[2], dp[2];
}tr[maxn * 31];
int idx;
int a[maxn];

int main(){

    cin.tie(0);
    cout.tie(0);
    ios::sync_with_stdio(0);

    int T;
    cin >> T;
    while(T--){
        for(int i = 0; i <= idx; i++)
            tr[i].ch[0] = tr[i].ch[1] = tr[i].dp[0] = tr[i].dp[1] = 0;
        idx = 0;
        int n;
        cin >> n;
        for(int i = 0; i < n; i++) cin >> a[i];
        int ans = 0;
        for(int i = 0; i < n; i++){
            int t = a[i] ^ i;
            int p = 0;
            int maxv = 0;
            for(int j = 30; j >= 0; j--){
                int bit = t >> j & 1;
                if (tr[p].ch[bit ^ 1])
                    maxv = max(maxv, tr[tr[p].ch[bit ^ 1]].dp[(a[i] >> j & 1) ^ 1]);
                if (!tr[p].ch[bit]) break;
                p = tr[p].ch[bit];
            }
            int v = maxv + 1;
            ans = max(ans, v);
            p = 0;
            for(int j = 30; j >= 0; j--){
                int bit = t >> j & 1;
                if (!tr[p].ch[bit]) tr[p].ch[bit] = ++idx;
                tr[tr[p].ch[bit]].dp[i >> j & 1] = max(tr[tr[p].ch[bit]].dp[i >> j & 1], v);
                p = tr[p].ch[bit];
            }
        }
        cout << ans << '\n';
    }
}
```

- 如何理解字典树的结构

  - 使用什么内容来遍历字典树

    考虑$a[i]\oplus j<a[j]\oplus i$，前k位相等时候则前k位符合$a[i]\oplus i==a[j]\oplus j$。利用这种性质，我们在解决$a[i]\oplus j<a[j]\oplus i$问题的时候可以从前$k$位相等结构入手，使用$a[i]\oplus i$遍历字典树。

  - dp数组的意义

    考虑$a[i]\oplus j<a[j]\oplus i$，第$k + 1$位不等的情况下若要满足$a[i]\oplus j<a[j]\oplus i$，那么就要求$a[i]\oplus j$为0，$a[j]\oplus i$为1，因此我们关注i（实际上关注a[i]）也可以起到相同的效果。前面说过在前k位中只用关注$a[i]\oplus i$的数值，这体现在字典树索引上，而第$k + 1$位要关注i这就体现在dp索引上，可以这样理解，我们通过$a[i]\oplus i$在字典树中查找到第$k$位，而第$k+1$位与$a[i]\oplus i$不同，则进入$a[i]\oplus i\oplus 1$的子节点；我们通过$a[j]\oplus i=1$得到$i=a[j]\oplus 1$也就是说$a[j]$的对应位异或上1之后就是i的对应位，也就是说$a[j]\oplus j$在$k+1$位上为$a[i]\oplus i\oplus 1$同时在$k+1$位上$i$为$a[j]\oplus1$的最优结果

    大致总结一下过程，先根据$a[i]\oplus i$搜索字典树前$k$位，因为$a[i]\oplus i==a[j]\oplus j$在$k$位成立。到了第$k+1$位的时候出现了不同那么这种情况下则要求$a[i]\oplus j<a[j]\oplus i$，显然，$a[j]\oplus i$必然为1，之前dp数组存了i的第k+1位的最优结果，则通过$i=a[j]\oplus 1$能够得到最优结果

- 如何使用字典树

  考虑$a[i]\oplus j<a[j]\oplus i$，前k位相等时候则前k位符合$a[i]\oplus i==a[j]\oplus j$，第$k + 1$位不等的情况下若要满足$a[i]\oplus j<a[j]\oplus i$，那么就要求$a[i]\oplus j$为0，$a[j]\oplus i$为1，前面说过$dp$中括号中的数字的意义为i从高到低的位数，