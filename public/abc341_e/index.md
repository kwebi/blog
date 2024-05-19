# abc341_E



题目链接如下：[E - Alternating String](https://atcoder.jp/contests/abc341/tasks/abc341_e)

题目大意如下：一个由 0 和 1 组成的长度为 𝑁 的字符串，如果字符串中任意两个连续的字符都不相同，则称为良好字符串。进行以下操作 𝑄 次： 1是从 𝐿 到 𝑅 翻转字符串， 0 变 1 ， 1 变 0 。2是判断 𝐿 到 𝑅 是否为良好字符串。
$$
（1≤N,Q≤5×10^5)
$$


思路如下：Q为 5×10^5 ，那么总体复杂度不超过 𝑛𝑙𝑜𝑔𝑛 。可以看出规律，一个字符串是否为良好字符串，可以分为左右两个字符串，左边和右边必须都为良好字符串，并且两个字符串连接处必须不相同。可以看出具有结合律性质，用线段树进行解决。L到R翻转，不影响L到R是否为良好字符串，所以每次翻转，打上标记，然后将代表L..R区间的树节点的左右改变一下，如果是翻转打上标记的区间的子区间，那么需要下放懒标记，直到修改区间，再维护一下父节点状态。查询带有懒标记的一个子区间，也同样需要下放，再维护。

构造线段树：

```
struct Node {
    short v = -1; // 值，good string为0，否则为1
    short lv, rv;
    short lz = 0;//懒标记
};
void maintain(vector<Node>& tr, int o)
{
    Node ln = tr[o * 2];
    Node rn = tr[o * 2 + 1];
    tr[o].lv = ln.lv;
    tr[o].rv = rn.rv;
    if (ln.v == 1 && rn.v == 1 && ln.rv != rn.lv) { // 维护信息,两个区间结合，判断是否为good string
        tr[o].v = 1;
    } else {
        tr[o].v = 0;
    }
}
void build(vector<Node>& tr, int l, int r, int o)
{
    if (l == r) {
        tr[o].v = 1;
        tr[o].lv = a[l];
        tr[o].rv = a[l];
        return;
    }
    int mid = (l + r) / 2;
    build(tr, l, mid, o * 2);
    build(tr, mid + 1, r, o * 2 + 1);
    maintain(tr, o);
}
```

翻转L到R：

```
void revNode(Node& n)
{
    n.lz ^= 1;
    n.lv ^= 1;
    n.rv ^= 1;
}
void lowdown(vector<Node>& tr, int o) // 下放懒标记
{
    tr[o].lz = 0;
    revNode(tr[o * 2]);
    revNode(tr[o * 2 + 1]);
}
void query1(vector<Node>& tr, int l, int r, int ql, int qr, int o)
{
    if (ql == l && qr == r) {
        revNode(tr[o]);
        if (l == r)
            tr[o].lz = 0;
        return;
    }

    if (tr[o].lz == 1) { // 传递懒标记
        lowdown(tr, o);
    }
    int mid = (l + r) / 2;
    if (qr <= mid) {
        query1(tr, l, mid, ql, qr, o * 2);
    }
    if (ql > mid) {
        query1(tr, mid + 1, r, ql, qr, o * 2 + 1);
    }
    if (ql <= mid && qr > mid) {
        query1(tr, l, mid, ql, mid, o * 2);
        query1(tr, mid + 1, r, mid + 1, qr, o * 2 + 1);
    }
    maintain(tr, o);

}
```

查询L到R：

```
Node query2(vector<Node>& tr, int l, int r, int ql, int qr, int o)
{
    if (ql == l && qr == r) {
        return tr[o];
    }
    int mid = (l + r) / 2;
    if (tr[o].lz == 1) { // 传递懒标记
        lowdown(tr, o);
    }
    if (qr <= mid) {
        Node ln = query2(tr, l, mid, ql, qr, o * 2);
        return ln;
    }
    if (ql > mid) {
        Node rn = query2(tr, mid + 1, r, ql, qr, o * 2 + 1);
        return rn;
    }
    if (ql <= mid && qr > mid) {
        Node ln = query2(tr, l, mid, ql, mid, o * 2);
        Node rn = query2(tr, mid + 1, r, mid + 1, qr, o * 2 + 1);
        maintain(tr, o);
        Node res;
        res.lv = ln.lv;
        res.rv = rn.rv;
        if (ln.v == 1 && rn.v == 1 && ln.rv != rn.lv) { // 合并区间维护是否为好字符串信息
            res.v = 1;
        } else {
            res.v = 0;
        }
        return res;
    }
}
```

主函数如下：

```
#include <algorithm>
#include <iostream>
#include <map>
#include <math.h>
#include <queue>
#include <stdio.h>
#include <string.h>
#include <vector>
using namespace std;

using ll = long long;

struct Node {
    short v = -1; // 值，good string为0，否则为1
    short lv, rv;
    short lz = 0;
};

vector<short> a(500005);
vector<Node> tr(2000005 + 1024);

char s[500005];
int main()
{
    int n, q;
    cin.tie(0)->sync_with_stdio(false);
    cin >> n >> q;
    cin >> s;
    for (int i = 0; i < strlen(s); i++) {
        a[i + 1] = s[i] - '0';
    }

    build(tr, 1, n, 1);

    for (int i = 0; i < q; i++) {
        int t, ql, qr;
        cin >> t >> ql >> qr;
        if (t == 1) {
            query1(tr, 1, n, ql, qr, 1);
        }
        if (t == 2) {
            int k = query2(tr, 1, n, ql, qr, 1).v;
            if (k == 1)
                cout << "Yes" << '\n';
            else
                cout << "No" << '\n';
        }
    }

    return 0;
}
```

官方题解使用的是前后比较后得出的数组A，数组Ai表示的是 𝑆𝑖 是否等于 𝑆𝑖+1，每次翻转，影响的只有此数组的两边，所以线段树只需单点修改，无需带懒标记的区间修改。常数更小。查询L到R，只需判断L到R-1是否都为1即可，可以用区间和判断。
