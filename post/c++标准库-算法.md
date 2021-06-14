## C++标准库-算法

以有限的步骤解决逻辑或数学问题叫做算法。算法分为非修改性算法，修改性算法，排序算法和删除性算法，下面分别进行介绍：

#### 非修改性算法-for_each()算法

for-each算法是对范围内元素均采用一种算法或进程，其函数原型如下：

```c++
template <class InputIterator, class Function>
   Function for_each (InputIterator first, InputIterator last, Function fn);
//first和last为元素区间的头尾，fn为调用的函数
```

**for-each使用**

```c++
void myfunction (int i) {  // function:
  std::cout << ' ' << i;
}

struct myclass {           // function object type:
  void operator() (int i) {std::cout << ' ' << i;}
} myobject;

for_each (myvector.begin(), myvector.end(), myfunction);//对于区间的所有元素调用函数
for_each (myvector.begin(), myvector.end(), myobject);//对于区间的元素调用仿函数类，仿函数类会自动调用类中operator()函数
```

#### 非修改性算法-元素计数算法

stl算法库提供了元素计数功能的算法，用以计算范围内符合条件的元素的个数

其原型为

```c++
template <class InputIterator, class T>
  typename iterator_traits<InputIterator>::difference_type
    count (InputIterator first, InputIterator last, const T& val);//count
template <class InputIterator, class UnaryPredicate>
  typename iterator_traits<InputIterator>::difference_type
    count_if (InputIterator first, InputIterator last, UnaryPredicate pred);//count_if
```

由函数原型我们可以看到count函数用以计算元素区间内值等于val的数目，count_if函数是计算区间内使用pred函数结果为true的元素个数

**元素计数函数使用**

```c++
int mycount = count (myints, myints+8, 10);//计算myints前八个元素中等于10的个数
bool IsOdd (int i) { return ((i%2)==1); }
int mycount = count_if (myvector.begin(), myvector.end(), IsOdd);//计算myvector中为偶数的个数
```

#### 非修改性算法-最值算法

stl算法库提供了最值算法，其函数原型如下

- 最小值算法min_element():

```c++
template <class ForwardIterator>
  ForwardIterator min_element (ForwardIterator first, ForwardIterator last);
template <class ForwardIterator, class Compare>
  ForwardIterator min_element (ForwardIterator first, ForwardIterator last,
                               Compare comp);
```

- 最大值算法max_elemrnt():

```c++
template <class ForwardIterator>
  ForwardIterator max_element (ForwardIterator first, ForwardIterator last);
template <class ForwardIterator, class Compare>
  ForwardIterator max_element (ForwardIterator first, ForwardIterator last,
                               Compare comp);
```

从原型可以看到可以找到first和last区间的最值或first和last之间的comp函数的最值，返回值为迭代器

**最值算法使用():**

```c++
bool myfn(int i, int j) { return i<j; }

struct myclass {
  bool operator() (int i,int j) { return i<j; }
} myobj;

min_element(myints,myints+7);//myints前七个数最小值
max_element(myints,myints+7);//myints前七个数最大值
min_element(myints,myints+7,myfn);//myints前七个数最小值
max_element(myints,myints+7,myfn);//myints前七个数最大值
min_element(myints,myints+7,myobj);//myints前七个数最小值
max_element(myints,myints+7,myobj);//myints前七个数最大值
```



#### 非修改性算法-搜索算法

**匹配搜索算法**

匹配搜索算法有两个函数，一个为find函数，该函数查找与value相同的元素，find_if函数寻找符合条件的函数

**函数原型**

```c++
template <class InputIterator, class T>
   InputIterator find (InputIterator first, InputIterator last, const T& val);//find
template <class InputIterator, class UnaryPredicate>
   InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred);//find_if
```

**函数的使用**

```c++
vector<int>::iterator it = find (myvector.begin(), myvector.end(), 30);//寻找myvector中等于30的元素，在数组中为下标，容器中为迭代器
bool IsOdd (int i) {
  return ((i%2)==1);
}
vector<int>::iterator it = find_if (myvector.begin(), myvector.end(), IsOdd);//寻找myvector中第一个偶数
```

**连续匹配搜索算法**

连续匹配搜索算法可以搜索元素区间内连续n个值等于value的位置

**函数原型**

```c++
template <class ForwardIterator, class Size, class T>
   ForwardIterator search_n (ForwardIterator first, ForwardIterator last,
                             Size count, const T& val);
```

**函数的使用**

```c++
search_n (myvector.begin(), myvector.end(), 2, 30);//寻找myvector中连续两个30位置
```

此函数还有一个高级使用方法，就是最后可以加一个条件函数，函数原型和用法如下

```c++
template <class ForwardIterator, class Size, class T, class BinaryPredicate>
   ForwardIterator search_n ( ForwardIterator first, ForwardIterator last,
                              Size count, const T& val, BinaryPredicate pred );//函数原型
bool mypredicate (int i, int j) {
  return (i==j);
}
search_n (myvector.begin(), myvector.end(), 2, 10, mypredicate);//判断myvector中连续两个值为10的位置（这里的判断使用的mypredicate函数）
```

**子区间搜索算法**

子区间搜索算法可以在一个区间中查找另一个子区间的位置

**函数原型**

```c++
template <class ForwardIterator1, class ForwardIterator2>
   ForwardIterator1 search (ForwardIterator1 first1, ForwardIterator1 last1,
                            ForwardIterator2 first2, ForwardIterator2 last2);
template <class ForwardIterator1, class ForwardIterator2, class BinaryPredicate>
   ForwardIterator1 search (ForwardIterator1 first1, ForwardIterator1 last1,
                            ForwardIterator2 first2, ForwardIterator2 last2,
                            BinaryPredicate pred);
```

同样，子区间搜索算法也可以使用条件函数

**函数的使用**

```c++
bool mypredicate (int i, int j) {
  return (i==j);
}
search (haystack.begin(), haystack.end(), needle1, needle1+4);//在haystack中寻找与needle1前四个元素相同的区间位置
search (haystack.begin(), haystack.end(), needle2, needle2+3, mypredicate);//在haystack中寻找和needle2前三个元素相同的元素位置（采用函数判断）
```

**最后子区间搜索算法**

和上面的函数相同的效果，只是上一个函数寻找第一个符合条件的位置，本函数是寻找最后一个符合条件的位置

**函数原型**

```c++
template <class ForwardIterator1, class ForwardIterator2>
   ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1,
                              ForwardIterator2 first2, ForwardIterator2 last2);
template <class ForwardIterator1, class ForwardIterator2, class BinaryPredicate>
   ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1,
                              ForwardIterator2 first2, ForwardIterator2 last2,
                              BinaryPredicate pred);
```

**函数的使用**

```c++
find_end (haystack.begin(), haystack.end(), needle1, needle1+3);//在haystack中寻找与needle1前三个元素相同的区间位置
bool myfunction (int i, int j) {
  return (i==j);
}
find_end (haystack.begin(), haystack.end(), needle2, needle2+3, myfunction);//在haystack中寻找和needle2前三个元素相同的元素位置（采用函数判断）
```

**搜索子区间元素算法**

此算法不同于以上区间算法，该算法实现的是<u>在区间内搜索子区间至少一个元素的位置，其并不强调完全吻合</u>，子区间任意一个元素在母区间拥有即可。

**函数原型**

```c++
template <class InputIterator, class ForwardIterator>
   InputIterator find_first_of (InputIterator first1, InputIterator last1,
                                   ForwardIterator first2, ForwardIterator last2);
template <class InputIterator, class ForwardIterator, class BinaryPredicate>
   InputIterator find_first_of (InputIterator first1, InputIterator last1,
                                   ForwardIterator first2, ForwardIterator last2,
                                   BinaryPredicate pred);
```

**函数的使用**

```c++
find_first_of (haystack.begin(), haystack.end(), needle, needle+3);//在haystack中寻找needle前三个元素任意一个位置
bool comp_case_insensitive (char c1, char c2) {
  return (std::tolower(c1)==std::tolower(c2));
}
find_first_of (haystack.begin(), haystack.end(),needle, needle+3, comp_case_insensitive);//寻找符合条件的元素位置
```

**连续相等元素算法**

该函数提供寻找第一个连续相等两个元素的位置，或连续两个元素符合条件的位置

**函数原型**

```c++
template <class ForwardIterator>
   ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last);
template <class ForwardIterator, class BinaryPredicate>
   ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last,
                                  BinaryPredicate pred);
```

由于是区间内操作，所以仅给区间参数即可，无需其他参数

**函数使用**

```c++
adjacent_find (myvector.begin(), myvector.end());//寻找myvector中连续两个相等元素的位置
bool myfunction (int i, int j) {
  return (i==j);
}
adjacent_find (yvector.begin(), myvector.end(), myfunction);//寻找myvector中符合条件的连续两个元素的位置
```

#### 非修改性算法-比较算法

stl算法提供了三种比较算法，分别是equal，mismatch，lexicographical_compare，这三种算法各有不同，下面一一介绍

- equal():比较两个容器是否相等，类型和元素值均要相等
- mismatch():返回两个容易第一个不相等的元素位置
- lexicographical_compare():按照子弹逐一比较，该函数也是最严格的，在元素相等时，长度长的比长度短的大

**函数的用法**

```c++
equal (myvector.begin(), myvector.end(), myints)//判断myvector和myints是否相等
mypair = mismatch (myvector.begin(), myvector.end(), myints);//寻找第一个不相等位置
lexicographical_compare(foo,foo+5,bar,bar+9);//严格比较foo和bar两个容器
```

#### 修改性算法-复制

stl 库中有两个复制函数，一个是从前向后复制函数（copy），另一个为从后向前复制函数（copy_backward）

**函数原型**

```c++
template <class InputIterator, class OutputIterator>
  OutputIterator copy (InputIterator first, InputIterator last, OutputIterator result);//copy
template <class BidirectionalIterator1, class BidirectionalIterator2>
  BidirectionalIterator2 copy_backward (BidirectionalIterator1 first,BidirectionalIterator1 last,BidirectionalIterator2 result);//copy_backward
```

**函数使用**

```c++
copy ( myints, myints+7, myvector.begin() );//从前向后把 myints 复制到 myvector 中
copy_backward ( myvector.begin(), myvector.begin()+5, myints.end() );//从后向前把 myvector 复制到 myints 中
```

#### 修改性算法-互换

在容器中已经使用了很多次交换算法swap，本节就来介绍一下交换算法，交换算法有两种，一种是全部交换，一种是部分交换

**函数原型**

```c++
template <class T> void swap (T& a, T& b);//迭代器等
template <class T, size_t N> void swap(T (&a)[N], T (&b)[N]);//数组等
template <class ForwardIterator1, class ForwardIterator2>
  ForwardIterator2 swap_ranges (ForwardIterator1 first1, ForwardIterator1 last1,
                                ForwardIterator2 first2);
```

**函数用法**

作为容器函数用法可以参考容器部分，本节单讲作为算法的应用。

```c++
swap(foo,bar);//foo和bar是两个相同的容器
swap_ranges(foo.begin()+1, foo.end()-1, bar.begin());//部分交换，将foo的掐头去尾剩下的部分与bar交换
```

#### 修改性算法-赋值

stl算法库提供了四个赋值函数，分别是fill,fill_n,generate,generate_n

- fill():给范围内所有元素赋相同的值
- fill_n():给前n个元素赋相同的值
- generate():给范围内元素执行相同的操作
- generate_n():给前n个元素执行相同的操作

**函数定义**

```c++
template <class ForwardIterator, class T>
  void fill (ForwardIterator first, ForwardIterator last, const T& val);//fill
template <class OutputIterator, class Size, class T>
  OutputIterator fill_n (OutputIterator first, Size n, const T& val);//fill_n
template <class ForwardIterator, class Generator>
  void generate (ForwardIterator first, ForwardIterator last, Generator gen);//generate
template <class OutputIterator, class Size, class Generator>
  OutputIterator generate_n (OutputIterator first, Size n, Generator gen);//generate_n
```

**函数的使用**

```c++
fill (myvector.begin(),myvector.begin()+4,5);//给myvector前四个元素（区间表示）赋5
fill_n (myvector.begin(),4,20);//给myvector前4个元素赋20
int RandomNumber () { return (std::rand()%100); }
generate (myvector.begin(), myvector.end(), RandomNumber);//给myvector每个元素一个随机值
generate_n (myarray, 9, RandomNumber);//给myarray前9个元素随机值
```

#### 修改性算法-替换

算法库提供一种将一个值替换为另一个值的函数replace，而在实际使用中可能会有更多要求比如数值在一个区间等情况，随机产生了另一个替换函数replace_if，使用条件函数作为参数

**函数定义**

```c++
template <class ForwardIterator, class T>
  void replace (ForwardIterator first, ForwardIterator last,
                const T& old_value, const T& new_value);//replace
template <class ForwardIterator, class UnaryPredicate, class T>
  void replace_if (ForwardIterator first, ForwardIterator last,
                   UnaryPredicate pred, const T& new_value );//replace_if
```

**函数使用**

```c++
replace (myvector.begin(), myvector.end(), 20, 99);//将所有值为20替换为99
bool IsOdd (int i) { return ((i%2)==1); }
replace_if (myvector.begin(), myvector.end(), IsOdd, 0);//将所有偶数值替换为0
```

#### 修改性算法-逆转

或者称逆置，可以将范围内元素逆转，第一个元素放到最后一个位置，最后一个元素放到第一个

**函数定义**

```c++
template <class BidirectionalIterator>
  void reverse (BidirectionalIterator first, BidirectionalIterator last);//reverse
template <class BidirectionalIterator, class OutputIterator>
  OutputIterator reverse_copy (BidirectionalIterator first,
                               BidirectionalIterator last, OutputIterator result);//reverse_copy
```

**函数使用**

```c++
reverse(myvector.begin(),myvector.end()); //逆置myvecter
reverse_copy (myints, myints+9, myvector.begin());//将myints前9个元素逆置结果保存到myvector
```

#### 修改性算法-分区

stl提供一种算法，输入参数提供一个条件函数，其算法可以将条件函数判断为真的的元素排在前面，将条件函数判断为假的排在后面，其返回值为分区中间的位置迭代器

**函数原型**

```c++
template <class ForwardIterator, class UnaryPredicate>
  ForwardIterator partition (ForwardIterator first,
                             ForwardIterator last, UnaryPredicate pred);
```

其中pred为条件函数

**函数使用**

```c++
bool IsOdd (int i) { return (i%2)==0; }
std::vector<int>::iterator bound;
  bound = std::partition (myvector.begin(), myvector.end(), IsOdd);
//排序前123456
//排序后246135
//bound指向排序后“1”的位置
```

#### 修改性算法-下一个排列

算法库提供一种字典上更大的排列方式，例如原序列为123，则下一个更大的排列为132，其仅返回更大的下一个序列，而不是最大的序列，当其能够有下一个序列时，或者说其不是字典上最大的序列时，该函数返回true并重新排列，当其没有下一个序列或者说字典上为最大值时，该函数返回false。

**函数原型**

```c++
template <class BidirectionalIterator>
  bool next_permutation (BidirectionalIterator first,
                         BidirectionalIterator last);
template <class BidirectionalIterator, class Compare>
  bool next_permutation (BidirectionalIterator first,
                         BidirectionalIterator last, Compare comp);//comp可以自定义排序方式
```

**函数使用**

```c++
next_permutation(myints,myints+3);//将myints重新排列为字典上更大的下一个序列
```

#### 修改性算法-随机排列

算法库提供一种随机排列算法，该算法可以将元素区间的元素重新排列，但是在实际测试中，其重新排列后的元素序列并不是随机排列，例：0123456789重新排列后就是6035784129，在不同编辑器中都是这个结果，可能我对底层运算原理没有搞太懂，其说是按指定规则打乱元素。

**函数原型**

```c++
template <class ForwardIterator, class UnaryPredicate>
  ForwardIterator partition (ForwardIterator first,
                             ForwardIterator last, UnaryPredicate pred);
```

**函数使用**

```c++
template <class RandomAccessIterator>
  void random_shuffle (RandomAccessIterator first, RandomAccessIterator last);
template <class RandomAccessIterator, class RandomNumberGenerator>
  void random_shuffle (RandomAccessIterator first, RandomAccessIterator last,
                       RandomNumberGenerator&& gen);
```

**函数使用**

```c++
random_shuffle ( myvector.begin(), myvector.end() );//使用指定规则打乱元素
int myrandom (int i) { return std::rand()%i;}
random_shuffle ( myvector.begin(), myvector.end(), myrandom);//使用自定函数打乱元素
```

#### 排序算法-全排序

stl算法库提供两种全排序方式，一种是sort，其并不保证相同元素的相对位置在排序后不变，另一种stable_sort则可以保证相同元素的相对位置在排序后不变，换句话说，sort是不稳定排序，stable_sort是稳定排序。<u>注：这两个sort函数仅支持带有随机访问（下标访问）的容器，如deque，vector等，不支持list等容器，不过，list等可以使用容器自带的sort函数进行排序，效果一样</u>

**函数原型**

```c++
template <class RandomAccessIterator>
  void sort (RandomAccessIterator first, RandomAccessIterator last);//sort
template <class RandomAccessIterator, class Compare>
  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);//带有条件函数sort
template <class RandomAccessIterator>
  void stable_sort ( RandomAccessIterator first, RandomAccessIterator last );//stable_sort
template <class RandomAccessIterator, class Compare>
  void stable_sort ( RandomAccessIterator first, RandomAccessIterator last,Compare comp );//带有条件函数的stable_sort
```

条件函数可以自定义排序方式，默认情况为operator < 从小到大排序

**函数使用**

```c++
sort (myvector.begin(), myvector.begin()+4); //不稳定排列myvector前四个元素
bool myfunction (int i,int j) { return (i<j); }
sort (myvector.begin()+4, myvector.end(), myfunction);//不稳定排列myvector

stable_sort (myvector.begin(), myvector.end());//稳定排列
bool compare_as_ints (double i,double j)
{
  return (int(i)<int(j));
}
stable_sort (myvector.begin(), myvector.end(), compare_as_ints);//只排整数部分（如果是小树不一样，则按原顺序）
```

#### 排序算法-部分排序

stl算法库提供了一种部分排序方法，并不是只对前n个元素或元素区间排序，其代表的意思是<u>对容器中的部分元素进行排序，用以获取最大的n个元素或最小的n个元素</u>，节省算法时间，取完部分结果后剩下的元素不一定呈现有序状态。通过函数原型进一步讲解

**函数原型**

```c++
template <class RandomAccessIterator>
  void partial_sort (RandomAccessIterator first, RandomAccessIterator middle, RandomAccessIterator last);//部分排序
template <class RandomAccessIterator, class Compare>
  void partial_sort (RandomAccessIterator first, RandomAccessIterator middle, RandomAccessIterator last, Compare comp);//根据条件部分排序
template <class InputIterator, class RandomAccessIterator>
  RandomAccessIterator partial_sort_copy (InputIterator first, InputIterator last, RandomAccessIterator result_first, RandomAccessIterator result_last);//拷贝后部分排序
template <class InputIterator, class RandomAccessIterator, class Compare>
  RandomAccessIterator partial_sort_copy (InputIterator first,InputIterator last, RandomAccessIterator result_first, RandomAccessIterator result_last, Compare comp);//根据条件拷贝后部分排序
```

由函数原型可知，其代表将first和last区间的元素排序后，从first到middle为有序状体，其余不一定为有序状态。

**函数使用**

```c++
partial_sort (myvector.begin(), myvector.begin()+5, myvector.end());//取最小的五个数
partial_sort_copy (myints, myints+9, myvector.begin(), myvector.end());//将myints前九个数排序结果拷贝到myvector中（具体排序几个数或取几个最小值以myints和myvector大小决定
```

#### 排序算法-分区

这个算法在编程中极其容易出现，比如快速排序就可能用到这个算法，其算法是首先指定一个元素，将所有小于该值的元素放到这个值的左面，将所有大于这个元素的值放到这个值右面，只能保证左小右大并不能保证在左区间或右区间内有序

**函数原型**

```c++
template <class RandomAccessIterator>
  void nth_element (RandomAccessIterator first, RandomAccessIterator nth,
                    RandomAccessIterator last);//nth为指定的中间元素
template <class RandomAccessIterator, class Compare>
  void nth_element (RandomAccessIterator first, RandomAccessIterator nth,
                    RandomAccessIterator last, Compare comp);//comp为自定义比较方法
```

**函数使用**

```c++
nth_element (myvector.begin(), myvector.begin()+5, myvector.end());//以原序列第五个元素为中点，比该元素小的放到左面，比他大的放到右面
```

#### 排序算法-堆相关操作

堆是程序中常用的一种数据结构，其第一个元素总是最大值或最小值，其结构类似于二叉树，堆的相关操作包括转化为堆、加入元素、删除元素、转化为普通序列

**函数原型**

```c++
template <class RandomAccessIterator>
  void make_heap (RandomAccessIterator first, RandomAccessIterator last);//转化为堆
template <class RandomAccessIterator, class Compare>
  void make_heap (RandomAccessIterator first, RandomAccessIterator last,  Compare comp );//自定义堆排序方式

template <class RandomAccessIterator>
  void push_heap (RandomAccessIterator first, RandomAccessIterator last);//将最后一个元素加入堆的排列
template <class RandomAccessIterator, class Compare>
  void push_heap (RandomAccessIterator first, RandomAccessIterator last, Compare comp);//自定义堆排序方式

template <class RandomAccessIterator>
  void pop_heap (RandomAccessIterator first, RandomAccessIterator last);//将第一个元素移出堆
template <class RandomAccessIterator, class Compare>
  void pop_heap (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
//自定义堆排序方式

template <class RandomAccessIterator>
  void sort_heap (RandomAccessIterator first, RandomAccessIterator last);//将堆排列成普通序列
template <class RandomAccessIterator, class Compare>
  void sort_heap (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
//自定义排序方式
```

<u>注：在pop或push中没有插入或删除元素，因为其并不是将元素直接去除序列，而是将第一个元素（最大值/最小值）放到序列最后一个位置，或者将最后一个元素插入到序列的对应位置</u>

**函数使用**

```c++
make_heap (v.begin(),v.end());//转化为堆
pop_heap (v.begin(),v.end()); v.pop_back();//将第一个元素从堆删除，用容器推出才将元素删除
v.push_back(99); push_heap (v.begin(),v.end());//先将元素放到序列最后位置，再将该元素插入到堆对应位置
sort_heap (v.begin(),v.end());//将堆转化为普通序列
```

#### 排序算法-合并等操作

算法库给我们提供了很多便利，该系列函数就让我们方便的操作两个容器合并，交集或差集，<u>注：原两个容器均需要有序</u>

**函数原型**

```c++
//合并两个序列到result
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator merge (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result);
template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator merge (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp);//用条件函数控制合并后的排序顺序
//无重复值合并两个序列
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_union (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result);
template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator set_union (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp);//用条件函数控制合并后的排序顺序
//取两个容器元素的交集
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_intersection (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result);
template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator set_intersection (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp);//用条件函数控制合并后的排序顺序
//取只在第一个容器有的第二个容器中没有的元素
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result);
template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator set_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp);//用条件函数控制合并后的排序顺序
//取两个容器的差集，或者说在一个容器中存在另一个容易中不存在的元素
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_symmetric_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result);
template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator set_symmetric_difference (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, OutputIterator result, Compare comp);//用条件函数控制合并后的排序顺序
```

**函数使用**

```c++
merge (first,first+5,second,second+5,v.begin());//合并两个容器
set_union (first, first+5, second, second+5, v.begin());//无重复合并两个容器
set_intersection (first, first+5, second, second+5, v.begin());//取两个容器交集放到v中
set_difference (first, first+5, second, second+5, v.begin());//取first中有而second中没有的元素放到v中
set_symmetric_difference (first, first+5, second, second+5, v.begin());//取两个容器在对方容器没有的元素
```

#### 排序算法-搜索算法

前面在非修改性算法已经提到了搜索算法，这里列举两个针对有序区间的搜索算法，他仅返回bool值用以判断区间是否包含某个元素而已，当作了解就行，没有前面介绍的那些搜索算法好用，不过因为是有序区间搜索，其时间复杂度大大减少

**函数原型**

```c++
template <class ForwardIterator, class T>
  bool binary_search (ForwardIterator first, ForwardIterator last, const T& val);//判断区间是否有val元素
template <class ForwardIterator, class T, class Compare>
  bool binary_search (ForwardIterator first, ForwardIterator last, const T& val, Compare comp);//判断区间符合条件的元素
template <class InputIterator1, class InputIterator2>
  bool includes ( InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2 );//判断一个有序区间是否包含另一个有序区间
template <class InputIterator1, class InputIterator2, class Compare>
  bool includes ( InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, InputIterator2 last2, Compare comp );//判断区间内符合条件的另一个区间
```

**函数使用**

```c++
binary_search (v.begin(), v.end(), 3);//判断v中是否有3
includes(container,container+10,continent,continent+4)//判断continent前四个元素是否在container前10个元素中
```

#### 删除算法

stl提供了几个简单粗暴的删除函数，下面一一讲解

- remove():删除元素
- remove_if():删除指定条件的元素
- remove_copy():将删除结果拷贝到新容器中（原容器不变）
- remove_copy_if():删除指定条件的元素到新容器中（原容器不变）
- unique():删除连续重复元素，如122334221-->123421
- unique_copy():删除重复元素的结果到新容器中（原容器不变）

**函数原型**

```c++
template <class ForwardIterator, class T>
  ForwardIterator remove (ForwardIterator first, ForwardIterator last, const T& val);//remove
template <class ForwardIterator, class UnaryPredicate>
  ForwardIterator remove_if (ForwardIterator first, ForwardIterator last, UnaryPredicate pred);//remove_if
template <class InputIterator, class OutputIterator, class T>
  OutputIterator remove_copy (InputIterator first, InputIterator last, OutputIterator result, const T& val);//remove_copy
template <class InputIterator, class OutputIterator, class UnaryPredicate>
  OutputIterator remove_copy_if (InputIterator first, InputIterator last, OutputIterator result, UnaryPredicate pred);//remove_copy_if
template <class ForwardIterator>
  ForwardIterator unique (ForwardIterator first, ForwardIterator last);//unique
template <class ForwardIterator, class BinaryPredicate>
  ForwardIterator unique (ForwardIterator first, ForwardIterator last, BinaryPredicate pred);//unique
template <class InputIterator, class OutputIterator>
  OutputIterator unique_copy (InputIterator first, InputIterator last, OutputIterator result);//unique_copy
template <class InputIterator, class OutputIterator, class BinaryPredicate>
  OutputIterator unique_copy (InputIterator first, InputIterator last, OutputIterator result, BinaryPredicate pred);//unique_copy
```

**函数使用**

```c++
remove (pbegin, pend, 20);//删除20
bool IsOdd (int i) { return ((i%2)==0); }
remove_if (pbegin, pend, IsOdd);//删除偶数
remove_copy (myints,myints+8,myvector.begin(),20);//删除20的结果保存到myvector中
remove_copy_if (myints,myints+9,myvector.begin(),IsOdd);//删除偶数结果保存到myvector中
unique (myvector.begin(), myvector.end()); //删除连续重复元素
unique_copy (myints,myints+9,myvector.begin());//删除连续重复元素保存到myvector中
```

注：如果想删除所有重复元素仅保留一个可以先使用sort然后使用unique

关于算法的部分大概就是这些，这么多算法一下谁也记不住，把这篇文章当作参考吧，先得知道都有什么算法，然后等到用的时候查一下就可以了，跟多资料[参考这里](http://www.cplusplus.com/reference/algorithm/)

