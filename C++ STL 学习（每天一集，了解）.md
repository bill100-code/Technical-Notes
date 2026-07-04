#  C++ STL 学习（每天一集，了解）



## 10.1 基本概念

stl是standard template library(标准模板库)

stl广义上分为三种：algorithm(算法)，container（容器），iterator（迭代器）

stl的重要特征：1.高性能 2. 高移植性 3.跨平台

## 10.2容器

### 10.2.1 容器的分类

1.序列式容器（Sequence Containers)

vector,list,queue,stack,deque等线性表

2.关联式容器（Associated Containers）

元素的顺序取决于特定的排序准则，与插入顺序无关

如set ,multiset ,map,multimap

![image-20260606195548975](C:\Users\27004\AppData\Roaming\Typora\typora-user-images\image-20260606195548975.png)

![image-20260606195639279](C:\Users\27004\AppData\Roaming\Typora\typora-user-images\image-20260606195639279.png)

### 10.2.2vector

使用模板实现类

默认构造形式 vector <T> name     //T可以是数据类型，也可以是类

#### 一、带参数的构造方式 

1.vector(begin,end) //左闭右开，把这一段区间拷贝过去。begin 和end是指针

2.vector(n,elem)     //将n个elem拷贝给本身

3.vector(const vector &vec)

#### 二、vector 的赋值

vector assign(begin,end); //将左闭右开区间的数据拷贝复制到本身

vector assign(n,elem)

vector assign(const vector &vec)

begin(),end()均为迭代器,begin指向的是vector的第一个元素，而end指向的是vector最后一个元素的下一个元素

vector.swap(&vec) 互换两个vector的元素

#### 三、vector的大小

vector.size() //元素个数

vector.empty()  //容器是否为空 (bool类型)

vector.resize(num,elem)     //重新制定容器的长度为Num,若容器变长，则以elem值填充新位置，若长度变短，则末尾超出容器长度的元素被删除

#### 四、访问vector中的元素

如果直接使用下标法访问元素很有可能下标越界

为了防止这种情况，给出了at函数

vector.at(idx) 如果idx越界，会弹出out of range异常。说实在的，这个stack我也可以写，只是效率没有它高，底层实现很简单。也算是一种封装抽象吧，不让你从底层访问了。c语言果然和c++差别巨大，作用的区域明显不同

用索引的话，下标越界会输出滚木

#### 五、vector 的插入

vector.insert(pos,elem)  //在pos插入一个elem的拷贝，返回新数据的位置

vector.insert(pos,n,elem)   //在pos插入n个elem，无返回值

vector.insert(pos,begin,end)  //在pos位置插入左闭右开的区间的数据，无返回值

注：pos为指针！！！

vector.push_back(elem); //在末尾插入元素elem

vector.pop_back()  删除最后一个元素，并没有返回值，不能用作栈

#### Summary:

vector学习到此为止，难怪老师说把c学完，c++会简单很多。因为c语言是直接对内存进行操作的，抽象程度也不够高，我们要从底层写很多代码，而c++有很多功能强大的库函数，我们不再需要手动造数据结构，可以将注意力放在算法上，避免了重复造轮子。

### 10.2.3deque容器

#### 一、deque容器简介

deque 是 double_ended_queue的缩写，意味双队列

deque在接口上与vector非常相似，很多操作我认为可以相互替换。同样的deque也可以随机访问元素，支持at，[]方法。

deque的头部与尾部添加元素很快，但是中间很慢，很合理。

#include <deque>

#### 二、  deque容器的操作

其他操作与vector极其相似，这里有两个新函数

deque.push_front(elem ),deque.pop_front(),望文生义

vector容器中的元素连续，而deque未必连续

### 10.2.4 list容器

双向链表在实际的工作中使用的更多，因为可以反向遍历。

list不能使用at或者[]访问（显然），迭代器自增自减，不能增减常数。

常用函数：

list.push_back(elem)  尾插法

list,push_front(elem) 头插法

list.pop_front()

list.pop_back()同理，删除头节点和尾结点

list.front(),list.back()只是访问，并不会删除

同时我们可以通过这两个函数去修改头和尾部的值，list.front()=10;list.back() = 60.

list的迭代器是双向迭代器除了提供前向迭代器的全部操作之外，双向迭代器哦还提供了前置和后置的自减运算

list.begin()   list.end()  

list.rbegin(); list.rend(); 使用方式和begin和endwan完全一致，不再赘述

list的构造方式

1.list(n,elem)

2.list(begin,end)    //构造函数将[begin,end]拷贝进list，这里是左闭右开区间。！！//老师讲义有问题，太蠢了，我还以为是特殊设计呢！！！！

3.list(const &list)

list赋值和vector没什么区别

list的大小同样，size(),empty(),resize()

### 10.2.5 queue容器

​	queue容器的构造无法使用n,elem和迭代器方法，只能使用默认构造和构造函数构造法

queue.push();   queue.pop()

而且由于queue没有提供迭代器，只能利用front+pop的方式一遍遍历一遍弹出。遍历就会出队

queue.back()   返回最后一个元素

front可以作为左值，可以进行赋值和修改。

size函数返回队列大小，无resize函数

### 10.2.6  set和multiset

#### 基本介绍：

①set是一个集合容器，容器中的元素是唯一的，按照排序规则插入，不能指定插入位置。

②set采用红黑树变体的数据结构实现，红黑树属于平衡二叉树，	插入和删除效率更高。

③set不可以直接存取元素，不能使用.at(pos)访问与[]操作符

④set只允许一个值出现一次，而multiset允许一个值出现多次

⑤set和multiset不允许直接修改，只能够先删除后修改（很合理，要保持红黑树的形状）

#### 应用场景：

进行集合的运算，求两个集合的并集的大小之类

#### 构造方式：

默认构造： set <int> a; multi set <string> b;等等

#### 容器插入与迭代器：

set.insert(elem)；会根据元素的值自动插入数据

由于set不能使用下标法访问，只能使用迭代器，这里提供了4个迭代器

set.begin(); set.end(); 以及反向迭代器set.rbegin(); set.rend();

默认是升序排列，那么如何自己制定排列方式呢？

#### 拷贝构造与赋值（与之前无异）

#### 删除元素

①set.clear()清理所有元素

②set.erase(pos) 删除pos迭代器所对应的元素，并返回下一个元素的迭代器

③set.erase(beg,end)  删除左闭右开区间的元素

④set.erase(elem)  删除容器中值位elem的元素 	如果elem在set中返回true，否则返回false	

## 10.3迭代器

迭代器的基本概念

为什么需要迭代器？STL提供的每种容器的实现原理各不相同，如果没有迭代器我们需要记住每一种容器中对象的访问方式，很显然这样很麻烦

每一个容器都实现了一个迭代器用于对容器内对象的访问，虽然每个容器中的迭代器的实现方式不一样，但是对于用户来说操作方式是一致的。也就是说通过迭代器同意了对所有容器的的访问方式。例如：五路居你是哪个容器，访问当前元素的下一个元素我们可以通过迭代器的自增进行访问

迭代器是为了提高发编程效率而开发的。虽然每个容器中的迭代器的实现方式不一样，但是对于用户来说操作方法是一致的 ，也就是说通过迭代器同意了对所有容器的访问方式

begin和end函数，每种容器都定义了begin和end函数，指向第一个元素和最后一个元素下一个元素 

vector <int>::iterator it; 可以粗浅理解为指针，老师没有讲述具体的原理

迭代器失效：插入新元素，可能会重新分配空间，而迭代器还是指向原来的位置，可能已经释放，所以迭代器失效了

insert函数有返回值，返回有效的迭代器

删除迭代器：vector <int> ::iterator it;

vector <int> a; it = a.begin();

a.erase(it);  好喜欢打字机专注模式

删除之后迭代器失效。删除的方式也很合理，把需要删除的元素后面的元素往前移一位，但是这样搭配for循环容易出现漏网之鱼，所以删除之后迭代器不要往前移动！！！所以，增加元素是重新分配地址，而删除元素则是后继元素的整体移动，这就是容器内部进行的操作。erase的返回值it1就是it的位置。为了保护怎敢迭代器不失效，我们要将迭代器重新赋值



