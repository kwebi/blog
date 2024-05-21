# CodeForces 1163B2个人题解


题目链接如下：[CF-R-558-Div2 B2. Cat Party (Hard Edition)](https://codeforces.com/problemset/problem/1163/B2)

#### 题意：

在  $ n $  天里，每一天都会有一只猫来到志郎家。在第 $ i$ 天来的猫有一条颜色为 $U_i$ 的丝带。志郎想知道最大的数字 $ x $ ，如果我们考虑前 $ x $ 天的连线，那么可以从这个连线中去掉$ 1 $天，这样在剩下的$x−1$天中出现的每种颜色的丝带出现的次数都是一样的。

转化为数学含义，在 $ n $ 个数的数组中，在前 $ x $ 中去掉一个，前 $x-1$ 个，每个数出现的次数都一样。

例如：$[2,2,1,1,5,4,4,5]$，那么，$x=7$ 构成了一个答案，因为如果我们去掉最左边的 $U_i = 5$ 
 ，每种色带颜色在 $x−1$ 天的前缀中将正好出现两次。

#### 思路：

记录每个数出现的次数，用map记录出现次数的组数，然后从右到左模拟判断。比如 $[2,2,1,1,5,4,4,5]$，2出现2次，1出现2次，5出现2次，4出现2次。那么出现次数都是2次，4组。

判断方法如下：
- 看出现次数的集合为`1`还是`2`，其他都无法满足要求
- 为`1`，两种情况，一种是所有数全部都一样，一种是所有数都不一样（出现次数都为1）满足要求。
- 为`2`，那么看出现次数的组数，是否存在一组次数，能全部转换为另一组次数。比如：
  1. $[2,2,1,1,5,4,4]$，2次的3组，1次的1组。去掉一次一组的即可，即某一组次数1，组数1。
  2. $[2,2,1,1,1,4,4]$，2次的2组，3次的1组。把3次的去掉一次，那么刚好变成2次的3组。所以规律是需要一类次数比一另类次数多1，然后组数刚好为1。

如果条件不满足，那么需要更新两个`map`，即出现 $a_i$ 的次数的`map1`，次数的组数的`map2`，一个数删除了，对应数的次数也减少，如果数组完全没有这个数了，则删除。对应次数减少了，那么`map2`对应也要更新（移除一个数，一个数的次数的组数减少，另一个次数的组数可能增加）。

#### AC代码
```cpp
#include <algorithm>
#include <iostream>
#include <math.h>
#include <queue>
#include <stdio.h>
#include <vector>
#include <math.h>
#include <string.h>
#include <map>
#include <unordered_map>
#include <assert.h>
using namespace std;

using ll = long long;

constexpr int N = 100005;


int a[N];
unordered_map<int, int> b;
int num[N];

unordered_map<int, int> mp;

int main() {

	int n;
	cin >> n;

	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}

	for (int i = 1; i <= n; i++) {
		mp[a[i]]++;//v的个数的集合
	}

	for (auto e : mp) {
		b[e.second]++;//v有多少个的组数
	}

	for (int i = n; i >= 1; i--) {
		auto ib = b.begin();
		ib++;//ib是b里面的第二个元素
		auto ip = *(b.begin());
		
		if (b.size() == 2 ) {
			auto is = *ib;
			if (ip.second == 1 && ip.first - 1 == is.first) {//刚好1组比2组多一个数
				cout << i;
				break;
			}
			if (is.second == 1 && is.first - 1 == ip.first) {
				cout << i;
				break;
			}
			if (is.second == 1 && is.first == 1) { //2个组，有一个组内只有一个数
				cout << i;
				break;
			}
			if (ip.second == 1 && ip.first == 1) {
				cout << i;
				break;
			}
		}
		if (b.size() == 1 ) {
			if (ip.first == 1) {//一个组一个数
				cout << i;
				break;
			}
			if (ip.first == i) {//一个组，数全相同
				cout << i;
				break;
			}
		}

		int v = a[i];
		b[mp[v]]--;//v个数的组数减少
		if (b[mp[v]] == 0) {//组数为0，删除
			b.erase(mp[v]);
		}
		mp[v]--;//v的个数减少
		if (mp[v] == 0) {
			mp.erase(v);
		} else {
			b[mp[v]]++;//减少后的个数对应的组数增加
		}
	}

	return 0;
}
```


