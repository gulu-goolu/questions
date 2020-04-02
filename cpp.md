---
author: murmur wheel
title: C++ 面试题整理
---

# C++ 编译的流程

1. 预处理，展开预编译宏。使用的 gcc 命令是：`gcc -E`
2. 编译、优化阶段，从 .cpp/.h 转换成 .s 文件。使用的 gcc 命令是：`gcc -S`
3. 汇编，从 .s 转化成 .o 文件。使用的 gcc 命令是：`gcc -c`
4. 链接，将 .o 文件转化成可执行程序。对应于链接命令：`ld`

使用 gcc 的 `-save-temps` 可以保留中间文件。

# 关键字

## struct 和 class 关键字的区别

struct 默认 public, class 默认 private。

## static 关键字

static 可以修饰类的数据成员、函数、以及局部变量。

static 修饰类的数据成员时，表示这个成员是类所有的，要通过 `<className>::<dataName>` 来访问。当其修饰函数时，表示这个函数的作用域被限定到这个编译单元内。当被用来修饰局部变量时，这个局部变量的生命周期从第一次访问开始，直到程序终止，其实际存放的位置是未初始化数据段，而不是分配在栈上。

## const 关键字

const 关键字可以修饰函数，变量以及指针。

修饰成员函数时，表示此函数只能不能写出 mutable 以外的数据成员。修饰变量是，表示不能更改变量的值。当修饰指针时，分两种情况，一种时不能修改指针本身的值，二是不能通过指针修改指针指向的内存。此外，使用 const_cast 可以更改 const 的修饰作用。

## extern 关键字

extern 可以修饰函数或者变量，表示引用别的编译单元的函数和变量，注意 extern 声明变量时不能初始化，否则会被视为定义变量。

`extern "C"`，指定 C++ 编译器以 C 的标准处理函数，方便导出给 C 语言使用。

# 构造函数相关的问题

## 什么时候编译器会生成一个默认构造函数?讲一讲它的实现细节？

编译器会在有必要的时候合成一个默认构造函数，被合成的构造函数只是满足编译器（而非程序）的需要。为了避免冲突，通常这个默认构造函数以 inline 或者 static 的形式给出。

注意：implicit trivial default constructors 实际上不会被合成出来。编译器合成的默认构造函数不会设置 class 内每一个数据成员的初始值。

## C++ 会为一个类中添加哪些函数？

1. 默认构造函数
2. 拷贝构造函数
3. 移动构造函数
4. 赋值操作符
5. 移动操作符
6. 析构函数

## 默认构造函数会做些什么？

1. 如果有子类，调用子类的默认构造函数。
2. 如果类中定义了虚函数，将虚表指针指向正确的地址。（虚表是常量，所以一般存放在只读数据段中。）
3. 如果存在虚继承，初始化虚继承相关的内容。

## 拷贝构造函数的作用？什么是位逐次拷贝？

拷贝构造函数的作用是用一个对象的数据来初始化新对象，格式为 `Class(const Class&)`。如果需要的话，编译器会为每一个类增加一个拷贝构造函数。位逐次复制就是按位复制对象。当类定义了复制构造函数时，不使用位逐次复制；当类的成员或基类中定义了复制构造函数时，不使用按位复制；当类中存在虚函数或者虚继承时，也不使用位逐次复制。

复制构造函数是怎么处理虚函数和虚继承的。

1. 对于有虚函数的情形，复制构造函数应使虚表指针指向正确的地址。
2. 对于虚继承，在复制构造函数中设置虚基类的地址。

# 数据成员

## 布局

1. 数据成员之间的布局：在同一个 access section 中，members 的排列只需符合 “较晚出现的 members 在 class object 中有较高的地址” 这一条件即可。
2. C++ 标准没有规定 vptr 放置的位置。具体是由编译器自己决定的。

## 存取

# 成员函数

# 继承

# std::move 和 std::forward

# 