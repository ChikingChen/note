# Codeforces Round 1012 (Div. 2) E2

```C++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll t,n,k,pos,a[400009],stk[400009];
inline ll read(){
	ll s = 0,w = 1;
	char ch = getchar();
	while (ch > '9' || ch < '0'){ if (ch == '-') w = -1; ch = getchar();}
	while (ch <= '9' && ch >= '0') s = (s << 1) + (s << 3) + (ch ^ 48),ch = getchar();
	return s * w;
}
bool judge(ll x){
	ll l = 1,r = 0,res = 0;
	for (ll i = pos;i >= pos - n;i -= 1){
		if (l <= r && stk[l] - i > x) l += 1;
		if (pos - i >= x) res += max(0ll,a[stk[l]] - a[i]);
		if (res > k) return 0;
		while (l <= r && a[stk[r]] > a[i]) r -= 1;
		stk[++ r] = i;
	}
	return 1;
}
int main(){
	t = read();
	while (t --){
		n = read(),k = read();
		ll suma = 0;
		for (ll i = 1;i <= n;i += 1) a[i] = read(),suma += a[i];
		for (ll i = 1;i <= n;i += 1) a[i] -= read(),a[i + n] = a[i];
		if (suma == k){puts("0"); continue;}
		for (ll i = 1;i <= 2 * n;i += 1) a[i] += a[i - 1];
		pos = n;
		for (ll i = n + 1;i <= 2 * n;i += 1) if (a[i] < a[pos]) pos = i;
		ll l = 1,r = n,ans = n;
		while (l <= r){
			ll mid = (l + r) >> 1;
			if (judge(mid)) ans = mid,r = mid - 1;
			else l = mid + 1;
		}
		printf("%lld\n",ans);
	}
	return 0;
}
```

计算出$c_i$，$c_i$最小值所在位置$pos$即为数组$n-1$所在位置，在$1$到$n$上二分寻找需要几个轮次才能使得$a$数组全为$0$。可以理解的是在贪心策略下，随着删除次数的增加，轮次减少，具有单调性。

贪心策略：使用单调栈反向遍历$c$数组$pos$到$pos-n$。

简单版本中如何维护一个位置要经过多少轮才能清空？观察$c$数组，找到$c$数组中$i$位置右侧最近的比它小的$c$数组成员。

困难版本中如何维护一个位置要在$x$轮中被清空？$c$数组中$i$位置右侧$x$个数组成员，选择他们的最小值，如果他们的最小值大于$c_i$，那么就对$a_{i+1}$减去**最小值和$c_i$的差**，也就是进行如此多次的操作。

使用双端队列来维护这个过程。一个端口保持单调栈的性质，另一个端口保证单调栈内的数字都在当前位置加x个长度的范围内。单调栈要求栈顶是

