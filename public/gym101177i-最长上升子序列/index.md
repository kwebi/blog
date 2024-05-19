# gym101177I-最长上升子序列

题目大意是给你一系列区间，区间两两互不相交，但可能会有重复出现，区间的值等于区间的长度，求最大的上升的区间值和。你可以类比最长上升子序列，不过最长上升子序列求的是序列的长度，而这里转化为序列的值。
<img src="https://wx1.sbimg.cn/2020/07/09/CASJY.jpg">
<!--more-->

```cpp
input
---
5
1 1
10 11
5 7
3 4
10 11
---
output
---
6
```

比如这个样例6=((1-1+1)+(7-5+1)+(11-10+1))。

这里提供`O(nlgn)`的做法：因为区间互不相交，所以只需要考虑区间的一端，就可以确定区间的顺序,可以考虑构造这样一个结构体，`l`存左端点，`v`存区间长度，`i`存原区间的位置。

然后按左端点排序，由于区间会有重复，所以为了避免重复更新，需要先更新后面的，所以如果`l`相等，我们就把`i`大的放前面。排完序后：按照排的顺序更新，每次查询当前这个Node的`i`所能达到的最大序列值，然后在`i`位置更新为这个最大值加上自己的值。由于是按照区间的顺序更新，即前`i`个满足上升性质，所以可以保证前`i`个的最大值就是结点i所能达到的最大上升区间序列值。前`i`个的最大值用树状数组可以维护。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;

struct Node {
    ll l, v;
    int i;
    bool operator==(const Node& b) const
    {
        return l == b.l;
    }
    bool operator<(const Node& b) const
    {
        if (l == b.l)
            return i > b.i;
        return l < b.l;
    }
};

Node a[100100];
ll Tr[100100];
ll INF = 2e9;
int lowbit(int x) { return x & -x; }

void update(int x, ll y, int n)
{
    for (int i = x; i <= n; i += lowbit(i)) {
        Tr[i] = max(Tr[i], y);
    }
}

ll query(int x)
{
    ll ret = -INF;
    for (int i = x; i; i -= lowbit(i)) {
        ret = max(ret, Tr[i]);
    }
    return ret;
}


int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        ll l, r;
        scanf("%lld%lld", &l, &r);
        a[i].l = l;
        a[i].i = i;
        a[i].v = r - l + 1;
    }
    sort(a + 1, a + 1 + n);
    ll ans = 0;
    for (int i = 1; i <= n; i++) {
        ll mx = query(a[i].i);
        update(a[i].i, mx + a[i].v, n);
        mx = query(a[i].i);
        ans = max(ans, mx);
    }
    printf("%lld\n", ans);
    return 0;
}
```

