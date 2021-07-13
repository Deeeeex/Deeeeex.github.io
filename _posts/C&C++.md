---
layout:     post
title:      2021-07-13-C&C++
subtitle:   (副标题)
date:       2021/7/13
author:     Dexter
header-img: img/the-first.png
catalog:   true
tags:
    - 往事如烟
---
## C/C++

##### 1.用new+delete模拟申请删除一块空间，对应C中的malloc+free

```c++
#include <bits/stdc++.h>
using namespace std;
int main()//用new+delete模拟申请删除一块空间，对应C中的malloc+free
{
    int *p;
    p=new int(100);//申请一块空间并赋值为100
    if(p==NULL) cout<<"Failure!"<<endl;
    else 
    {
        cout<<*p<<endl;
        delete p;
    }
    cout << *p << endl;

    char *p2=new char[100];//申请100个char的空间
    char arr[5]="love";
    strcpy(p2,arr);
    cout<<p2<<endl;
    delete []p2;//删除整个p2数组的格式
    cout<<p2<<endl;
    return 0;
}
```

##### 2.动态内存管理基本格式

```c++
#include<bits/stdc++.h>
using namespace std;
int main()
{
    int n;
    cin>>n;
    int *p=new int[n];//动态内存管理
    for(int i=0;i<n;i++)
    {
        p[i]=i+1;
    }
    for(int i=0;i<n;i++)
    {
        cout<<p[i]<<' ';
    }
    delete []p;
    return 0;
}
```

##### 3.函数重载

> 两个以上的函数，具有相同的函数名，但是形参的个数或者类型不同，编译器会根据实参的类型及个数来自动确定调用哪一个函数（但不能以形参名字或函数返回类型不同来区分函数）。

带默认形参值得函数：调用时如果给出实参则使用实参，否则使用预先声明的默认形参值，例如

```c++
int add(int x=6,int y=5)//ps.默认形参值需靠右放
{
    return x+y;
}
int main()
{
    int ret;
    ret=add(10,20);//30
    ret=add(10);//15
    ret=add();//11
    return 0;
}
```

##### 4.内联函数 

内联函数既有宏定义的优点，又克服了宏定义的缺点；

在编译时在调用func的地方用函数体进行了替换，所以在程序执行的时候会减少调用开销，举例如下：

```c++
inline int abd(int value)
{
    return (value<0?-value:value);
}
void main()
{
    int m=-2,ret;
    ret=abs(++m);//会将++m先进行运算再传入函数而不是define中简单的替换
}
```

- 内联函数适用于频繁调用的，并且函数体较小的（只有几条语句）函数定义为内联函数。
- 内联函数内不允许有循环语句和switch语句，否则按照普通语句来处理。

> 附上内联函数与普通函数的区别：
>
> 1. 内联函数和普通函数的参数传递机制相同，但是编译器会在每处调用内联函数的地方将内联函数内容展开，这样既避免了函数调用的开销又没有宏机制的缺陷
>
> 2. 普通函数在被调用的时候，系统首先要到函数的入口地址去执行函数体，执行完成之后再回到函数调用的地方继续执行，函数始终只有一个复制。
>
>     内联函数不需要寻址，当执行到内联函数的时候，将此函数展开，如果程序中有N次调用了内联函数则会有N次展开函数代码
>
> 3. 内联函数有一定的限制，内联函数体要求代码简单，不能包含复杂的结构控制语句。如果内联函数函数体过于复杂，编译器将自动把内联函数当成普通函数来执行

##### 5.引用

初始化独立引用的几种方式

1.“=”的右端是一个变量

```c++
int a;
int &ra=a;
```

2."="的右端是一个常量

```c++
const float &r2=1.0;
```

3.定义常引用

```c++
int x=1;
const int &rx=x;
rx=98;//错误，只能使用rx，不能修改
```

C++的引用调用

```c++
void func(int &pnum)
{
    pnum++;
}
int main()
{
    int value=5;
    func(value);
    cout<<value;//输出6
    return 0;
}
```

如果一个函数返回引用，那么函数调用可以出现在赋值号的左边

```c++
int &f(int *pint)
{
    return *pint;
}
int main()
{
    int a=10,b;
    b=f(&a)*5;
    f(&a)=88;
    cout<<b<<" "<<a;//输出结果为50 88
    return 0;
}
```

需要注意的是，这里的返回值是一个存储单元的引用，如果它是一个形参（会被删除）那么就没有意义，即返回的引用的单元需要是一个生命周期比函数长的单元才可以作为“左值”；

