---
title: STL容器之vector
date: 2021-11-15 22:03:37
tags:
- C++
- STL
- Vector
categories: STL
---

### 1. Vector

vector的函数原型如下：

```c++
template < class T, class Alloc = allocator<T> > class vector;
```

vector可以采用如下方式进行声明：

<!--more-->

```c++
#include <vector>
using namespace std;

vector<int> value1;                  // 仅声明，不设置大小和初值
vector<int> value2 = {0,1,2,3};      // 声明，并赋指定初值
vector<int> value3(100);             // value的大小为100，元素数值初始为0
vector<int> value4(100, 5);          // value的大小为100，元素全部初始化为5
vector<int> value5(value2);          // 声明，并使用一个已经存在的vector赋初值
vector<int> value6(value4.begin(), value4.end());   // 声明，使用迭代器赋初值
```

vector是一个大小可变的序列化容器，它使用连续的存储空间，因此，可以使用相对于起始位置的偏移量来访问元素。vector常用的成员函数如下：

| 成员函数    | 作用                               |
| ----------- | ---------------------------------- |
| size()      | 返回vector中的元素个数             |
| capacity()  | 返回vector的容量                   |
| resize()    | 变更vector中的元素格式             |
| reserve()   | 变更vector的容量                   |
| empty()     | 检测vector是否为空，1为空，0为非空 |
| push_back() | 在末尾插入元素                     |
| pop_back()  | 删除末尾的元素                     |
| insert()    | 在指定pos插入元素                  |
| erase()     | 删除指定pos的元素                  |
| clear()     | 删除vector中的所有元素             |
| begin()     | 返回指向vector开头的迭代器         |
| end()       | 返回指向vector末尾的迭代器         |

### 2. size()，capacity()，resize()和reserve()

![image-20211115223606494](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211115223606494.png)

vector在进行空间分配时，可能会分配多余的空间以适应元素数量的动态变化。因此，vector的空间可以分为两个部分，一部分是已存储元素占用的空间，size()返回的就是这部分元素的数量；另一部分是未用空间，capacity()返回的是整个vector的容量（即已用空间和未用空间加在一起可以存储的元素数量）。

resize()改变的是元素数量，reserve()改变的是vector的总容量

示例如下：

```c++
// resizing vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some initial content:
  for (int i=1;i<10;i++) myvector.push_back(i);

  myvector.resize(5);
  myvector.resize(8,100);
  myvector.resize(12);

  std::cout << "myvector contains:";
  for (int i=0;i<myvector.size();i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```

上面的程序的运行结果为：

```bash
myvector contains: 1 2 3 4 5 100 100 100 0 0 0 0
```

### 3. vector的动态扩容机制

在添加新元素时，如果vector的空间大小不足，**则会使用一个固定的系数（>1）乘以当前空间大小配置一块较大的空间，然后将原空间内容拷贝过来，在新空间的末尾添加新元素，并释放原空间。**

（注：一般情况下，在vector末尾插入元素的时间复杂度为常数级，但是当超出vector的容量时，情况有一些特殊。）

可以通过如下代码验证：

```c++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec;
    for(int i = 0; i <= 7; i++) {
        vec.push_back(i);
        std::cout << "size = " << vec.size() << "; capacity = " << vec.capacity() << std::endl;
    }
    return 0;
}
```

运行结果如下图所示：

<img src="https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211115225818428.png" alt="image-20211115225818428" style="zoom: 80%;" />

从上图可以发现，vector以比例系数为2进行动态扩容。
