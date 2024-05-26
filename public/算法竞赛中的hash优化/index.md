# 算法竞赛中的hash优化


### undered_map 加速

参考文章：[Tricks to make unordered_map faster added.](https://codeforces.com/blog/entry/21853)
undered_map 使用拉链法 hash，可以通过改变初始大小和负载因子来变快。
可以变快约 1/5。

```cpp
unordered_map<int, int> mp;
mp.reserve(1024);
mp.max_load_factor(0.25);
```

### 防 hack

参考文章：[Blowing up unordered_map, and how to stop getting hacked on it](https://codeforces.com/blog/entry/62393)
在 cf 等比赛中可以 hack 别人的 unordered_map 代码，自定义 hash 函数可以防止针对默认 hash 造数据而超时。
缺点是常数很大。也可能超时。

```cpp
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        // http://xorshift.di.unimi.it/splitmix64.c
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
unordered_map<int, int,custom_hash> mp;
mp.reserve(1024);
mp.max_load_factor(0.25);
```

### 用 gp_hash_table 变得更快

参考文章：[浅谈 pb_ds 库及其在 OI/其他算竞中的应用](https://zhuanlan.zhihu.com/p/648274705)
gp_hash_table 使用探测法，常数小，加入自定义hash后依然很快。综合最优。

```cpp
#include <bits/extc++.h>

struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        // http://xorshift.di.unimi.it/splitmix64.c
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
__gnu_pbds::gp_hash_table<int, int,custom_hash> mp;
```

