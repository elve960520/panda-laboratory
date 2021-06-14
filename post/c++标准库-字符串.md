##C++标准库-字符串

在c++中，有两种字符串格式：

- basic_string：C++中的字符串模式
- c_string：继承自c字符串(char *)

*本文如不特殊介绍均为basic_string*

####字符串的构造

string s  //生成空字符串

string s(str) //复制str到s中

string s(str,stridx) //将str中始于stridx的部分复制到s中

string s(str,strbegin,strlen) //将str中始于strbegin长度为strlen的部分复制到s中

string s(beg,end) //将beg和end区间的部分复制到s中

string s(c_str) //将c_string类型的s_str复制到s中

string s(c_str,char_len) // 将c_string的前char_len个字符复制到s中

string s(num,c) //初始化s为num个c

#### 字符串的析构

~string() //销毁所有内存，释放内存

**例1：**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(){
    string str("12345678");
    char ch[] = "abcdefgh";
    string a; //定义空字符串
    
    string str_1(str);//复制str到str_1
    string str_2(str,2,5);//将str从第二个元素开始复制5个元素给str_2
    string str_3(ch,5); //将ch的前5个字符复制到str_3中
    string str_4(5,'x'); //生成5个x到str_4
    string str_5(str.begin(),str.end()); //将str的开头和结尾区间复制到str_5中

    cout<<str<<endl;
    cout<<ch<<endl;
    cout<<str_1<<endl;
    cout<<str_2<<endl;
    cout<<str_3<<endl;
    cout<<str_4<<endl;
    cout<<str_5<<endl;

    return 0;
}
```

#### 字符串的大小和容量

- size()和length()。这两个函数返回string字符串中字符的个数。
- max_size()。该函数返回string类型能够包含最多的字符数。
- capacity()。该函数返回在重新分配内存之前，string类型所能包含的最大的字符数。
- reserve()。该函数为string类型重新分配内存大小。输入参数为n，如果n大于当前的字符格式，则长度为n，否则保留现有长度。
- resize()。该函数也是为string类型重新分配大小，该函数不论n多少，均改变为n的大小。

**例2：**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(){
    int size = 0;
    int length = 0;
    unsigned long maxsize = 0;
    int capacity = 0;

    string str("12345678");
    string str_custom;
    str_custom = str;
    str_custom.reserve(5);

    size = str_custom.size();
    length = str_custom.length();
    maxsize = str_custom.max_size();
    capacity = str_custom.capacity();

    cout<<"size:"<<size<<endl;
    cout<<"length:"<<length<<endl;
    cout<<"maxsize:"<<maxsize<<endl;
    cout<<"capacity:"<<capacity<<endl;

    str_custom.resize(5);
    size = str_custom.size();
    length = str_custom.length();
    cout<<"size:"<<size<<endl;
    cout<<"length:"<<length<<endl;

    return 0;
}
```

#### 元素存储与访问

在string类中，可以使用两种元素访问方式，一种是[]，一种是at()。

注：[]不检查元素有效性，at检查元素有效性。

**例3:**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(){

    const string cs("conststring");
    string s("abcde");
    char temp = 0;
    char temp_1 = 0;
    char temp_2 = 0;
    char temp_3 = 0;
    char temp_4 = 0;
    char temp_5 = 0;

    temp = s[2]; //获取字符‘c’
    temp_1 = s.at(2); //获取字符‘c’
    temp_2 = s[s.length()]; //未定义行为，但是会获取到\0
    temp_3 = cs[cs.length()]; //指向字符\0
    // temp_4 = s.at(s.length()); //程序异常
    // temp_5 = cs.at(cs.length()); //程序异常

    cout<<temp<<temp_1<<temp_2<<temp_3<<endl;

    return 0;
}
```

**例4:**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(){

    string s("abcde");
    cout<<s<<endl;
    char& r = s[2];
    char* p = &s[3];
    r = 'X';
    *p = 'Y';
    cout<<s<<endl;
    s = "12345678";
    r = 'X';
    *p = 'Y';
    cout<<s<<endl;

    return 0;
}
```

#### 字符串的比较

字符串可以使用<,>,=,<=,>=,!=来比较，还可以用compare函数比较，当两个参数相同时，compare返回0，否则返回非零值。

| int compare (const string& str) const noexcept;              | 与另一个字符串str比较                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| int compare (size_t pos, size_t len, const string& str) const; | 从该字符串的pos开始len长度的字符串与str比较                  |
| int compare (size_t pos, size_t len, const string& str, size_t subpos, size_t sublen) const; | 从该字符串的pos开始len长度的字符串与str的subpos开始sublen长度的部分比较 |
| int compare (const char* s) const;                           | 与c_string字符串比较                                         |
| int compare (size_t pos, size_t len, const char* s) const;   | 将该字符串的pos开始len长度的字符串与s比较                    |
| int compare (size_t pos, size_t len, const char* s, size_t n) const; | 将该字符串的pos开始len长度的字符串与s的前n个字符比较         |

**例5**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(){

    string A("aBcdef");
    string B("Abcdef");
    string C("123456");
    string D("123dfg");

    int m = A.compare(B);
    int n = A.compare(1,5,B);
    int p = A.compare(1,5,B,4,2);
    int q = C.compare(0,3,D,0,3);

    cout<<"m:"<<m<<endl;
    cout<<"n:"<<n<<endl;
    cout<<"p:"<<p<<endl;
    cout<<"q:"<<q<<endl;

    return 0;
}
```

注：本文部分摘自[C++ Reference](http://www.cplusplus.com/reference/string/string/compare/)

#### 字符串内容变化

1. assign函数

   assign()函数可以直接赋值给字符串。

   | string& assign (const string& str);                          | 将str拷贝到原字符串中                             |
   | ------------------------------------------------------------ | ------------------------------------------------- |
   | string& assign (const string& str, size_t subpos, size_t sublen); | 将str中subpos开始sublen长度的字符串赋值给原字符串 |
   | string& assign (const char* s);                              | 将c_string格式字符串赋值给原字符串                |
   | string& assign (const char* s, size_t n);                    | 将c_string字符串前n个字符赋值给字符串             |
   | string& assign (size_t n, char c);                           | 将n个c字符赋值给原字符串                          |
   | template <class InputIterator> string& assign (InputIterator first, InputIterator last); | 将first到last区间的字符串赋值给原字符串           |
   | string& assign (initializer_list<char> il);                  | 将字符数组il拷贝到原字符串中                      |
   | string& assign (string&& str) noexcept;                      | 获取str的内容（没懂）                             |

2. operator =函数

   不必多说，赋值语句

3. erase函数

   erase()函数用以删除元素

   | iterator erase (const_iterator position);                   | 删除position位置的元素    |
   | ----------------------------------------------------------- | ------------------------- |
   | iterator erase (const_iterator first, const_iterator last); | 删除first到last之间的元素 |

4. swap函数

   swap()函数用以交换字符串

   | void swap (basic_string& str); | 将原字符串与str交换 |
   | ------------------------------ | ------------------- |
   |                                |                     |

5. insert函数

   insert()函数用以插入字符串

   | basic_string& insert (size_type pos, const basic_string& str); | 将str插入到pos之前                           |
   | ------------------------------------------------------------ | -------------------------------------------- |
   | basic_string& insert (size_type pos, const basic_string& str, size_type subpos, size_type sublen); | 将str的subpos起sublen长的字符串插入到pos之前 |
   | basic_string& insert (size_type pos, const charT* s);        | 将c_string字符串s插入到pos之前               |
   | basic_string& insert (size_type pos, const charT* s, size_type n); | 将s的前n个字符插入到pos之前                  |
   | basic_string& insert (size_type pos,   size_type n, charT c); | 将n个字符c插入到pos之前                      |
   | iterator insert (const_iterator p, size_type n, charT c);    | 将n个字符c插入到迭代器p之前                  |
   | iterator insert (const_iterator p, charT c);                 | 将字符c插入到迭代器p之前                     |
   | template <class InputIterator> iterator insert (iterator p, InputIterator first, InputIterator last); | 将first和last之间的字符串插入到迭代器p之前   |
   | basic_string& insert (const_iterator p, initializer_list<charT> il); | 将字符数组il插入到p之前                      |

6. append函数

   append()函数用以将字符串添加到末尾

   | basic_string& append (const basic_string& str);              | 将str添加到原字符串末尾                           |
   | ------------------------------------------------------------ | ------------------------------------------------- |
   | basic_string& append (const basic_string& str, size_type subpos, size_type sublen); | 将str从subpos开始sublen个字符串添加到原字符串末尾 |
   | basic_string& append (const charT* s);                       | 将c_string字符串s添加到原字符串末尾               |
   | basic_string& append (const charT* s, size_type n);          | 将s的前n个字符添加到原字符串末尾                  |
   | basic_string& append (size_type n, charT c);                 | 将n个字符c添加到原字符串末尾                      |
   | template <class InputIterator> basic_string& append (InputIterator first, InputIterator last); | 将迭代器first和last之间的字符串添加到原字符串末尾 |
   | basic_string& append (initializer_list<charT> il);           | 将字符数组il添加到原字符串末尾                    |

7. replace函数

   replace()函数用以替换字符串元素

   | basic_string& replace (size_type pos,     size_type len,     const basic_string& str); | 将原字符串中pos开始的len个字符替换为str                      |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | basic_string& replace (const_iterator i1, const_iterator i2, const basic_string& str); | 将原字符串中在i1和i2迭代器之间的字符替换为str                |
   | basic_string& replace (size_type pos, size_type len, const basic_string& str, size_type subpos, size_type sublen); | 将原字符串中pos开始的len个字符替换为str中的subpos开始的sublen个字符串 |
   | basic_string& replace (size_type pos,     size_type len,     const charT* s); | 将原字符串中pos开始的len个字符替换为c_string字符串s          |
   | basic_string& replace (const_iterator i1, const_iterator i2, const charT* s); | 将原字符串中在i1和i2迭代器之间的字符替换为c_string字符串s    |
   | basic_string& replace (size_type pos,     size_type len,     const charT* s, size_type n); | 将原字符串中pos开始的len个字符替换为c_string字符串s的前n个字符 |
   | basic_string& replace (const_iterator i1, const_iterator i2, const charT* s, size_type n); | 将原字符串中在i1和i2迭代器之间的字符替换为c_string字符串s的前n个字符 |
   | basic_string& replace (size_type pos,     size_type len,     size_type n, charT c); | 将原字符串中pos开始的len个字符替换为n个字符c                 |
   | basic_string& replace (const_iterator i1, const_iterator i2, size_type n, charT c); | 将原字符串中在i1和i2迭代器之间的字符替换为n个字符c           |
   | template <class InputIterator> basic_string& replace (const_iterator i1, const_iterator i2, InputIterator first, InputIterator last); | 将原字符串中在i1和i2迭代器之间的字符替换为迭代器first和last之间的字符 |
   | basic_string& replace (const_iterator i1, const_iterator i2, initializer_list<charT> il); | 将原字符串中在i1和i2迭代器之间的字符替换为字符数组il         |

**例6:**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(int argc, char const *argv[])
{
    cout<<"assign:"<<endl;
    string str1("123456");
    string str2("abcdefghijklmn");
    string str;

    str.assign(str1);
    cout<<str<<endl;

    str.assign(str1,3,3);
    cout<<str<<endl;

    str.assign(str1,2,str1.npos);
    cout<<str<<endl;

    str.assign(5,'X');
    cout<<str<<endl;

    string::iterator itB = str1.begin();
    string::iterator itE = str1.end();
    str.assign(itB,(--itE));
    cout<<str<<endl;

    cout<<"operation =:"<<endl;
    str = str1;
    cout<<str<<endl;

    cout<<"erase:"<<endl;
    str.erase(3);
    cout<<str<<endl;

    str.erase(str.begin(),str.end());
    cout<<str<<endl;

    str.swap(str2);
    cout<<str<<endl;

    cout<<"insert:"<<endl;
    string A("ello");
    string B("H");
    B.insert(1,A);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.insert(1,"yanchy",3);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.insert(1,A,2,2);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.insert(1,5,'C');
    cout<<B<<endl;

    A = "ello";
    B = "H";
    string::iterator it = B.begin()+1;
    const string::iterator itF = A.begin();
    const string::iterator itG = A.end();
    B.insert(it,itF,itG);
    cout<<B<<endl;

    cout<<"append:"<<endl;
    A = "ello";
    B = "H";
    cout<<"A="<<A<<"B="<<B<<endl;
    B.append(A);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.append("12345",2);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.append("12345",2,3);
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.append(10,'A');
    cout<<B<<endl;

    A = "ello";
    B = "H";
    B.append(A.begin(),A.end());
    cout<<B<<endl;

    cout<<"replace:"<<endl;
    string var("abcdefghijklmn");
    const string dest("1234");
    string dest2("567891234");
    var.replace(3,3,dest);
    cout<<"1:"<<var<<endl;

    var = "abcdefghijklmn";
    var.replace(3,1,dest.c_str(),1,3);
    cout<<"2:"<<var<<endl;

    var = "abcdefghijklmn";
    var.replace(3,1,5,'X');
    cout<<"3:"<<var<<endl;

    string::iterator itW,itX,itY,itZ;
    itW = var.begin();
    itX = var.end();
    var = "abcdefghijklmn";
    var.replace(itW,itX,dest);
    cout<<"4:"<<var<<endl;

    itY = dest2.begin()+1;
    itZ = dest2.end();
    var = "abcdefghijklmn";
    var.replace(itW,itX,itY,itZ);
    cout<<"5:"<<var<<endl;

    var = "abcdefghijklmn";
    var.replace(3,1,dest.c_str(),4);
    cout<<"6:"<<var<<endl;

    return 0;
}
```

#### 字符串拼接

除了append或者insert等方法，string类还提供了operation +方法

Str3 = str1+str2; //这句话就相当于将str1和str2拼接到一起并赋值给str3。

#### 字符串IO操作

字符串提供了三种io操作，分别是">>","<<",getline()

- ">>":将字符串输入到字符串中
- "<<":将字符串输出到流中
- getline():将整行的数据输入到字符串中

**例7**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(int argc, char const *argv[])
{
    string name;
    cout<<"input name"<<endl;
    cin>>name;
    cout<<name<<endl;
    cin >> ws; //清除缓存区 
    getline(cin,name);
    cout<<name<<endl;

    return 0;
}
```

#### 字符串查找

string类中字符串查找使用find函数，包括find(),rfind(),find_first_of(),find_last_of(),find_first_not_of(),find_last_not_of()。

| size_type find (const basic_string& str, size_type pos = 0) const noexcept; | 寻找字符串str在原字符串中的位置                         |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| size_type find (const charT* s, size_type pos = 0) const;    | 寻找c_string字符串s在原字符串中的位置                   |
| size_type find (const charT* s, size_type pos, size_type n) const; | 寻找c_string字符串s在原字符串中从pos开始n个字符中的位置 |
| size_type find (charT c, size_type pos = 0) const noexcept;  | 寻找字符c在原字符串中的位置                             |

其余五个函数用法同find函数一样。

**例8**

```c++
#include <iostream>
#include <string>

using namespace std;

int main(int argc, char const *argv[])
{
    string str_ch("for");
    string str("Hi ,Peter ,I'm sick.Please bought some drugs for me.");

    string::size_type m = str.find('P',5);//在str中从第5个字符开始寻找“P”的位置
    string::size_type rm = str.rfind('P',5);//在str中从第5个字符开始向前寻找“P”的位置
    cout<<"P in "<<(int)m<<endl;
    cout<<"P r in "<<(int)rm<<endl;

    string::size_type n = str.find("some",0);//在str中从0开始寻找some位置
    string::size_type rn = str.rfind("some",30);//在str中从30的位置开始向前寻找some位置
    cout<<"some in "<<(int)n<<endl;
    cout<<"some r in "<<(int)rn<<endl;

    string::size_type mo = str.find("Peter",4,5);//在str中从4开始寻找Peter中前5个字符
    string::size_type rmo = str.rfind("drug",-1,4);//在str中从后向前寻找drug中前5个字符
    cout<<"drug in "<<(int)mo<<endl;
    cout<<"drug r in "<<(int)rmo<<endl;

    string::size_type no = str.find(str_ch,0);//在str中从0开始寻找str_ch的位置
    string::size_type rno = str.rfind(str_ch,0);//在str中从0开始向前寻找str_ch的位置
    cout<<"for in "<<(int)no<<endl;
    cout<<"for r in "<<(int)rno<<endl;

    return 0;
}
```

#### 字符串迭代器

在string类中有几个迭代器可以直接使用

- begin():字符串开始
- end():字符串结尾
- rbegin():指向字符串最后一个字符，+1会指向第一个字符
- rend():指向字符串第一个字符之前的理论元素

