# 右值引用与移动语义


什么是右值？在C++中，一种被广泛认可的说法是，不能取地址，没有名字的就是右值，通常位于等号右边，相反，位于等号左边的，能取地址，有名字的被称为左值。

<!--more-->

```
a = b + c
```

例如上式中，a就是个左值，b+c则是右值。

C++11又将右值分为纯右值和将亡值。纯右值包括：不跟对象关联的字面值，一些运算表达式(如1+3)。将亡值是跟右值引用相关的表达式，比如右值引用`T&&`函数的返回值，`std::move`的返回值。

右值引用就是对一个右值进行引用的类型。右值引用主要是为了移动语义，而移动语义则需要右值是可以被更改的，这也是为什么不用常量引用。只有绑定右值的引用类型，就能够延长右值的生命期。

`std::move`的功能是将左值强制转换为右值引用。

接下来看个例子：

```cpp
#include <iostream>
using namespace std;

class HugeMem
{
  public:
    HugeMem(int size) : sz(size > 0 ? size : 1)
    {
        c = new int[sz];
    }
    ~HugeMem() { delete[] c; }
    HugeMem(HugeMem &&h) : sz(h.sz), c(h.c)
    {
        h.c = nullptr;
    }
    int sz;
    int *c;
};

class Moveable
{
  public:
    Moveable() : i(new int(3)), h(1024) {}
    ~Moveable() { delete i; }
    Moveable(Moveable &&m) : i(m.i), h(move(m.h))
    {
        m.i = nullptr;
    }
    int *i;
    HugeMem h;
};

```

上述代码中，强制将m.h转化为右值，从而实现移动构造。对于一些简单的，不含任何资源的的类型来说，就无需实现移动语义了。有了移动语义，可以实现高性能的置换函数。

```cpp
template <class T>
void swap(T &a, T &b)
{
    T tmp(move(a));
    a = move(b);
    b = move(tmp);
}
```
