## C++标准库-容器

C++提供了一系列容器，这些容器可以在实际使用中方便我们操作或存储一些数据，本章将详细介绍这些容器，本章内容可参考数据结构等方面书籍使用。

#### 序列式容器-vector

vector（向量类模版）类似于动态数组，可当作数组使用，使用需要包含vector头文件

> ```c++
> template < class T, class Alloc = allocator<T> > class vector; //基本用法
> ```

**vector对象定义**

>```c++
>vector <string> myvt;//字符串数组
>vector <int> nums; //数组
>vector <vector <int>> nnums; //二维数组
>```

**vector对象初始化**

>```c++
>myvt.push_back("1.beijing");
>myvt.push_back("2.shanghai");
>
>nums.push_back(1);
>nums.push_back(2);
>
>nnums.push_back(nums);
>```

**vector对象容量**

vector可以使用size(),capecity(),resize(),reserve()函数对容器容量进行操作

- size():size函数返回容器的元素数目
- capacity():capacity函数返回容器中能够容纳的元素数
- resize():resize函数可以改变容器的大小
- reserve():reserve函数可以预先设置容器的大小

> ```c++
> vector <string> myvt;
> myvt.reserve(4);//预先设置容器大小
> cout<<myvt.size()<<endl; //输出容器元素个数
> cout<<myvt.capacity()<<endl; //输出容器可容纳元素个数
> myvt.resize(2); //将元素个数设置为2个
> ```

**vector的基本函数**

- empty():判断数组是否为空
- clear():清空数组
- front():返回数组第一个元素
- back():返回数组最后一个元素
- push_back():向数组后面添加一个元素
- insert():可以实现在迭代器或指定位置插入元素
- pop_back():将数组最后一个元素删除并返回该元素
- erase():可删除由迭代器指定的元素或区间
- swap():交换两个数组的元素

**vector遍历**

遍历vector可以使用at()或迭代器两种方式，但必须在for或while循环中

> ```c++
> //迭代器
> void Iter_for(vector<ST> &vt){
> ST temp;
> vector::iterator iter;
> for(iter = vt.begin();iter!=vt.end();iter++){
>  temp = *iter;
>  cout<<temp.id<<temp.db<<endl;
> }
> }
> ```

> ```c++
> //迭代器
> void at_for(vector<ST> &vt){
> ST temp;
> int i = 0;
> int m = vt.size();
> for(i = 0;i<m;i++){
>  temp = vt.at(i);
>  cout<<temp.id<<temp.db<<endl;
> }
> }
> ```

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/vector/vector/)

#### 序列式容器-list

list列表是一个双向链表实现的容器，执行插入删除会非常迅速，需要包含list头文件。list有以下特点：

- 没有下标[]和at()运算
- 没有内存分配等操作，每个元素都有自己的内存
- 能实现所有链表操作

**list的初始化**

list链表可以用以下初始化方式

> ```c++
> list <A> listname;//创建空链表
> list <A> listname(size);//创建一个大小为size的链表
> list <A> listname(size,value);//创建一个大小为size，所有元素均为value的链表
> list <A> listname(elselist);//从其他链表拷贝出一个链表
> list <A> listname(first,last);//从迭代器first到last之间拷贝出一个链表
> ```

**list元素赋值**

list有四个常用函数用以赋值，分别可以在头尾输入输出

- push_back():在链表尾部加入元素
- push_front():在链表头部加入元素
- pop_back():将链表尾部元素去除
- pop_front():将链表头部元素去除

**list的迭代器**

- begin():指向list的第一个元素
- rbegin():反向迭代器，指向最后一个元素，++之后会向前移动
- end():指向最后一个元素的下一个元素
- rend():反向迭代器，指向第一个元素的前一个元素，--之后会向后移动
- front():list第一个元素的引用，可以直接使用，以上四个需要解(*)才能使用
- back():list最后一个元素的引用，可以直接使用

**list容量**

- size():list中元素的个数
- max_size():能容纳的最多的元素的个数，通常很大
- resize():重新调整list大小

**list基本函数**

- empty():判断list是否为空

- assign():重置list

- swap():交换两个list内容

- insert():插入元素到list中

- erase():删除元素或元素区间

- clear():清空list

- merge():合并两个list，并升序排列

- sort():对list进行升序排列（默认），可选降序

- remove():删除指定内容的所有元素

- remove_if():删除与指定内容不符的所有元素

- splice(): 插入函数。**注：将元素插入到原list后，被插的元素就不存在于x中了，如下第一个函数执行后，x为空**

> ```c++
> void splice(iterator it,list &x);//将列表x插入到迭代器it之前
> void splice(iterator it,list &x,iterator first);//将列表x的第first个元素插入到迭代器it之前
> void splice(iterator it,list &x,iterator first,iterator last);//将列表x的first和last区间的元素插入到迭代器it之前
> ```

- unique():删除重复元素，相同元素仅剩下一个

- reverse():将list元素逆置

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/list/list/)

#### 序列式容器-deque

deque为“double-ended queue”，双端队列，和vector类似，但是可以在两端非常高校的添加或删除元素，特点：

- deque可以在两端插入和删除
- deque不支持内存分配操作
- deque区块不使用时会被释放
- 和vector一样，在中间插入或删除元素时，效率较低
- deque迭代器为智能型指针
- deque支持随机访问

**deque的定义**

deque可以使用以下方法定义，使用时需要包含deque头文件

> ```c++
> deque <typename T> name; //创建一个空双端队列
> deque <typename T> name(size); //创建一个size大小的双端队列
> deque <typename T> name(size,value); //创建一个size大小且每个元素均为value的双端队列
> deque <typename T> name(elsedeque); //从elsedeque拷贝一个新的双端队列
> deque <typename T> name(first,last); //拷贝出一个迭代器first和last之间的双端队列
> ```

**deque容量操作**

deque可以对其容量进行操作

- size():返回deque容器的元素个数
- max_size():返回deque可以容纳的最多的元素个数
- resize():把deque重新调整为n个大小

**deque元素赋值**

- push_front():在头部插入元素
- push_back():在尾部插入元素
- pop_front():将头部元素删除
- pop_back():将尾部元素删除

**deque迭代器**

- begin():指向deque队列第一个元素
- rbegin():反向迭代器，指向deque队列最后一个元素，++会向前移动
- end():指向deque队列最后一个元素的下一个元素
- rend():反向迭代器，指向deque队列理论的第一个元素前一个元素
- front():返回第一个元素引用
- back():返回最后一个元素引用

**deque相关函数**

- empty():判断deque队列是否为空
- at():deque支持at函数访问元素
- assign():该函数可以重置一段或一个元素
- swap():用以交换队列
- insert():该函数可以往队列中插入函数
- erase():该函数可以删除队列中一个或一段元素
- clear():用以清空队列
- find():该函数可以查找元素在队列中的位置
- []:和at函数一样，用以随机访问队列元素

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/deque/deque/)

#### 关联式容器-set/multiset

set/multiset采用的是二叉树数据结构，set容器中所有元素必须具有唯一值，不能包含重复的元素，容器中元素即是键值又是数据；multiset容器中元素不必具有唯一值。使用set/multiset需要包含<set>头文件，容器中元素会自动排序，支持随机访问。

**set/multiset初始化**

> ```c++
> set<int> first; //生成空集合
> int myints[]= {10,20,30,40,50}; 
> set<int> second (myints,myints+5);//从列表拷贝到集合
> set<int> third (second); //从其他集合拷贝出一个集合
> set<int> fourth (second.begin(), second.end()); //从另一个区间拷贝出一个集合
> set<int,classcomp> fifth; //带比较函数的集合（通常决定排序顺序）
> set<int,bool(*)(int,int)> sixth (fn_pt); //创建一个带内存控制集合
> ```

multiset初始化方式同set

**set/multiset容量操作**

同以上其他容器，集合同样有empty(),size(),max_size()函数，除此之外还提供了统计和查找函数

- empty():判断集合是否为空

- size():返回集合元素个数

- max_size():返回集合容纳的最多元素个数，通常很大
- count(key):统计关键词key的个数
- find(key):返回集合中键值为key的第一个元素位置，返回迭代器类型
- Low_bound(key):返回集合中键值等于key的最后一个元素，返回迭代器类型
- Upper_bound(key):返回集合中键值大于等于key的第一个元素，返回迭代器类型
- Equal_range(key):返回一个迭代器对，或称迭代器区间，该区间包含集合中键值等于key的所有元素

**set/multiset基本用法**

- =：同其他容器一样，集合支持 operator =（赋值）运算，左右两端需要同类型
- swap():与另一个集合交换内容，同样左右两端需要同类型
- insert():向集合插入（若干）元素，返回插入的位置

> ```c++
> multiset<int>::iterator it;
> it=mymultiset.insert(25); //插入一个元素
> it=mymultiset.insert (it,27); //在指定位置插入一个元素
> int myints[]= {5,10,15};
> mymultiset.insert (myints,myints+3); //插入一个列表
> ```

- erase():删除若干元素

> ```c++
> erase(it); //删除迭代器it位置的元素
> erase(first,last); //删除迭代器first和last区间的元素
> erase(key); //删除键值为key的元素
> ```

- clear():清空集合

**set/multiset迭代器**

- begin():指向集合第一个元素
- rbegin():反向迭代器，指向集合最后一个元素，++会向前移动
- end():指向集合最后一个元素的下一个元素
- rend():反向迭代器，指向集合理论的第一个元素前一个元素

查看源码或更多参数细节参考[set](http://www.cplusplus.com/reference/set/set/)或[multiset](http://www.cplusplus.com/reference/set/multiset/)

#### 关联式容器-map/multimap

map/multimap是键-值对的集合，可使用键值获取相应的数值，他俩不同的是map不允许键值重复，而multimap则允许键值重复，两者使用时需要包含<map>头文件，以下不加以说明map即代表了map和multimap

**map初始化**

> ```c++
> map<char,int> first;//创建一个空map集合
> first['a']=10;
> first['b']=30;
> first['c']=50;
> first['d']=70;
> map<char,int> second (first.begin(),first.end());//从一个迭代器区间拷贝一个集合
> map<char,int> third (second);//拷贝另一个集合
> bool(*fn_pt)(char,char) = fncomp;
> map<char,int,bool(*)(char,char)> fifth (fn_pt);//以fn_pt为排序规则的集合
> ```
>
> multimap用法与map相同

**map容量操作**

- empty():判断集合是否为空
- size():返回集合元素个数

- max_size():返回集合容纳的最多元素个数，通常很大

**map的遍历**

map的遍历中常用几个迭代器

- begin():指向集合第一个元素
- rbegin():反向迭代器，指向集合最后一个元素，++会向前移动
- end():指向集合最后一个元素的下一个元素
- rend():反向迭代器，指向集合理论的第一个元素前一个元素

遍历操作

```c++
map<typekey,typevalue>::iterator it;
for(it = map.begin();it!=map.end();it++){
  cout<<(*it).first<<":"<<(*it).second<<endl;
}
```

**map常用函数**

- insert():插入函数

> ```c++
> mymap.insert ( std::pair<char,int>('a',100) );//直接插入
> pair<std::map<char,int>::iterator,bool> ret;//返回键值对，第一个参数是插入的迭代器位置，第二个元素是是否进行了插入
> ret = mymap.insert ( std::pair<char,int>('z',500) );//直接插入并返回
> mymap.insert (it, std::pair<char,int>('b',300));//在固定位置（迭代器）插入，返回值为插入的迭代器位置，若未插入则返回end()
> mymap.insert(anothermap.begin(),anothermap.end());//将一个集合插入，无返回值
> ```

- erase():

> ```c++
> mymap.erase (it);//删除指定位置的元素
> mymap.erase ('c');//删除指定key值的元素
> mymap.erase ( mymap.begin(), mymap.end()); //删除区间元素
> ```

- clear():清空函数，清空集合数据

- swap():交换两个集合的所有元素

- count(key):计算key键值出现的次数

- find(key):返回键值为key的首次出现位置迭代器

- upper_bound(key):返回键值大于等于key的第一个元素迭代器

- lower_bound(key):返回键值等于key的最后一个元素迭代器

- equal_range(key):返回键值等于key的迭代器区间

 **map作为关联式数组**

通常关联式容器无法进行元素的直接存取，必须依靠迭代器。但是map可以使用下标操作，例

> ```c++
> map["a"] = "apple";
> ```

但这通常是不推荐的，因为在直接存取过程中，如果键值不存在，那么map会自动创建该键值对，并将value设置为value默认元素。

查看源码或更多参数细节参考[map](http://www.cplusplus.com/reference/map/map/)或[multimap](http://www.cplusplus.com/reference/map/multimap/)

#### 特殊容器-bitset

bitset是使用一个数组来管理各个位，并且每一位仅能为1或0，使用时需要包含头<bitset>文件

**bitset初始化**

> ```c++
> string str("10011011");
> bitset <16> b1;//创建一个16位bitset，所有位均为0
> bitset <16> b2(25);//创建一个16位bitset，各个位的值为25的二进制值（0000000000011001）
> bitset <16> b3(str);//创建一个16位bitset，各个位的值与字符串一样
> bitset <16> b3(str,2,2);//创建一个16位bitset，各个位的值与字符串第2位开始共2位的值一样（右对齐）
> ```

**bitset常用函数**

- count():计算bitset中1的个数
- size():返回bitset大小
- test(n):判断第n位是否为1
- any():判断是否有至少一位是1
- none():判断是否全是0
- all():判断是否全是1
- set():设置所有位均为1
- reset():设置所有位均为0
- flip():翻转所有位
- to_string():将bitset以字符串形式表示

**bitset特殊操作**

- ^=:异或并赋值
- |=:或并赋值
- &=:与并赋值
- <<=:左移并赋值
- \>>=:右移并赋值
- 以上操作不加等号时候就返回结果的拷贝

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/bitset/bitset/)

#### 特殊容器-stack

stack模版，又称栈，基本特点为后进先出（LIFO），可以使用任何序列式容器作为其底层，但deque目前表现效果较好，故默认使用，使用时需包含<stack>头文件

**stack的初始化**

> ```c++
> deque<int> mydeque (3,100);
> vector<int> myvector (2,200);  
> stack<int> first; //创建一个空栈
> stack<int> second (mydeque); //创建一个拷贝deque的栈
> stack<int,vector<int> > third;//创建一个基于vector的空栈
> stack<int,vector<int> > fourth (myvector);//创建一个基于vector的拷贝vector的栈
> ```

**stack常用方法**

- empty():判断栈是否为空
- size():返回栈的大小
- top():返回栈顶的引用，可以对栈顶元素提取或赋值（不是退栈）
- push():将元素压入栈中（无返回值）
- pop():将栈顶元素移除（无返回值）

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/stack/stack/)

#### 特殊容器-queue

queue模版，又称队列，基本特点为先入先出（FIFO），元素只能从容器一段压入，从另一段移除，常用于数据缓冲区，使用时需包含<queue>头文件

**queue初始化**

> ```c++
> deque<int> mydeck (3,100);
> list<int> mylist (2,200);
> queue<int> first;//初始化一个空队列
> queue<int> second (mydeck);//创建一个deuqe双端队列的拷贝
> queue<int,std::list<int> > third;//创建一个基于list的队列
> queue<int,std::list<int> > fourth (mylist);//创建一个基于list的list拷贝队列
> ```

**queue常用方法**

- empty():判断队列是否为空
- size():返回队列元素个数
- front():返回队列第一个元素的引用
- back():返回队列最后一个元素的引用
- push():将元素压入队列尾部（无返回值）
- pop():从头部将元素移除队列（无返回值）

查看源码或更多参数细节参考[这里](http://www.cplusplus.com/reference/queue/queue/)

*待更新：比较运算，单向链表，优先队列*

