# CodeForces 1974 C题解


题目链接如下：[C. Beautiful Triple Pairs](https://codeforces.com/contest/1974/problem/C)，求最美三元组的对数（一对最美三元组指两个三元组，恰好有2个元素相同，一个不同）。

思路是分类讨论，$(a,b,c)$ ，分别计算 $(a,b)$, $(a,c)$ , $(b,c)$ 的最美三元组，最后相加。

因为需要有一个元素不同，需要去重，然后记录重复元组个数。然后计数即可，计数思路是分类计数，以 $[[1,1,2],[1,1,2],[1,1,3],[1,1,4],[1,1,4]]$ 为例子，答案是每类元组个数乘剩下元组个数的和，最后除以二。

AC代码如下：
```cpp
#include <algorithm>
#include <iostream>
#include <math.h>
#include <queue>
#include <stdio.h>
#include <vector>
#include <string.h>
#include <set>
#include <map>
#include <cassert>

using namespace std;

using ll = long long;
typedef pair<ll, ll> pll;

constexpr int N = 200005;

ll a[N];

struct Node {
	ll a, b, c;
	bool operator<(const Node& x) const {
		if (a != x.a) {
			return a < x.a;
		} else if (a == x.a) {
			if (b != x.b) {
				return b < x.b;
			} else if (b == x.b) {
				return c < x.c;
			}
		}
	}
};

int main()
{
	// cin.tie(nullptr)->sync_with_stdio(false);
	int T;
	cin >> T;
	while (T-- > 0) {
		int n;
		cin >> n;
		for (int i = 0; i < n; i++) {
			cin >> a[i];
		}
		map<Node, ll> st;
		for (int i = 0; i + 2 < n; i++) {
			st[ {a[i], a[i + 1], a[i + 2]}]++;
		}
		map<pll, ll> vp1;
		map<pll, ll> vp2;
		map<pll, ll> vp3;

		map<pll, ll> vpc1;
		map<pll, ll> vpc2;
		map<pll, ll> vpc3;

		for (auto val : st) {
			auto e = val.first;
			auto cnt = val.second;

			auto lp1 = make_pair(e.a, e.b);
			auto lp2 = make_pair(e.b, e.c);
			auto lp3 = make_pair(e.a, e.c);

			vp1[lp1]++;
			vpc1[lp1] += cnt;
			vp2[lp2]++;
			vpc2[lp2] += cnt;
			vp3[lp3]++;
			vpc3[lp3] += cnt;
		}
		ll ans = 0;

		for (auto val : st) {
			auto e = val.first;
			auto cnt = val.second;

			auto lp1 = make_pair(e.a, e.b);
			auto lp2 = make_pair(e.b, e.c);
			auto lp3 = make_pair(e.a, e.c);
			ll ad = cnt;
			ans += (cnt ) * (vpc1[lp1] - cnt );
			ans += (cnt ) * (vpc2[lp2] - cnt );
			ans += (cnt ) * (vpc3[lp3] - cnt );
		}

		cout << ans/2 << '\n';
	}

	return 0;
}
```



