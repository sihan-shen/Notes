
## 泛型（类属）程序设计
- 基于STL的编程

## 什么是STL？
- C++提供了一个基于模板实现的标准模板库（Standard Template Library，简称STL）
- STL基于模板实现了一些面向序列数据的表示及常用的操作
- STL支持了一种抽象的编程模式
  - 隐藏了一些低级的程序元素，如数组、链表、循环等
- STL由多人参与设计，C++设计者更看重C++的基于STL的编程

## STL包含什么？
### 容器
- 用于存储序列化的数据，如：向量、队列、栈、集合等

### 算法（函数）
- 用于对容器中的数据元素进行一些常用操作，如：排序、统计以及自定义操作等

### 迭代器
- 实现了抽象的指针功能，它们指向容器中的数据元素，用于对容器中的数据元素进行遍历和访问
- 迭代器是容器和算法之间的桥梁
  - 传给算法的不是容器，而是指向容器中元素的迭代器
  - 算法通过迭代器实现对容器中数据元素的访问
  - 使得算法与容器保持独立，从而提高算法的通用性

## 基于STL编程的例子
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;
void display(int x) { cout << ' ' << x; return; }
int main()
{	vector<int> v; //创建容器对象v，元素类型为int
	int x;
	cin >> x;
	while (x > 0) //生成容器v中的元素
	{	v.push_back(x); //往容器v中增加一个元素
		cin >> x;
	}
	//利用算法max_element计算并输出容器v中的最大元素
	cout << "Max = " << *max_element(v.begin(),v.end()) 
		 << endl;
   //利用算法accumulate计算并输出容器v中所有元素的和
	cout << "Sum = " << accumulate(v.begin(),v.end(),0) 
		 << endl;
	//利用算法sort对容器v中的元素进行排序
	sort(v.begin(),v.end()); 
	//利用算法for_each输出排序结果
	cout << "Sorted result is:\n";
	for_each(v.begin(),v.end(),display);
	cout << '\n';
	return 0;
}
```
- 用STL来实现上述程序的功能不需要涉及一些低级的程序元素，如数组、链表、循环，使得程序设计变得更便捷

## 容器
- 用于表示由同类型元素构成的、长度可变的元素序列
- 容器是由类模板来实现的，模板的参数是容器中元素的类型
- STL中包含了很多种容器，不同的容器适合于不同的应用场合

### STL的主要容器
#### vector<元素类型>
- 用于需要快速定位（访问）任意位置上的元素以及主要在元素序列的尾部增加/删除元素的场合
- 在头文件vector中定义，用动态数组实现

#### list<元素类型>
- 用于经常在元素序列中任意位置上插入/删除元素的场合
- 在头文件list中定义，用双向链表实现

#### deque<元素类型>
- 用于主要在元素序列的两端增加/删除元素以及需要快速定位（访问）任意位置上的元素的场合
- 在头文件deque中定义，用分段的连续空间结构实现

#### stack<元素类型>
- 用于仅在元素序列的尾部增加/删除元素的场合
- 在头文件stack中定义，可基于deque、list或vector来实现

#### queue<元素类型>
- 用于仅在元素序列的尾部增加、头部删除元素的场合
- 在头文件queue中定义，可基于deque和list来实现

#### priority_queue<元素类型>
- 与queue的操作类似，不同之处在于：每次增加/删除元素之后，它将对元素位置进行调整，使得头部元素总是最大的
- 在头文件queue中定义，可基于deque和vector来实现

#### map<关键字类型，值类型>和multimap<关键字类型，值类型>
- 用于需要根据关键字来访问元素的场合
- 容器中每个元素是一个pair结构类型，该结构有两个成员：first和second，关键字对应first，值对应second，元素是根据其关键字排序的
  - 对于map，不同元素的关键字不能相同
  - 对于multimap，不同元素的关键字可以相同
- 在头文件map中定义，常常用某种二叉树来实现

#### set<元素类型>和multiset<元素类型>
- 分别是map和multimap的特例，每个元素只有关键字而没有值，或者说，关键字与值合一了
- 在头文件set中定义

#### basic_string<字符类型>
- 与vector类似，不同之处在于其元素为字符类型，并提供了一系列与字符串相关的操作
- string和wstring分别是它的两个实例：
  - basic_string<char>
  - basic_string<wchar_t>
- 在头文件string中定义

## 容器的基本操作
- 容器类提供了一些成员函数来实现容器的基本操作，其中包括：
  - 往容器中增加元素
  - 从容器中删除元素
  - 获取容器中指定位置的元素
  - 在容器中查找元素
  - 获取容器首/尾元素的迭代器
  - ......
- 这些成员函数往往具有通用性（大部分容器类都包含它们）

### 注意事项
- 如果容器的元素类型是一个类，则针对该类可能需要：
  - 自定义拷贝构造函数和赋值操作符重载函数
    - 对容器进行的一些操作中可能会创建新的元素（对象的拷贝构造）或进行元素间的赋值（对象赋值）
  - 重载小于操作符（<）
    - 对容器进行的一些操作中可能要进行元素比较运算

## 示例
### 利用容器vector来实现序列元素的存储与统计
```cpp
#include <iostream>
#include <vector>
using namespace std;
int main()
{ vector<int> v; //创建一个vector类容器对象v
   int x;
   cin >> x;
   while (x > 0) //循环往容器v中增加正的int型元素
   {  v.push_back(x); //往容器v的尾部增加一个元素
      cin >> x;
   }
   int sum=0,max,min;
   max = min = v[0]; //vector重载了操作符"[]"
   for (int i=0; i<v.size(); i++) //遍历容器元素
   { sum += v[i]; 
      if (v[i] < min) min = v[i];
      else if (v[i] > max) max = v[i];
   }
   cout << "sum=" << sum << ",max=" 
          << max << ",min="  << min << endl;
   return 0;
}
```
或使用C++11范围for循环：
```cpp
int sum=0,max,min;
   max = min = v[0];
   for (int n: v) //遍历容器元素（C++11）
   { sum += n;
      if (n < min) min = n;
      else if (n > max) max = n;
   }
   cout << "sum=" << sum << ",max=" 
          << max << ",min="  << min << endl;
   return 0;
}
```

### 利用容器map来实现电话号码簿功能
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main()
{ map<string,int> phone_book; //创建一个map类容器，用于存储电话号码簿
  //创建电话簿
  pair<string,int> item; //pair<string,int>是phone_book的元素类型
  item.first = "wang"; item.second = 12345678;
  phone_book.insert(item); //在容器phone_book中增加一个元素
  item.first = "li"; item.second = 87654321;
  phone_book.insert(item); //在容器phone_book中增加一个元素
  item.first = "zhang"; item.second = 56781234;
  phone_book.insert(item); //在容器phone_book中增加一个元素
  ......
  //输出电话号码簿
  cout << "电话号码簿的信息如下：\n";
  for (pair<string, int> item: phone_book)
    cout << item.first << ": " << item.second << endl; 
                                         //输出元素的姓名和电话号码
  //查找某个人的电话号码
  string name;
  cout << "请输入要查询号码的姓名：";
  cin >> name;
  map<string,int>::const_iterator it; //创建一个不能修改所指向的元素的迭代器
  it = phone_book.find(name); //查找关键字为name的容器元素
  if (it == phone_book.end()) //判断是否找到
     cout << name << ": not found\n"; //未找到
  else
     cout << it->first << ": " << it->second << endl; //找到
  
  return 0;
}
```
或使用map重载的"[]"操作符：
```cpp
//创建电话簿
phone_book["wang"] = 12345678; //通过[]操作和关键字往容器中加入元素
phone_book["li"] = 87654321;
phone_book["zhang"] = 56781234