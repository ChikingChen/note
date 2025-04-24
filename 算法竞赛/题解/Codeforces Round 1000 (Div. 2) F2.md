```c++
#include<bits/stdc++.h>
using namespace std;

typedef long long LL;
const int P = 998244353;
template<const int mod>
struct ModInt {
    static const int P = mod;
    int x;
    ModInt (int x = 0) : x(x % P) {}
    ModInt (LL x) : x(int(x % P)) {}
    int val() {return x;}

    ModInt operator + (const ModInt &a) const {int x0 = x + a.x; return ModInt(x0 < P ? x0 : x0 - P);}
    ModInt operator - (const ModInt &a) const {int x0 = x - a.x; return ModInt(x0 < 0 ? x0 + P : x0);}
    ModInt operator * (const ModInt &a) const {return ModInt(1ll * x * a.x % P);}
    ModInt operator / (const ModInt &a) const {return *this * a.inv();}
    bool operator == (const ModInt &a) const {return x == a.x;};
    bool operator != (const ModInt &a) const {return x != a.x;};
    void operator += (const ModInt &a) {x += a.x; if (x >= P) x -= P;}
    void operator -= (const ModInt &a) {x -= a.x; if (x < 0) x += P;}
    void operator *= (const ModInt &a) {x = 1ll * x * a.x % P;}
    void operator /= (const ModInt &a) {*this = *this / a;}
    friend ModInt operator + (int y, const ModInt &a){int x0 = y + a.x; return ModInt(x0 < P ? x0 : x0 - P);}
    friend ModInt operator - (int y, const ModInt &a){int x0 = y - a.x; return ModInt(x0 < 0 ? x0 + P : x0);}
    friend ModInt operator * (int y, const ModInt &a){return ModInt(1ll * a.x * y % P);}
    friend ModInt operator / (int y, const ModInt &a){return ModInt(y) / a;}
    friend ostream &operator<<(ostream &os, const ModInt &a) {return os << (a.x + P) % P;}
    friend istream &operator>>(istream &is, ModInt &t) {return is >> t.x;}

    ModInt pow(LL n) const {
       ModInt sum(1), base(x);
       n %= (P - 1);
       while (n) {
           if (n & 1) sum *= base;
           base *= base;
           n >>= 1;
       }
       return sum;
    }

    ModInt inv() const {
        int a = x, b = P, x = 1, y = 0;
        while (b) {
        int t = a / b;
           a -= t * b; swap(a, b);
           x -= t * y; swap(x, y);
        }
        if (x < 0) x += P;
        return x;
    }
};
using mint = ModInt<P>;

#define int long long

typedef pair<int, int> pii;
typedef unsigned long long uLL;
typedef __int128_t i128;

const int inf32 = 1e9;
const LL inf64 = 1e18;

const int maxn = 6e5 + 5;

mint dp[maxn];

void init(int n) {
    dp[0] = dp[2] = 1;
    for (int i = 4; i <= n; i += 2) {
        dp[i] = dp[i - 2] * (2 * i - 2) / (i / 2 + 1);
    }
}

template<class Info, class Tag>
struct _LazySegmentTree { // 区间 [0, n]
    int n;
    vector<Info> info;
    vector<Tag> tag;

    _LazySegmentTree(int x) { // 初始值
        vector<Info> a(x + 5, Info());
        init(x, a);
    }
    _LazySegmentTree(int x, vector<Info> a) { // a 赋值
        init(x, a);
    }

    void init(int x, vector<Info> a) {
        n = x;
        info.assign(4 * n + 5, Info());
        tag.assign(4 * n + 5, Tag());

        function<void(int, int, int)> build = [&] (int l, int r, int p) {
            if (l == r) return info[p] = a[l], void();
            int mid = (l + r) >> 1;
            build(l, mid, p << 1);
            build(mid + 1, r, p << 1 | 1);
            pushup(p);
        };
        build(0, n, 1);
    }

    void apply(int p, Tag t) {
        tag[p].apply(t);
        info[p].apply(t);
    }
    void pushup(int p) {
        info[p] = info[p << 1] + info[p << 1 | 1];
    }
    void pushdown(int p) {
        apply(p << 1, tag[p]);
        apply(p << 1 | 1, tag[p]);
        tag[p] = Tag();
    }

    void rangeApply(int l, int r, int x, int y, Tag k, int p) {
        if (x <= l && r <= y) {
            apply(p, k);
            return ;
        }
        pushdown(p);
        int mid = (l + r) >> 1;
        if (x <= mid) rangeApply(l, mid, x, y, k, p << 1);
        if (y > mid) rangeApply(mid + 1, r, x, y, k, p << 1 | 1);
        pushup(p);
    }   
    void rangeApply(int x, int y, Tag k) {
        rangeApply(0, n, x, y, k, 1);
    }

    Info rangeQuery(int l, int r, int x, int y, int p) {
        if (x <= l && r <= y) {
            return info[p];
        }
        pushdown(p);
        Info res = Info().iniQuery();
        int mid = (l + r) >> 1;
        if (x <= mid) res = res + rangeQuery(l, mid, x, y, p << 1);
        if (y > mid) res = res + rangeQuery(mid + 1, r, x, y, p << 1 | 1);
        pushup(p);
        return res;
    }
    Info rangeQuery(int x, int y) {
        return rangeQuery(0, n, x, y, 1);
    }
};

struct Tag {
    LL add;

    void apply(Tag t) {
        add += t.add;
    }

    Tag(LL x = 0) : add(x) {} // Lazytag 初值
};

struct Info {
    LL mn, s;

    void apply(Tag t) {
        mn += t.add;
    }

    Info(LL x = 0, LL y = 1) : mn(x), s(y) {} // 初始值
    Info iniQuery() { // Query 初值 
        return Info(inf64, 0); 
    }
};

Info operator + (Info a, Info b) {
    if (a.mn != b.mn) {
        return a.mn < b.mn ? a : b;
    } else {
        return Info(a.mn, a.s + b.s);
    }
}

using LazySegmentTree = _LazySegmentTree<Info, Tag>;

int tree[maxn << 2];

void pushup(int p) {
    tree[p] = max(tree[p << 1], tree[p << 1 | 1]);
}
void build(int l, int r, int p) {
    if (l == r) return tree[p] = 0, void();
    int mid = (l + r) >> 1;
    build(l, mid, p << 1);
    build(mid + 1, r, p << 1 | 1);
    pushup(p);
}
void update(int l, int r, int x, int k, int p) {
    if (l == r && l == x) {
        tree[p] = k;
        return ;
    }
    int mid = (l + r) >> 1;
    if (x <= mid) update(l, mid, x, k, p << 1);
        else update(mid + 1, r, x, k, p << 1 | 1);
    pushup(p);
}
int query(int l, int r, int x, int y, int p) {
    if (x <= l && r <= y) {
        return tree[p];
    }
    int res = 0, mid = (l + r) >> 1;
    if (x <= mid) res = max(res, query(l, mid, x, y, p << 1));
    if (y > mid) res = max(res, query(mid + 1, r, x, y, p << 1 | 1));
    return res;
}

signed main()
{
    cin.tie(0); cout.tie(0);
    ios::sync_with_stdio(0);

    init(6e5);

    int T = 1;
    cin >> T;
    while (T--) {
        int n;
        cin >> n;
        vector<pii> seg(n + 5, {0, 0});
        for (int i = 1; i <= n; i++) cin >> seg[i].first >> seg[i].second;

        mint ans = dp[2 * n];
        vector<int> val(2 * n + 5, 0);
        val[0] = 2 * n;
        cout << ans << ' ';

        build(1, 2 * n, 1);
        LazySegmentTree t(2 * n);
        for (int i = 1; i <= n; i++) {
            auto [x, y] = seg[i];
            int used = y - x + 1 - t.rangeQuery(x, y).s; // 新增的被使用的块

            int l = 1, r = x, tag = 0;
            while (l <= r) {
                int mid = (l + r) >> 1;
                if (query(1, 2 * n, mid, x, 1) > y) l = mid + 1, tag = mid;
                    else r = mid - 1;
            } // 二分找较大括号的左边界
            ans /= dp[val[tag]]; // 除去左边界原有对应括号中的空闲块
            val[tag] -= (y - x + 1) - used; // 计算现有左边界对应括号中的空闲块
            ans *= dp[val[tag]]; // 乘上左边界现有对应括号中的空闲块

            val[x] = y - x + 1 - used - 2; // 设定小括号中的左括号对应的空闲块
            ans *= dp[val[x]]; // 乘到乘法中去

            update(1, 2 * n, x, y, 1), t.rangeApply(x, y, 1);
            // 在两棵线段树中分别设置新括号的存在

            cout << ans << ' ';
        }

        cout << "\n";
    }

    return 0;
}
```

假设现在有一个括号内加入了一个新的括号，答案的更新仅和这两个相互嵌套的括号相关。相互嵌套的括号中的空闲块分为两个部分，分别为位于括号内的和位于括号外的，先前计算答案的时候只用将两者作为一个整体考虑，现在需要将两者分开。首先我们需要知道较大的括号的右括号在哪，这需要对第一棵线段树二分得到。而后是较小的括号内有多少空闲块，我们通过第一棵线段树的查询得到。

