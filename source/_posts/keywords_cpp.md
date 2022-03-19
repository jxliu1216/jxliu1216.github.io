---
title: C++关键字
date: 2021-12-21 22:12:13
tags:
- C/C++
categories: C/C++
---

### 1. sizeof
**作用：**统计数据类型所占内存大小
**语法：**sizeof(数据类型 / 变量)


### 2. const

**const修饰普通变量**

使用cosnt修饰普通变量意味着该变量在后续的使用中是不可以被修改的（即只读的）。**因此，在使用const修饰普通变量时，必须要对其进行初始化。**

**const修饰指针**

```c++
// 常量指针：指针的指向可以修改，指针指向的值不可以修改
int a, b;
const int* p1 = &a;
p1 = &b;   // 正确，指针指向可以修改
*p1 = 1000;   // 错误，指针指向的值不可以修改

// 指针常量：指针的指向不可以修改，指针指向的值可以修改
int* const p2 = &a;
p2 = &b;     // 错误，指针的指向不可以修改
*p2 = 2000;  // 正确，指针指向的值可以修改

// 常量指针&指针常量：指针的指向和指针指向的值均不可以修改
const int* const p3 = &a;
```

**const修饰引用**

使用const修饰引用后，就无法使用引用来修改被引用对象的值。因此，常引用的作用就是防止被引用对象被错误修改。

```c++
int a = 100;
const int &b = a;    // b为a的常引用
b = 50;    // 错误，b为常引用，不能修改
a = 50;    // 修改被引用对象a后，引用b的值也会改变
```

**cosnt与#define的区别**

- #define为宏定义，在预处理阶段进行处理，是简单的替换，没有类型检查
- const在编译阶段进行处理，会进行类型检查

### 3. constexpr

constexpr表明被修饰的对象是在编译阶段就可以被确定的，而const仅能保障被修饰的对象在运行阶段不被修改。由于constexpr表示被其修饰的对象在编译器就是能确定下来的常量，计算该对象所依赖的东西同样在编译器就能确定下来。

**constexpr修饰变量**

```c++
constexpr int a = 100;
constexpr int b = a + 100;
int num = 10;
constexpr int c = num + 10;    // 错误，num并不是常量，不能用来初始化c
```

**constexpr修饰指针**

使用constexpr修饰指针时，该指针指向的对象必须有固定的地址，即在编译阶段即可确定地址。需要注意的是，constexpr修饰的是指针本省，只能放在最前面。

```c++
const int num = 100;

int main() {
    int
    constexpr int * p = &num;
}
```

**constexpr修饰函数**

```c++
constexpr int sum(int num) {
    return num + 100;
}

int main() {
    constexpr int a = sum(100);
    int b = 10;
    constexpr int c = sum(b);    // 错误，b不是常量
    return 0;
}
```

### 4. volatile

volatiel提醒编译器它后面所定义的变量随时都有可能发生变化，因此，编译后的程序每次需要存储或读取这个变量的时候，都会直接从变量所在的地址进行读取。如果没有volatiel关键字，编译器在编译时可能会对程序进行优化，可能会暂时使用寄存器中的值。如果这个变量被别的程序修改了，将会出现不一致的现象。

### 5. nullptr

空指针，可以用于指针的初始化，但是不可对其访问（nullptr实际上就是0，内存0为操作系统占用内存，应用程序无权限访问）。

```c++
int* p = nullptr;
std::cout << p << std::endl;      // 输出0
*p = 100;    // 报错，Linux平台显示Segmentation Fault
```

**注意：**对于创建的指针变量，必须对其进行初始化，可以使用nullptr对指针变量进行初始化。

### 6. extern

**修饰变量**

extern可以用来修饰变量。例如：要在b.c中使用a.c中定义变量，则可以在b.c中使用extern。

```c++
// a.c
int num = 100;
int main() {
    ...
}

// b.c
extern int num;   // 表明num已经在其他源文件中有过定义

int func() {
    ...
}
```
需要注意的时，并不是所有变量都可以在其他源文件中使用extern进行引用。能够被引用的变量一定是具有外部链接性的，外部链接性可以理解为要能够被别的源文件“看到”。因此，全局变量通常可以在别的源文件中使用extern进行应用，而局部变量和static修饰的变量通常无法在别的文件中使用extern进行引用。

**修饰函数**

除了修饰变量，extern还可以用来修饰函数。extern修饰函数和修饰变量类似，可以通过使用extern使用另外一个源文件中定义的函数。

```c++
// a.c
int func1() {
    ...
}

// b.c
extern func1();

int fun2() {
    ...
    func1();
    ...
}
```

使用头文件包含同样可以实现在别的源文件调用，那和extern有什么区别呢？使用头文件包含的话，有预处理的过程，而使用extern的话，需要使用哪个函数就引用哪个函数。在大型软件开发过程中，这种编译效率上的差异是非常明显的。

**extern "C"**

在C＋＋中调用C库函数，就需要在C＋＋程序中用extern “C”声明要引用的函数。

