```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pb push_back
#define fi first
#define se second
#define SZ(x) (int)(x.size())
const int N=2e5+10;
int t,n,u,v,l[N],r[N];
ll ans;
vector<int>e[N];
void dfs(int u,int fa){
    for(auto &v:e[u]){
        if(v==fa)continue;
        dfs(v,u);
        if(l[v]>=r[u]){//至少需要操作r[u]这么多次，并且子树需要额外操作的次数l[v]-r[u]
            l[u]=r[u];
            ans+=l[v]-r[u];
        }
        else{//v至少需要操作l[v]次，那么u也至少操作l[v]次
            l[u]=max(l[u],l[v]);
        }
    }
}
int main(){
    scanf("%d",&t);
    while(t--){
        scanf("%d",&n);
        ans=0;
        rep(i,1,n)e[i].clear();
        rep(i,1,n){
            scanf("%d%d",&l[i],&r[i]);
        }
        rep(i,2,n){
            scanf("%d%d",&u,&v);
            e[u].pb(v);
            e[v].pb(u);
        }
        dfs(1,0);
        printf("%lld\n",ans+l[1]);
    }
    return 0;
}
```

 研究父节点u和子节点v，假设为固定值a[u]，a[v]

- a[u] > a[v]：以父节点为根节点，子节点直接加
- a[v] > a[u]：以子节点为根节点，父节点直接加，会加到根节点上，所以需要计数

父节点区间为[l[u], r[u]]，子节点区间为[l[v], r[v]]

- l[v]>=r[u]：子节点必然大于父节点，加到父节点上去时会带到根节点，这种情况下，父节点选最大，子节点选最小，剩下的加上补平差距，l[u]=r[u]保证父节点值选最大
- l[v]<r[u]：子节点可能小于父节点，父节点为了保证合法的同时尽可能小，要限制父节点的最小值为max(l[u],l[v])

