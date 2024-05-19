# 快速幂与模运算

[例题](https://www.luogu.org/problemnew/show/P1226):

> 输入 `b`，`p`，`k` 的值，求 `b`^`p` mod `k` 的值。其中 `b`，`p`，`k` 为长整型数。
<!--more-->

1. 快速幂

假设`p`=`22`，那么`p`可以用`16`+`4`+`2`表示，即 $2^4+2^2+2^1$ 表示
<br />
上代码(没求模)：

```cpp
using ll = long long;

ll pow_mod(ll b, ll p)
{
    ll res = 1;
    while (p > 0)
    {
        if (p & 1)
        {
            res = (res * b);//当前位为1，则乘以b
        }
        b = (b * b);//求下一位的b
        p >>= 1;
    }
    return res ;
}
```

1. 模运算


$$
(A+B) \mod b=(A \mod b+B \mod b) \mod b
$$

$$
(A×B) \mod b =((A \mod b)×(B \mod b)) \mod b
$$



所以上面那题的可以写出来了
```cpp
#include <stdio.h>

using ll = long long;

ll pow_mod(ll b, ll p, ll k)
{
    ll res = 1;
    while (p > 0)
    {
        if (p & 1)
        {
            res = (res * b) % k;
        }
        b = (b * b) % k;
        p >>= 1;
    }
    return res % k;
}

int main()
{
    ll b, p, k;
    scanf("%lld%lld%lld", &b, &p, &k);
    ll res = pow_mod(b, p, k);
    printf("%lld^%lld mod %lld=%lld", b, p, k, res);
    return 0;
}
```

