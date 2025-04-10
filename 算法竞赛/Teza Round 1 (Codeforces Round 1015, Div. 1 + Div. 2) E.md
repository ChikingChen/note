# Teza Round 1 (Codeforces Round 1015, Div. 1 + Div. 2) E

```c++
#include<bits/stdc++.h>

using namespace std;

typedef long long LL;
#define endl '\n'
#define mp make_pair

const int mod=1e9+7;

struct CCC
{
    int n;
    vector<int> jc,inv;
    CCC(int nn):n(nn),jc(nn+1),inv(nn+1){init();};
    void init()
    {
        jc[0]=jc[1]=1;
        for(int i=2;i<=n;i++) jc[i]=1ll*jc[i-1]*i%mod;
        inv[0]=1;
        inv[n]=pw(jc[n],mod-2);
        for(int i=n-1;i>=1;i--) inv[i]=1ll*inv[i+1]*(i+1)%mod;
    }
    int pw(int a,int k)
    {
        int ans=1;
        for(;k;k>>=1)
        {
            if(k&1) ans=1ll*ans*a%mod;
            a=1ll*a*a%mod;
        }
        return ans;
    }
    int C(int n,int m)
    {
        return 1ll*jc[n]*inv[m]%mod*inv[n-m]%mod;
    }
    int A(int n,int m)
    {
        return 1ll*jc[n]*inv[n-m]%mod;
    }
};
CCC C(5001);
void solve()
{
    int n;
    cin>>n;
    vector<int> t(n+1),num(n+1);
    for(int i=1;i<=n;i++) cin>>t[i];
    vector<bool> ok(n+1);
    for(int i=1;i<=n;i++)
    {
        num[i]=num[i-1];
        if(t[i]!=-1) ok[t[i]]=true;
        else num[i]++;
    }
    vector<vector<int>> dp(num[n]+1,vector<int>(n));//dp[-1的个数][mex-1]
    int mi1=n,mi2=n;//l左边和右边已知元素的最小值
    for(int l=1;l<=n;l++)
    {
        mi2=n;
        for(int r=n;r>=l;r--)
        {
            dp[num[r]-num[l-1]][0]++;
            dp[num[r]-num[l-1]][min(mi1,mi2)]--;//不用在意区间可能达不到这个mex，后面会有限制
            if(t[r]!=-1) mi2=min(mi2,t[r]);
        }
        if(t[l]!=-1) mi1=min(mi1,t[l]);
    }
    for(int i=0;i<=num[n];i++)
    {
        for(int j=1;j<=n;j++)
        {
            dp[i][j]+=dp[i][j-1];
        }
    }
    int ans=0,cnt=0;
    for(int i=0;i<n;i++)//i=mex-1
    {
        cnt+=(!ok[i]);//最少填入多少个数可以到达mex
        for(int j=cnt;j<=num[n];j++)
            ans=(1ll*ans+1ll*C.A(j,cnt)*C.jc[num[n]-cnt]%mod*dp[j][i]%mod)%mod;
    }
    cout<<ans<<endl;
}
signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    int T=1;
    cin>>T;
    while(T--) solve();
}
```

