每天积累一点，加油！

@toc

## 1. C++指针free是如何知道内存块大小的？
```cplusplus
int *p = (int *) malloc(10*sizeof(int));
p++;
free(p);  // 报异常错误
```
malloc分配的区块是带有metadata的，里面记录了区块的大小等必要的信息。malloc返回的指针经过free调用的时候会先找到metadata再进行释放，也就是说如果你喂给free一个别的指针的话那肯定是会报错的。

**特别注意**
1. 不能用delete和free释放非动态变量，报异常错误；
2. 用delete和free撤销动态数组时，其中的指针变量必须指向数组的第一个元素。

## 2. C++中static-cast和reinterpret-cast差别
### static_cast作用
1. 类层次中基类和子类的指针或引用类型转换：<上行转换：子类到基类>安全，<下行转换：基类到子类>无运行时动态类型检查，不安全；
2. 用于基本类型之间的转换，如char2int，安全靠程序猿自己把握；
3. void指针与其它类型指针之间的转换；
**warning**： static-cast保留指针const, volatile, 等属性
### reinterpret-cast作用
KEY：通常为操作数的位模式提供较低层的重新解释；
```c
int i;
char* p = "This is an example";
i = reinterpret_cast<int>(p);
```
### 其他
1. dynamic-cast：“安全地向下转换”，即下行转换前会动态检查；
2. const-cast: 强制消除对象的const属性；

## 3. 智能指针shared_ptr循环引用问题
### 循环引用问题
智能指针shared_ptr采用引用计数的方式来记录指针，只有引用计数为1时才执行delete销毁对象。在双向链表使用时会出现循环引用问题，如例子所示：
```c
#include<iostream>
#include<memory>
using namespace std;
struct Node
{
    int _value;
    shared_ptr<Node> _next; // modify:weak_ptr<Node>
    shared_ptr<Node> _prev; // modify:weak_ptr<Node>
};
int main()
{
    shared_ptr<Node> sp1(new Node);
    shared_ptr<Node> sp2(new Node);
    sp1->_next = sp2;
    sp2->_prev = sp1;
    cout<<sp1.use_count()<<endl; //引用计数-->2
    cout << sp2.use_count() << endl;//引用计数-->2
    system("pause");
    return 0;
} //如何真正销毁sp1指向的Node？先销毁sp2---sp1---sp2------
```
### weak_ptr配合解决问题
通过一个shared-ptr或weak-ptr对象构造，析构和构造不会引起引用指针的增加和减少。

## 4. 对象生存周期
在C++中存在四大类对象，分别为：
1. **全局对象**：程序开始就构造，比main更早，程序结束main后析构；
2. **局部对象**：对象诞生即构造，程序流程离开声明周期时，析构；
3. **静态对象**：对象诞生即构造，程序结束main后析构，比全局对象早析构；
4. **new产生的对象**：对象诞生即构造，delete即析构；

### 代码演示
```c
GlobalClass gcls;  // 全局对象：其它文件通过关键字extern调用；
static Demo1Class dcls1; // 静态全局对象：仅限本文件使用；
// 全局对象：Global中存储；
// 局部静态对象： Local Static中存储；

int main(){
  Demo2Class dcls2； // 局部对象：Stack栈中存储；
  Demo3Class* dcls3 = new Demo3Class(); // new对象：Heap堆中存储；
  delete dcls3;
  return 0;
}
```

## 5. delete基类指针
### 问题描述：
类继承中，通过基类指针delete释放，会不会有内存泄漏？

### 分析：
根据**C++的继承规则和继承的实现原理机制**如果你不把基类的析构函数声明并定义为virtual,那么子类在释放的时候,没法做尾场清理的。
在类继承机制中,构造函数和析构函数具有一种特别机制叫 **“层链式调用通知”**：在构造一个有类继承机制的类,比如上面的类B,那么会先调用A类的构造,A构造完成之后在调用B类的构造函数,达到"由里向外"通知调用的效果.那么释放一个有类继承机制的类,那么会调用B类的析构函数, 再调用A类的析构函数,达到"由外向里"通知通知的效果,那么为了达到这个这种“层链式调用通知”的效果，C++标准规定:基类的析构函数必须声明为virtual, 如果你不声明,那么"层链式调用通知"这样的机制是没法构建起来.从而就导致了基类的析构函数被调用了,而派生类的析构函数没有调用这个问题发生。




## 参考
* <http://www.yolinux.com/TUTORIALS/GDB-Commands.html>