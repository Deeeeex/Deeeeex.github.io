---
layout:     post
title:      STL函数
subtitle:   C++学习笔记（六）
date:       2021-08-18
author:     Chen Jinhao
header-img: img/cup-of-coffee-1280537_1920.jpg
catalog: true
tags:
    - C++学习笔记
---
### STL函数（Mooc笔记）

### 1.排序算法sort

（1）对基本类型的数组从小到大排序：

**sort(数组名+n1,数组名+n2);**

n1和n2都是int类型的表达式，可以包含变量；

将数组中下标范围为[n1,n2）的元素从小到大排序（下标为n2的元素不在排序区间内）；

（2）对元素类型为T的基本类型数组从大到小排序：

**sort(数组名+n1,数组名+n2,greater<int>());**

（3）用自定义的排序规则，对任何类型T的数组排序

**sort(数组名+n1,数组名+n2,排序规则结构名());**

排序规则结构的定义方式格式如下：

```c++
struct name
{
	bool operator()(const T &a1,const T &a2)const{
	//若a1应该在a2前面，则返回true
	//否则返回false
	}
}
```

 例如

```c++
struct Rule1//按从大到小排序
{
    bool operator()(const int &a1,const int &a2)const
    {
        return a1>a2;
	}
};
struct Rule2//按个位数从小到大排序
{
    bool operator()(const int &a1,const int &a2)
    {
        return a1%10<a2%10;
	}
}
```

实例

```c++
/*用sort对结构数组进行排序*/
#include <bits/stdc++.h>
using namespace std;
struct Student
{
    char name[20];
    int id;
    double gpa;
};
Student students[]={
    {"Jack",112,3.4},{"Mary",102,3.8},{"Mary",117,3.9},
    {"Ala",333,3.5},{"Zero",101,4.0}};
struct StudentRule1{//按姓名从小到大排
    bool operator()(const Student &s1,const Student &s2)const{
        if(stricmp(s1.name,s2.name)<0)//stricmp函数表示不区分大小写的字符串比较
        return true;
        return false;
    }
};
struct StudentRule2
{ //按id从小到大排
    bool operator()(const Student &s1, const Student &s2) const
    {
        return s1.id<s2.id;
    }
};
struct StudentRule3
{ //按gpa从高到低排
    bool operator()(const Student &s1, const Student &s2) const
    {
        return s1.gpa > s2.gpa;
    }
};
void PrintStudents(Student s[],int size)
{
    for(int i=0;i<size;i++)
    {
        cout<<"("<<s[i].name<<","<<s[i].id<<","<<s[i].gpa<<")";
    }
    cout<<endl;
}
int main()
{
    int n=sizeof(students)/sizeof(Student);
    sort(students,students+n,StudentRule1());
    PrintStudents(students,n);
    sort(students, students + n, StudentRule2());
    PrintStudents(students, n);
    sort(students, students + n, StudentRule3());
    PrintStudents(students, n);
    return 0;
}
```

### 2.二分查找算法

STL提供在排好序的数组上进行二分查找的算法

- binary_search
- lower_bound
- upper_bound

#### (1).用binary_search进行二分查找（用法一）

**binary_search(数组名+n1,数组名+n2,值);**

在已经排好序的数组中查找区间为下标范围为[n1,n2)的元素，返回值为true(找到)或false(未找到)

在这里“等于”的含义是:a<b和b<a都不成立

#### (2).用binary_search进行二分查找（用法二）

在用自定义排序规则排好序的、元素为任意的T类型的数组中进行二分查找

**binary_search(数组名+n1,数组名+n2,值,排序规则结构名())；**

查找区间同(1)

在这里“等于”的含义是：“a必须在b前面”和“b必须在a前面”都不成立

#### (3).用lower_bound二分查找下界（用法一）

在对元素类型为T的从小到大排好序的基本类型的数组中进行查找

**T * lower_bound(数组名+n1,数组名+n2,值);**

返回一个指针 T * p;

*p是查找区间里下标最小的，大于等于“值”的元素。如果找不到，p指向下标为n2的元素

#### (4).用lower_bound二分查找下界（用法二）

在元素为任意的T类型、按照自定义排序规则的数组中进行查找

**T * lower_bound(数组名+n1,数组名+n2,值,排序规则结构名());**

返回一个指针T * p;

*p是查找区间里下标最小的，按自定义规则排序，可以排在“值”后面的元素。如果找不到，p指向下标为n2的元素

#### (5).用upper_bound二分查找上界 两种 用法类似lower_bound：略

返回一个指针 T* p;

*p是查找区间里下标最小的，大于“值”的元素（必须排在“值”后面的数）。如果找不到，p指向下标为n2的元素 

### 3.STL里的平衡二叉树结构

#### (1).multiset用法

multiset<T> st;

定义了一个multiset变量st，st里面可以存放T类型的数据，并且能自动排序（开始时st为空）

排序规则：表达式“a<b”为true,则a排在b前面

可用st.insert添加元素，st.find查找元素，st.erase删除元素，复杂度都是log(n);

用法示例：

```c++
#include <iostream>
#include <cstring>
#include <set> //使用multiset和set需要此头文件
using namespace std;
int main()
{
    multiset<int> st;
    int a[10] = {1, 14, 12, 13, 7, 13, 21, 19, 8, 8};
    for (int i = 0; i < 10; i++)
        st.insert(a[i]);       //插入的是a[i]的复制品
    multiset<int>::iterator i,j; //迭代器，近似于指针
    for (i = st.begin(); i != st.end(); ++i)
        cout << *i << ",";
    cout << endl;
    i = st.find(22);
    if (i == st.end())
        cout << "not found" << endl;
    st.insert(22);
    i = st.find(22);
    if (i == st.end())
        cout << "not found" << endl;
    else
        cout << "found:" << *i << endl;
    i = st.lower_bound(13);
    cout << *i << endl;
    i = st.upper_bound(8);
    cout << *i << endl;
    j=st.erase(i);
    //erase函数参数应该是一个指向需要被删除的元素的迭代器，返回值为删除过后紧随其后的元素对应的迭代器
    cout<<*j<<endl;
    for (i = st.begin(); i != st.end(); i++)
        cout << *i << ",";
    return 0;
}
```

multiset<T> ::iterator p;

- p是迭代器，相当于指针，可用于指向multiset中的元素。访问multiset中的元素要经过迭代器
- 与指针不同的地方在于：multiset上的迭代器可++,--,用!=和==比较，不可比大小，不可加减整数，不可相减

####  (2).自定义排序规则的multiset用法

```c++
#include <iostream>
#include <cstring>
#include <set>
using namespace std;
struct Rule1{
    bool operator()(const int & a,const int &b){
        return (a%10)<(b%10);
    }//返回值为true说明a必须排在b前面
};
int main()
{
    multiset<int,greater<int>> st;
    int a[10]={1,14,12,13,7,13, 21,19,8,8};
    for(int i=0;i<10;++i)
        st.insert(a[i]);
    multiset<int,greater<int>>::iterator i;
    for(i=st.begin();i!=st.end();i++)
        cout<<*i<<",";
    cout<<endl; 
    multiset<int,Rule1> st2;
    for(int i=0;i<10;i++)
    {
        st2.insert(a[i]);
    }
    multiset<int,Rule1>::iterator p;
    for(p=st2.begin();p!=st2.end();p++)
        cout<<*p<<",";
    cout<<endl;
    p=st2.find(133);//find(x)的意义是找到一个y,使得“y必须在x前面”和“x必须在y前面”都不成立
    cout<<*p<<endl;
    return 0;
}
```

#### (3).set的用法

- set和multiset的区别在于容器里不能有重复元素

  a和b重复：“a必须排在b前面”和“b必须排在a前面”都不成立

- set插入元素可能不成功

```c++
#include <iostream>
#include <cstring>
#include <set> 
using namespace std;
int main()
{
    set<int> st;
    int a[10] = {1,2,3,8,7,7,5,6,8,12};
    for (int i = 0; i < 10; i++)
        st.insert(a[i]);
    cout<<st.size()<<endl;//输出：8
    set<int>::iterator i;
    for (i = st.begin(); i != st.end(); ++i)
        cout << *i << ",";//输出：1,2,3,5,6,7,8,12
    cout << endl;
    pair<set<int>::iterator,bool> result=st.insert(2);
    //这里的pair相当于一个结构体，包含了一个迭代器和一个bool表示是否插入成功
    if(!result.second)//条件不成立说明插入不成功
        cout<<*result.first<<"already exists."<<endl;
    else
        cout<<*result.first<<"inserted."<<endl;
    return 0;
}
```

pair模板的用法实例

pair<int,double> a;

等价于：

```c++
struct{
	int first;
	double second;
};
a.first=1;
a.second=93.93;
```

#### (4).multimap的用法

multimap容器里的元素都是pair形式的

multimap<T1,T2>mp;

则mp里的元素都是以下类型:

```c++
struct {
	T1 first;//关键字
	T2 second;//值
}
```

multimap中的元素按照first排序，并可以按first进行查找

缺省的排序规则是“a.first<b.first”为true,则a排在b前面

应用实例：找出比设定分数低的所有学生中分数最高的，并输出其名字和Id

```c++
#include <iostream>
#include <map>//使用multimap和map需要包含此头文件
#include <cstring>
using namespace std;
struct StudentInfo{
    int id;
    char name[20];
};
struct Student
{
    int score;
    StudentInfo info;
};
typedef multimap<int,StudentInfo> MAP_STD;
//此后MAP_STD等价于multimap<int,StudentInfo>
typedef int* PINT;
int main()
{
    MAP_STD mp;
    Student st;
    char cmd[20];
    while(cin>>cmd)
    {
        if(cmd[0]=='A')
        {
            cin>>st.info.name>>st.info.id>>st.score;
            mp.insert(make_pair(st.score,st.info));
        }//make_pair生成一个pair<int,StudentInfo>变量
        //其first等于st.score,second等于st.info
        else if(cmd[0]=='Q')
        {
            int score;
            cin>>score;
            MAP_STD::iterator p=mp.lower_bound(score);
            if(p!=mp.begin())
            {
                p--;
                score =p->first;//比查询分数低的最高分
                MAP_STD::iterator maxp=p;
                int maxId=p->second.id;
                for(;p!=mp.begin()&&p->first==score;p--)
                {//遍历所有成绩和score相等的学生，找出Id最大的学生
                    if(p->second.id>maxId)
                    {
                        maxp=p;
                        maxId=p->second.id; 
                    }
                }
                if(p->first==score)//如果上一个for循环的结束是由于p==mp.degin()而终止且p->first恰好也等于score则需要补充判断 
                {
                    if (p->second.id > maxId)
                    {
                        maxp = p;
                        maxId = p->second.id;
                    }
                }
                cout<<maxp->second.name<<" "
                <<maxp->second.id<<" "
                <<maxp->first<<endl;
            }
            else cout<<"Nobody"<<endl;//lower_bound返回begin,说明没有比查询分数低的学生
        }
    }
    return 0;
}
```

#### (5).map的用法

于multimap的区别在于：

- 不能有关键字重复的元素
- 可以使用[],下标为关键字，返回值为first和关键字相同的元素的second
- 插入元素可能失败

应用实例1:

```c++
#include <iostream>
#include <map>
#include <string>
using namespace std;
struct Student{
    string name;
    int score;
};
Student student[5]={
{"Jack",89},{"Tom",74},{"Cindy",87},
{"Alysa",87},{"Micheal",98}};
typedef map<string,int> MP;
int main()
{
    MP mp;
    for(int i=0;i<5;i++)
        mp.insert(make_pair(student[i].name,student[i].score));
    cout<<mp["Jack"]<<endl;//输出89
    mp["Jack"]=60;//修改名为“Jack”的元素的second
    for(MP::iterator i=mp.begin();i!=mp.end();i++)
        cout<<"("<<i->first<<","<<i->second<<")";
    cout<<endl;
    Student st;
    st.name="Jack";
    st.score=99;
    pair<MP::iterator,bool> p=mp.insert(make_pair(st.name,st.score));
    if(p.second)
        cout << "(" << p.first->first << "," << p.first->second << ")";
    else
        cout<<"insertion failed"<<endl;
    mp["Harry"]=78;//插入一元素，其first为"Harry",然后将其second改为78
    MP::iterator q=mp.find("Harry");
    cout << "(" << q->first << "," << q->second << ")" << endl;
    //输出(Harry,78)
    return 0;
}
```

应用实例2：统计词频

```c++
#include <iostream>
#include <set>
#include <map>
#include <string>
using namespace std;
struct Word{
    int times;
    string wd;
};
struct Rule{
    bool operator()(const Word & w1,const Word & w2)
    {
        if(w1.times!=w2.times)
        return w1.times>w2.times;
        else return w1.wd<w2.wd;
    }
};
int main()
{
    string s;
    set<Word,Rule> st;//set用于统计每个单词的出现次数，且排序规则是词频
    map<string,int> mp;//map用于查找单词本身，所以排序顺序是按照字典序
    while(cin>>s)
    ++mp[s];
    for(map<string,int>::iterator i=mp.begin();
    i!=mp.end();i++){
        Word tmp;
        tmp.wd=i->first;
        tmp.times=i->second;
        st.insert(tmp);
    }
    for(set<Word,Rule>::iterator i=st.begin();
    i!=st.end();i++){
        cout<<i->wd<<" "<<i->times<<endl;
    }
    return 0;
}
```

