---
layout:      post
title:      "C++中STL的模板类"
date:       2021-05-15
author:     "颜杰东"
catalog:     true
tags:
     - STL
---

## 一、Vector

### 1、简介：

​	vector是一个大小可变的**动态数组（变长数组）**，和数组一样，他能对数组中的任意元素根据**索引**来**快速访问**。与普通数组不同的是，它的大小是可以动态变化的。当元素不断增加，超过vector自身容量后，vector会申请一块更大的内存空间（据参考资料这块内存空间是原来的一倍），把现有vector中的元素复制过去，然后销毁原来的内存。

### 2、怎么用它？

​	使用vector，需要引入头文件`#incude<vector>`,然后，我们用`vector<typeName> vt(n_elem);`来声明一个名为v的对象，这个对象可以储存n_elem个类型为typename的元素，式中n_elem也可以不写，这样初始长度默认为0，例子如下：

```c++
#include<vector>
using namespace std;
int main(){
    vector<int> v1;//创建一个存放int型的vector v1，初始长度为0
    vector<double> v2(20);//创建一个存放double型的vector v1，初始长度为10
}
```

Q：类型可以是类（如string）吗？

A：可以，如`vector<string> v3;`创造了一个string的vector

### 3、Vector的基本方法

```c++
//1.构造函数
vector()//:创建一个空vector
vector(int nSize)//:创建一个vector,元素个数为nSize
vector(int nSize,const t& t)//:创建一个vector，元素个数为nSize,且值均为t
vector(const vector&)//:复制构造函数
vector(begin,end)//:复制[begin,end)区间内另一个数组的元素到vector中
//2.增加函数
void push_back(const T& x)//:向量尾部增加一个元素X
iterator insert(iterator it,const T& x)//:向量中迭代器指向元素前增加一个元素x
iterator insert(iterator it,int n,const T& x)//:向量中迭代器指向元素前增加n个相同的元素x
iterator insert(iterator it,const_iterator first,const_iterator last)//:向量中迭代器指向元素前插入另一个相同类型向量的[first,last)间的数据
//3.删除函数
iterator erase(iterator it)//:删除向量中迭代器指向元素
iterator erase(iterator first,iterator last)//:删除向量中[first,last)中元素
void pop_back()//:删除向量中最后一个元素
void clear()//:清空向量中所有元素
//4.遍历函数
reference at(int pos)//:返回pos位置元素的引用
reference front()//:返回首元素的引用
reference back()//:返回尾元素的引用
iterator begin()//:返回向量头指针，指向第一个元素
iterator end()//:返回向量尾指针，指向向量最后一个元素的下一个位置
reverse_iterator rbegin()//:反向迭代器，指向最后一个元素
reverse_iterator rend()//:反向迭代器，指向第一个元素之前的位置
//5.判断函数
bool empty() const//:判断向量是否为空，若为空，则向量中无元素
//6.大小函数
int size() const//:返回向量中元素的个数
int capacity() const//:返回当前向量所能容纳的最大元素值
int max_size() const//:返回最大可允许的vector元素数量值
//7.其他函数
void swap(vector&)//交换两个同类型向量的数据
void assign(int n,const T& x)//设置向量中前n个元素的值为x
void assign(const_iterator first,const_iterator last)//向量中[first,last)中元素设置成当前向量元素
```

注：vector中可以用>,<,==,!=等按字典序比较两个vector的大小。



Q： iterator迭代器是什么？

A： 迭代器是一个广义的指针，它可以执行类似指针的操作，如解引用和递增。

迭代器使用举例：

```c++
vector<double>::iterator pd;//声明一个迭代器
vector<double> scores;
//可对迭代器操作如下
pd = scores.begin();//让pd指向scores的首元素
*pd = 22.3;//给首元素赋值
pd++;//让pd指向scores的下一个元素
```

### 4.使用举例

```C++
#include<vector>
#include<iostream>
using namespace std;

//用迭代器遍历查看v1内的元素
void display(vector<int> v1){
    vector<int>::iterator p;
    for(p = v1.begin();p != v1.end();p++){
        cout<<*p<<" ";
    }
    cout<<endl;
}

int main(){

    vector<int> v1;
    cout<<"size:"<<v1.size()<<" ";

//增
    v1.push_back(1);
    v1.push_back(2);
    v1.push_back(3);
    v1.push_back(4);
    cout<<v1.size()<<endl;

//可以直接使用索引查看v1内的元素
    for(int i = 0; i < v1.size(); i ++){
        cout<<v1[i]<<" ";
    }
    cout<<endl;

//删
    v1.pop_back();//删除尾部元素
    display(v1);

    vector<int>::iterator front = v1.begin();
    v1.erase(front+1,front+2);//删除头结点后一个节点开始，注意，erase删除的区间是前闭后开
    display(v1);

//改
//可直接通过索引修改vector的元素
    v1[1] = 66;
    display(v1);

//vector支持反向遍历
    for(vector<int>::reverse_iterator rp=v1.rbegin(); rp != v1.rend(); rp++){
        cout<<*rp<<" ";
    }
    return 0;

    // 输出
    // size:0 4
    // 1 2 3 4
    // 1 2 3
    // 1 3
    // 1 66
    // 66 1
}
```

## 5、实战例题

https://www.luogu.com.cn/problem/P1540

https://www.luogu.com.cn/problem/P1113



## 二、Stack&&Queue

### 1、简介：

Stack（堆栈）是一种后进先出（Last in First out）的线性表，它只能在一端(称为栈顶(top))对数据项进行插入和删除。

Queue（队列）是一种先进先出（First in First out）的线性表表，它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。

### 2、怎么用它？

例子如下：

```c++
#include<stack>
#include<queue>
#include<string>

using namespace std;
int main(){
    stack<int> s1;//创建一个存放int型的stack s;
    queue<double> q;//创建一个存放double型的queue q;
    stack<string> s2;//创建一个存放string类的stack s;
}
```

### 3、具体方法

#### 堆栈：

- **top()**：返回一个栈顶元素的引用，类型为 T&。如果栈为空，返回值未定义。
- **push(const T& obj)**：可以将对象副本压入栈顶。这是通过调用底层容器的 push_back() 函数完成的。
- **push(T&& obj)**：以移动对象的方式将对象压入栈顶。这是通过调用底层容器的有右值引用参数的 push_back() 函数完成的。
- **pop()**：弹出栈顶元素。
- **size()**：返回栈中元素的个数。
- **empty()**：在栈中没有元素的情况下返回 true。
- **emplace()**：用传入的参数调用构造函数，在栈顶生成对象。
- **swap(stack & other_stack)**：将当前栈中的元素和参数中的元素交换。参数所包含元素的类型必须和当前栈的相同。对于 stack 对象有一个特例化的全局函数 swap() 可以使用。

#### 队列：

| 元素访问                                                     |                                        |
| ------------------------------------------------------------ | -------------------------------------- |
| [front](https://www.apiref.com/cpp-zh/cpp/container/queue/front.html) | 访问第一个元素  (公开成员函数)         |
| [back](https://www.apiref.com/cpp-zh/cpp/container/queue/back.html) | 访问最后一个元素  (公开成员函数)       |
| 容量                                                         |                                        |
| [empty](https://www.apiref.com/cpp-zh/cpp/container/queue/empty.html) | 检查底层的容器是否为空  (公开成员函数) |
| [size](https://www.apiref.com/cpp-zh/cpp/container/queue/size.html) | 返回容纳的元素数  (公开成员函数)       |
| 修改器                                                       |                                        |
| [push](https://www.apiref.com/cpp-zh/cpp/container/queue/push.html) | 向队列尾部插入元素  (公开成员函数)     |
| [emplace](https://www.apiref.com/cpp-zh/cpp/container/queue/emplace.html)(C++11) | 于尾部原位构造元素  (公开成员函数)     |
| [pop](https://www.apiref.com/cpp-zh/cpp/container/queue/pop.html) | 删除首个元素  (公开成员函数)           |
| [swap](https://www.apiref.com/cpp-zh/cpp/container/queue/swap.html) | 交换内容  (公开成员函数)               |

注：stack和queue同样可以用>,<,==,!=等按字典序比较两个stack和queue的大小。

其中，stack是从栈底开始比较，queue是从队首开始比较

```c++
	s[0].push(1);
    s[0].push(2);
    //top 2 1
    s[1].push(2);
    s[1].push(1);
    //top 1 2
    cout<<(s[0]>s[1])<<endl;

    q[0].push(1);
    q[0].push(2);
    //front 1 2 rear;
    q[1].push(2);
    q[1].push(1);
    //front 2 1 rear;
    cout<<(q[0]>q[1])<<endl;

//输出为 0 0
```

这里面，emplace的用法其实与push相差不大，一些区别可以参见https://blog.csdn.net/Kprogram/article/details/82055673?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control

### 4、实战例题

https://www.luogu.com.cn/problem/CF911A

广度优先搜索的所有题都可以使用队列。



## 三、map

## 1、简介

如何统计一串字符串中各个字符的数量？

一个比较巧妙的方法是建立一个数组，进行如下操作：

```c++
int main(){
    string s;
    cin>>s;
    int a[256] = {0};
    for(int i = 0; i < s.length();i++){
        a[s[i]]++;
    }
}
```

a[s[i]]++其实是利用数组，建立了一个字符的ASCII值（s[i]）与对应字符数量的**映射**，我们把s[i]称作**key**（键），a[s[i]]称作key对应的**value**（值）。但利用数组建立的这种映射是有局限的，比如key只能是整型（或者类似整型的字符型）。

c++提供了map模板类来帮助我们建立更强大的映射，map的映射不局限于整型，它可以建立字符串到整型，结构体到字符型等等更强大的映射（注意结构体作为key时候需要重载操作符<，可以参考https://blog.csdn.net/fengfengdiandia/article/details/87403713)。

## 2、怎么用它？

```c++
#include<map>//使用map，需要include map头文件
using namespace std;

int main(){
    //创建map对象
	map<string,int> myMap;//前面（string）是key，后面（int）是value
    
    //创建map迭代器，指向map第一个元素
   	map<string,int>::iterator myMapIter = myMap.begin();
    可以用myMapIter->first获取iter指向的map元素的key
       用myMapIter->second获取iter指向的map元素的value
}
```



## 3、具体方法

     begin()         返回指向map头部的迭代器
    
     clear(）        删除所有元素
    
     count()         返回指定元素出现的次数
    
     empty()         如果map为空则返回true
    
     end()           返回指向map末尾的迭代器
    
     equal_range()   返回特殊条目的迭代器对
    
     erase()         删除一个元素
    
     find()          查找一个元素
    
     get_allocator() 返回map的配置器
    
     insert()        插入元素
    
     key_comp()      返回比较元素key的函数
    
     lower_bound()   返回键值>=给定元素的第一个位置
    
     max_size()      返回可以容纳的最大元素个数
    
     rbegin()        返回一个指向map尾部的逆向迭代器
    
     rend()          返回一个指向map头部的逆向迭代器
    
     size()          返回map中元素的个数
    
     swap()           交换两个map
    
     upper_bound()    返回键值>给定元素的第一个位置
    
     value_comp()     返回比较元素value的函数

## 4、使用举例

具体的增删改查参见https://www.cnblogs.com/yonglin1998/p/11780828.html，写得很详细

## 5、实战例题

https://www.luogu.com.cn/problem/P1201

https://www.luogu.com.cn/problem/P1918