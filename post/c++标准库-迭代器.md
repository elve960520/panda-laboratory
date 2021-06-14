## C++标准库-迭代器

迭代器是容器和算法之间的纽带

迭代器将容器序列抽象成元素，而不用去理会容器内的数据类型，其用法像指针，也可以说指针是一种特殊的迭代器

#### 迭代器分类

- 输入迭代器：可以读取迭代器的内容，但是不能修改
- 输出迭代器：可以修改迭代器的内容，但是不能读取
- 前向迭代器：既可以读取也可以修改迭代器内容，但是只能一个方向移动（++）
- 双向迭代器：在前向迭代器基础上可以实现双向移动
- 随机访问迭代器：在双向迭代器基础上增加了随机访问功能，这种使用最方便快捷功能也最多

<table style="valign:middle">
	<tr>
  <th colspan="4" >类别</th>
  <th >属性</th>
  <th >表达式</th>
</tr>
<tr>
  <td colspan="4" rowspan="2">所有类别</td>
  <td>构造、拷贝、删除</td>
  <td> X b(a)</br> b = a </td>
</tr>
<tr>
  <td>递增</td>
  <td>++a </br> a++</td>
</tr>
<tr>
  <td rowspan="10">随机访问迭代器</td>
  <td rowspan="6">双向迭代器</td>
  <td rowspan="5">向前迭代器</td>
  <td rowspan="2">输入迭代器</td>
  <td>比较</td>
  <td>a==b </br> a!=b </td>
</tr>
<tr>
  <td>解引用用作右值</td>
  <td>*a </br> a->m</td>
</tr>
<tr>
  <td>输出迭代器</td>
  <td>解引用用作左值</br> (仅适用于可修改容器)</td>
  <td>*a=t </br> *a++ = t</td>
</tr>
<tr>
  <td rowspan="2"> </td>
  <td>有默认构造函数</td>
  <td>X a</br> X()</td>
</tr>
<tr>
  <td>多解引用均有效</td>
  <td>{a=b;*a++;*b;}</td>
</tr>
<tr>
  <td colspan="2"> </td>
  <td>可以递减</td>
  <td>--a </br> a-- </br> * a--</td>
</tr>
<tr>
  <td rowspan="4" colspan="3"> </td>
  <td>支持算数运算符</td>
  <td>a + n </br> n + a </br> a - n </br> a - b</td>
</tr>
<tr>
  <td>支持比较运算符</td>
  <td>a < b </br> a > b </br> a <= b </br> a >= b</td>
</tr>
<tr>
  <td>支持复合赋值操作</td>
  <td>a += n </br> a -= n</td>
</tr>
<tr>
  <td>支持下标引用</td>
  <td>a[n]</td>
</tr>
</table>

#### 迭代器函数

**前进函数**

由于所有迭代器都支持前进，所以此函数适合全部迭代器，由于随机访问迭代器本身就可以随意移动，此函数对于list等容器的迭代器比较有用，大大方便了迭代器移动

函数原型

```c++
template <class InputIterator, class Distance>
  void advance (InputIterator& it, Distance n);//it是要前进的迭代器，n是前进步数（在双向迭代器中该值可以为负数，代表向后移动）
```

函数使用

```c++
list<int>::iterator it = mylist.begin();//生成迭代器指向mylist首节点
advance (it,5);//现在指向第五个节点
```

**距离函数**

该函数可以计算两个迭代器之间的距离，使用时需包含<iterator>头文件

函数原型

```c++
template<class InputIterator>
  typename iterator_traits<InputIterator>::difference_type
    distance (InputIterator first, InputIterator last);//fisrt和last分别指向两个迭代器
```

函数使用

```c++
list<int>::iterator first = mylist.begin();
list<int>::iterator last = mylist.end();
distance(first,last);//测量距离
```

**交换函数**

该函数用以交换两个迭代器指向的元素的内容，<u>注：是交换元素内容，并不是交换两个迭代器的指向，迭代器还是指向原来的位置</u>

函数原型

```c++
template <class ForwardIterator1, class ForwardIterator2>
  void iter_swap (ForwardIterator1 a, ForwardIterator2 b);//a和b指向两个迭代器
```

函数使用

```c++
iter_swap(myints,myvector.begin());//交换myints数组和myvector容器的首元素
```

