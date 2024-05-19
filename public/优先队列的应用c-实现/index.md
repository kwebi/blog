# 优先队列的应用C++实现


优先队列可以用堆来实现, 堆底层可以用数组表示，
通过索引关系，可以表示成一颗二叉完全树

<!--more-->

C++的STL提供了相应的容器适配器
包含在`queue`头文件中


下面通过一道题来看如何使用它
#### [给定一个字符串，请将字符串里的字符按照出现的频率降序排列。](https://leetcode-cn.com/problems/sort-characters-by-frequency/description/)

```cpp
string frequencySort(string s) {
}
```

首先，统计字符出现的频率，通过map容器可以很简单的统计出来

```cpp
map<char, int> mp;

for (auto e : s)
{
    ++mp[e];
}
```

然后我们需要构建一个优先队列，而且要指定优先队列的排序方式
因此我们定义了一个自己的结构体, 并定义了`<`操作符(降序定义小于号，升序大于号)，

```cpp
struct Node
{
    Node(const pair<char, int> &val) : p(val) {}
    pair<char, int> p;
};

bool operator<(const Node &a, const Node &b)
{
    return a.p.second < b.p.second;
}
```

然后把键值对放入优先队列中

```cpp
priority_queue<Node, vector<Node>, less<Node>> pq;
for (auto e : mp)
{
    pq.push(make_pair(e.first, e.second));
}
```

要用的时候，依次取出来就是了，每次取出的都是里面最大(或最小)的

```cpp
string res;
while (!pq.empty())
{
    for (int i = 0; i < pq.top().p.second; ++i)
        res.push_back(pq.top().p.first);
    pq.pop();
}
```

还有好几个类似的题，都可以用这种方式解决

比如 :
+ [leetcode-692](https://leetcode-cn.com/problems/top-k-frequent-words)
+ [leetcode-378](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix)
+ [leetcode-373](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums)
+ [leetcode-347](https://leetcode-cn.com/problems/top-k-frequent-elements)


Python中也有相应的库提供类似的功能，`《Python CookBook》`中就有提到

```python
>>> import heapq
>>> nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
>>> print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
>>> print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]
```

+ 这些方法是基于堆数据结构实现的，对于查找的元素个数较少的时候比较合适

+ 如果仅仅想查找最大或者最小（N=1）的元素的话，用min()或者max()比较合适

+ 如果 N 的大小和集合大小接近的时候，通常先排序这个集合然后再使用切片操作会更快点

`>>> sorted(items)[:N]`

#### 下面是堆的实现, 还是建议掌握的

```cpp
#include <vector>
using namespace std;

template <class T>
class Heap
{
  public:
    Heap(size_t maxElems)
    {
        h = new HeapStruct;
        h->Elems = new T[maxElems + 1];
        h->Capacity = maxElems;
        h->size = 0;
    }
    ~Heap()
    {
        destroy();
    }
    void insert(T x)
    {
        size_t i;
        if (isFull())
        {
            return;
        }
        for (i = ++h->size; i / 2 > 0 && h->Elems[i / 2] > x; i /= 2)
        {
            h->Elems[i] = h->Elems[i / 2];
        }
        h->Elems[i] = x;
    }
    T deleteMin()
    {
        size_t i, child;
        T minElems, lastElems;
        if (isEmpty())
            return h->Elems[0];
        minElems = h->Elems[1];
        lastElems = h->Elems[h->size--];
        for (i = 1; i * 2 <= h->size; i = child)
        {
            child = i * 2;
            if (child != h->size && h->Elems[child + 1] < h->Elems[child])
                ++child;
            if (lastElems > h->Elems[child])
                h->Elems[i] = h->Elems[child];
            else
                break;
        }
        h->Elems[i] = lastElems;
        return minElems;
    }
    bool isFull()
    {
        return h->size == h->Capacity;
    }
    bool isEmpty()
    {
        return h->size == 0;
    }
    T findMin()
    {
        return h->Elems[1];
    }

  private:
    void destroy()
    {
        delete h->Elems;
        delete h;
    }
    void makeEmpty() {}

    struct HeapStruct
    {
        size_t Capacity;
        size_t size;
        T *Elems;
    };
    HeapStruct* h;
};
```
