# 1.容器

## 1.1 array

**array保存在栈中**

```c++
std::array<int, 4> arr= {1,2,3,4};

int len = 4;
std::array<int, len> arr = {1,2,3,4}; // 非法, 数组大小参数必须是常量表达式

const int len = 4;
std::array<int, len> arr = {1,2,3,4}; // 正确写法
```

## 1.2 vector

不同的编译器实现的扩容方式不一样，vector 在VS2015中以1.5倍扩容，GCC以2倍扩容。

**vector保存在堆中**

```cpp
vector<vector<int>> v;

v.size();		//返回行数

v[0].size();	//返回列数
```




## 1.3 list

list 是双向循环链表 记住！！！

List的插入、删除或者拼合操作不会造成原有迭代器的失效。

List不能用STL 中的sort函数进行排序，而是要用自身的sort函数。List仅支持随机访问迭代器，而List是双向迭代器。

## 1.4 forward_list

单向链表，标准库容器中唯一不提供size()方法的容器，当不需要双向迭代时，具备比list更高的空间利用率。
## 1.5 stack

基础示例:

```cpp
	stack<int> s;
	s.push(10);
	s.push(20);
	s.push(30);

	while (!s.empty())
	{
		//输出栈顶元素
		cout << "栈顶元素" << s.top() << endl;
		//弹出栈顶元素
		s.pop();
	}
	cout << "栈的大小为:" << s.size() << endl;
```
emplace ://emplace函数可以将一个元素加入栈中，与push的区别在于：emplace可以直接传入Node的构造函数的参数，并将构造的元素加入栈中
```cpp
#include <iostream>       
#include <stack>
using namespace std;

struct Node {
    int a,b;
    Node (int x, int y) {
        a = x; b = y;
    }
};
int main ()
{
    stack<Node> mystack;
    mystack.emplace(1,2);        
    //mystack.push(1,2);        //编译不通过，要达到上面的效果需要手动构造，例如mystack.push(Node(1,2));
    Node p = mystack.top();
    cout << p.a << " " << p.b << endl;
    
    stack<Node> my2;
    my2.swap(mystack);            //swap函数可以交换两个栈的元素
    cout << mystack.size() << " " << my2.size() << endl;
  return 0;
}
```
# 2.智能指针

对于编译器来说，智能指针实际上是一个栈对象，并非指针类型，在栈对象生命期即将结束时，智能指针通过析构函数释放有它管理的堆内存。所有智能指针都重载了“operator->”操作符，直接返回对象的引用，用以操作对象。访问智能指针原来的方法则使用“.”操作符。

访问智能指针包含的裸指针则可以用 get() 函数。由于智能指针是一个对象，所以if (my_smart_object)永远为真，要判断智能指针的裸指针是否为空，需要这样判断：if (my_smart_object.get())。

## 2.1	auto_ptr

**采用管理权转移，拷贝时会导致对象悬空，设计有缺陷，不建议使用**

```c++
//auto_ptr<int> p;//初始化为NULL
//  错误写法  auto_ptr<int> p = new int(123);
auto_ptr<int> p(new int(123));
cout << *p;
```

```c++
    auto_ptr<int> p(new int(123));
    auto_ptr<int> p1(p);//将p的使用权转给p1,p1已经指向nullptr无法正常访问
//    cout << *p << endl;
    cout << *p1 << endl;
```

## 2.2	unique_ptr

**特点：防拷贝，简单粗暴，建议使用**

**缺点：不能拷贝，可以转让**

```c++
unique_ptr<int> p(new int(123));
*p = 7;
cout << *p<< endl;
```

```c++
unique_ptr<string> upt1=std::move(upt);  //控制权限转移
if(upt.get()!=nullptr)					//判空操作更安全
{
	//do something
}
```

```c++
unique_ptr<int> p1;          //创建空的智能指针
p1.reset(new int(3));    //绑定动态对象
unique_ptr<int> p2(new int(4)) ;        //创建时绑定动态对象
cout << *p1 <<endl;
cout << *p2 <<endl;
//所有权发生变化
int *p = p1.release();      //释放所有权

unique_ptr<string> p_s1(new string("abc"));
//    unique_ptr<string> p_s2 = std::move(p_s1);
    cout << *p_s1 <<endl;
//    cout << p_s2 <<endl;
```
## 2.3	shared_ptr

 shared_ptr的原理：通过引用计数的方式来实现多个shared_ptr对象之间共享资源。

1. shared_ptr在其内部，给每个资源都维护了一份计数，用来记录该份资源被几个对象共享。
2. 在对象被销毁时(也就是析构函数调用)，就说明自己不使用该资源了，对象的引用计数减一。
3. 如果引用计数是0，就说明自己是最后一个使用该资源的对象，必须释放该资源。
4. 如果不是0，就说明除了自己还有其他对象在使用该份资源，不能释放该资源，否则其他对象就成野指针了。

```c++
shared_ptr<int> p1(new int(123));
shared_ptr<int> p2(p1);
shared_ptr<int> p3(p2);
cout << *p1 << " " << p1.use_count() << endl;
cout << *p2 << " " << p2.use_count() << endl;
```
循环引用

循坏引用分析：

1. node1和node2两个智能指针对象指向两个结点，引用计数变成1，我们不需要手动delete。
2. node1和_next指向node2，node2的_prev还指向下一个结点。但是_prev还指向上一个节点。
3. node1和node2析构，引用计数减一，但是_next还指向下一个节点。但是_prev还指向上一个节点。
4. 也就是说_next析构了，node2就释放了。
5. 也就是说_prev析构了，node1就释放了。
6. 但是_next属于node成员，node1释放了，_next才会析构，而node1由_prev管理，_prev属于node2成员，所以这就叫循环引用，谁也不会释放。

```c++
#include<memory>
#include <iostream>

using namespace std;

struct ListNode {
    int _data;
    shared_ptr<ListNode> _prev;
    shared_ptr<ListNode> _next;

    ~ListNode() {
        cout << "~ListNode()" << endl;
    }

};

int main() {
    shared_ptr<ListNode> node1(new ListNode);
    shared_ptr<ListNode> node2(new ListNode);
    cout << node1.use_count() << endl;
    cout << node2.use_count() << endl;

    node1->_next = node2;
    node2->_prev = node1;
    cout << node1.use_count() << endl;
    cout << node2.use_count() << endl;
    return 0;
}
```

**解决方案：在引用计数的场景下，把节点中的_prev和_next改成weak_ptr就可以了**

**原理：node1->_next = node2;和node2->_prev = node1;时weak_ptr的_next和_prev不会增加 node1和node2的引用计数。** 

## 2.4	weak_ptr

weak_ptr 被设计为与 shared_ptr 共同工作，可以从一个 shared_ptr 或者另一个 weak_ptr 对象构造而来。weak_ptr 是为了配合 shared_ptr 而引入的一种智能指针，它更像是 shared_ptr 的一个助手而不是智能指针，因为它不具有普通指针的行为，没有重载 operator* 和 operator-> ，因此取名为 weak，表明其是功能较弱的智能指针。它的最大作用在于协助 shared_ptr 工作，可获得资源的观测权，像旁观者那样观测资源的使用情况。观察者意味着 weak_ptr 只对 shared_ptr 进行引用，而不改变其引用计数，当被观察的 shared_ptr 失效后，相应的 weak_ptr 也相应失效。

```c++
#include<memory>
#include <iostream>

using namespace std;

struct ListNode {
    int _data;
    weak_ptr<ListNode> _prev;
    weak_ptr<ListNode> _next;

~ListNode() {
    cout << "~ListNode()" << endl;
}

};

int main() {
    shared_ptr<ListNode> node1(new ListNode);
    shared_ptr<ListNode> node2(new ListNode);
    cout << node1.use_count() << endl;
    cout << node2.use_count() << endl;

    node1->_next = node2;
    node2->_prev = node1;
    cout << node1.use_count() << endl;
    cout << node2.use_count() << endl;
    return 0;
}
```

下面给出几个使用指南。
（1）如果程序要使用多个指向同一个对象的指针，应选择shared_ptr。这样的情况包括：
（a）的元素和最小的元素；
（b）两个对象都包含指向第三个对象的指针；
（c）STL容器包含指针。很多STL算法都支持复制和赋值操作，这些操作可用于shared_ptr，但不能用于unique_ptr（编译器发出warning）和auto_ptr（行为不确定）。如果你的编译器没有提供shared_ptr，可使用Boost库提供的shared_ptr

# 3.类型推导

## 3.1 auto

编程时候常常需要把表达式的值付给变量,需要在声明变量的时候清楚的知道变量是什么类型。然而做到这一点并非那么容易(特别是模板中)，有时候根本做不到。为了解决这个问题，C++11新标准就引入了auto类型说明符，用它就能让编译器替我们去分析表达式所属的类型。和原来那些只对应某种特定的类型说明符(例如 int)不同。auto 让编译器通过初始值来进行类型推演。从而获得定义变量的类型，所以说 auto 定义的变量必须有初始值。

```c++
int i = 3;
auto a = i,&b = i,*c = &i;//正确: a初始化为i的副本,b初始化为i的引用,c为i的指针.
auto sz = 0, pi = 3.14;//错误,两个变量的类型不一样。
```

## 3.2 decltype

`decltype` 关键字是为了解决 auto 关键字只能对变量进行类型推导的缺陷而出现的。

```c++
auto i = 1;
decltype(i) i2 = i;
cout  << i << "\t";
cout << i2;
```
# 4.强制类型转换
C++中四种类型转换是：static_cast, dynamic_cast, const_cast, reinterpret_cast

## 4.1 const_cast

**const_cast<类型说明符> (变量或表达式)**

 用于将const变量转换为非const类型

const_cast用于强制去掉const这种不能被修改的常数特性，但需要特别注意的是const_cast不是用于去除变量的常量性，而是去除指向常数对象的指针或引用的常量性，其去除常量性的对象必须为指针或引用。

## 4.2  static_cast

static_cast<类型说明符> (变量或表达式)

用于各种隐私转换，比如非const转const， void*转指针等， static_cast 能用于多态向上转化，如果向下转能成功但是不安全，结果未知。
## 4.3 dynamic_cast

dynamic_cast<类型说明符> (变量或表达式)

用于动态类型转换，只能用于含有虚函数的类，用于类层次间的向上和向下转化。只能转指针或引用。向上转换：指的是子类向基类转换。 向下转换：指的是基类向子类转换。  他通过判断在执行到该语句的时候变量的运行时类型和要转换的类型是否相同来判断是否能够向下转换。    
## 4.4 reinterpret_cast

reinterpret_cast<类型说明符> (变量或表达式)

几乎什么都可以转，比如将int转指针，可能会出问题，尽量少用。

改变指针或引用的类型、将指针或引用转换为一个足够长度的整形、将整型转换为指针或引用类型。

## 4.5 为什么不用C的强制转换？
C的强制转换表面上看起来功能强大什么都能转换，但转化不够明确，不能进行错误检查，容易出错。

# 委托构造函数

```c++
class Base {
public:
    int value1;
    int value2;
    Base() {
        value1 = 1;
    }
    Base(int value) : Base() {  // 委托 Base() 构造函数
        value2 = 2;
    }
};
```



# 继承构造

在继承体系中，如果派生类想要使用基类的构造函数，需要在构造函数中显式声明。 
 假若基类拥有为数众多的不同版本的构造函数，这样，在派生类中得写很多对应的“透传”构造函数。如下：

```c++
struct A
{
  A(int i) {}
  A(double d,int i){}
  A(float f,int i,const char* c){}
  //...等等系列的构造函数版本
}；
struct B:A
{
  B(int i):A(i){}
  B(double d,int i):A(d,i){}
  B(folat f,int i,const char* c):A(f,i,e){}
  //......等等好多个和基类构造函数对应的构造函数
}；
```

C++11的继承构造：

```c++
struct A
{
  A(int i) {}
  A(double d,int i){}
  A(float f,int i,const char* c){}
  //...等等系列的构造函数版本
}；
struct B:A
{
  using A::A;
  //关于基类各构造函数的继承一句话搞定
  //......
}；
```

# lambda

```
int a = 0;
auto f = [=] { return a; };

a+=1;

cout << f() << endl;       //输出0

int a = 0;
auto f = [&a] { return a; };

a+=1;

cout << f() <<endl;       //输出1
```

```c++
int singleNumber(vector<int>& nums) {
    if(nums.empty()) return 0;
    int len = nums.size();
    int temp = 0;
    for(int i = 0 ; i < len ; i++){
        temp ^=nums[i];
    }
    return temp;
}
```