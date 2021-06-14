## C++标准库-数值计算

标准库提供了一些数值运算模版，虽然在写普通程序中一般用不到，但是在做一些专业数学领域的内容时可能会用到，下面做以介绍

#### 复数及其运算

c++提供了一个复数模版，用以实现复数操作。复数有实部和虚部，虚部平方为负数，更多关于复数内容可以查看高中数学知识点。

复数在使用的时候需要包含<complex>头文件

**基本定义**

```c++
#include <complex> 
complex<double> first (2.0,2.0);
complex<double> second (first);
complex<long double> third (second);
```

目前所知complex支持float，double，long double

**相位角定义**

```c++
#include <complex>
complex<double> forth(polar (2.0, 0.5));//根据模长和相位角生成复数
```

**复数的实部和虚部**

```c++
double myreal = mycomplex.real();//实部
double myimag = mycomplex.imag();//虚部
mycomplex.real(3.5)；//实部赋值
mycomplex.imag(2.5);//虚部赋值
```

**算数运算**

复数支持=、+、-、*、/、+=、-=、\*=、/=、==、!=、<<、>>操作

```c++
#include <iostream>     // std::cout
#include <complex>      // std::complex

using namespace std;

int main ()
{
  complex<double> first(10.0,5.0);
  complex<double> second(first);
  complex<double> third;

  third.real(3.5);
  third.imag(2.5);
  cout<<first<<endl;
    
  third = first + second;
  cout<<third<<endl;
  
  second = complex<double>(5.0,2.5);
  third = first - second;
  cout<<third<<endl;
  
  third = first * second;
  cout<<third<<endl;
  
  third = first / second;
  cout<<third<<endl;
  
  third += first;
  cout<<third<<endl;
  
  third -= second;
  cout<<third<<endl;
  
  third *= first;
  cout<<third<<endl;
  
  third /= second;
  cout<<third<<endl;
  
  third = first;
  cout<<(third == complex<double>(10.0,5.0))<<endl;

  return 0;
}
```

**常用计算函数**

复数有几个常用的计算方法：

- abs():求模
- norm():绝对值平方
- arg():求复数相位
- conj():求共轭复数
- sin():正弦
- cos():余弦
- tan():正切
- sinh():双曲正弦
- cosh():双曲余弦
- tanh():双曲正切
- pow():幂运算
- exp():e为底幂运算
- sqrt():平方根
- log():以10为底对数

```c++
#include <iostream>     // std::cout
#include <complex>      // std::complex

using namespace std;

int main ()
{
  complex<double> mycomplex(10.0,5.0);

  cout<<"abs:"<<abs(mycomplex)<<endl;
  cout<<"norm:"<<norm(mycomplex)<<endl;
  cout<<"arg:"<<arg(mycomplex)<<endl;
  cout<<"conj:"<<conj(mycomplex)<<endl;

  cout<<"sin:"<<sin(mycomplex)<<endl;
  cout<<"cos:"<<cos(mycomplex)<<endl;
  cout<<"tan:"<<tan(mycomplex)<<endl;
  cout<<"sinh:"<<sinh(mycomplex)<<endl;
  cout<<"cosh:"<<cosh(mycomplex)<<endl;
  cout<<"tanh:"<<tanh(mycomplex)<<endl;

  cout<<"pow:"<<pow(mycomplex,mycomplex)<<endl;
  cout<<"exp:"<<exp(mycomplex)<<endl;
  cout<<"sqrt:"<<sqrt(mycomplex)<<endl;
  cout<<"log:"<<log(mycomplex)<<endl;

  return 0;
}
```

#### 数组（向量）及其运算

c++提供了向量的运算，用在专业数学或计算机领域，方便进行数据计算，不建议普通程序使用。其类为valarray，

使用时需包含<valarray>头文件，默认生成的为一维数据，通过操作可以实现二维三维等。

**向量定义**

```c++
#include <iostream>
#include <valarray>

using namespace std;

int main(int argc, char const *argv[])
{
  int init[]= {10,20,30,40};
  valarray<int> first;                             // 空向量
  valarray<int> second (5);                        // 5个元素0
  valarray<int> third (10,3);                      // 3个初值为10的元素
  valarray<int> fourth (init,4);                   // 包含4个元素，且元素值与init一致
  valarray<int> fifth (fourth);                    // 从另一个向量拷贝过来
  valarray<int> sixth (fifth[slice(1,2,1)]);  //分割出一个向量（后续章节）

  cout << "sixth sums " << sixth.sum() << '\n';

    return 0;
}
```

**成员函数与操作**

- []:向量支持下标操作，可直接使用下标进行赋值等操作
- operator +：实现两个向量相加操作
- operator -：实现两个向量相减操作
- operator *：实现两个向量相乘操作
- operator /：实现两个向量相除操作
- operator %：实现两个向量取余
- operator ～：向量取反
- size():向量大小
- max():最大值
- min():最小值
- sum():求和
- resize():重新设置向量大小
- shift():逻辑移位（空位填0，默认向左为正）
- cshift():循环移位（将空位由另一端元素填上，默认向左为正）
- apply():参数为函数，为向量中每一个元素执行该函数
- free():删除所有元素，长度设置为0

这些成员函数和操作在上一章大部分都已经讲过了，这里不再赘述，用法都比较简单，一看便会，还有一些超越函数等，以下函数需包含<cmath>头文件，且操作是对向量中每一个元素进行操作

- abs():求绝对值
- pow():幂运算
- exp():e为底幂运算
- sqrt():求平方根
- log():求对数
- log10():求以10为底对数
- sin():正弦
- cos():余弦
- tan():正切
- sinh():双曲正弦
- cos():双曲余弦
- tanh():双曲正切
- asin():反正弦
- acos():反余弦
- atan():反正切
- atan2():反正切值

**slice和slice_array**

slice是切割的意思，这个函数可以实现将valarray函数分割开来，形成一个新的向量，通过slice可以使valarray有了更多维数，slice_array是将valarray切割出来的子集转换成的类，可以方便以后进行运算或转化成valarray，这两个的关系一时可能分不太清，可以这么比如：slice是一种操作，其作用是将valarray分割开来，而分割出来的子向量就是slice_array（这个比喻不知道是否恰当，我是这么理解的，欢迎指正）

**slice使用**

```c++
#include <iostream>
#include <valarray>

using namespace std;

int main(int argc, char const *argv[])
{
    valarray<double> v1(10);
    for (int i = 0; i < 10; ++i)
    {
        v1[i] = i;
    }
    for (int i = 0; i < 10; ++i)
    {
        cout<<v1[i];
    }
    cout<<endl;

    valarray<double> v2 = v1[slice(1,3,2)];
    for (int i = 0; i < 3; ++i)
    {
        cout<<v2[i];
    }
    cout<<endl;

    return 0;
}
```

**gslice和gslice_array**

gslice比slice更高级一些，能够切分二维数组或切分成二维数组，甚至支持三维及以上，其用法也会比slice复杂一些，第一种用法和slice一样，可以使用gslice(1,length,step)来切分，不做赘述，当二维及以上时，可以使用以下方法

**使用方法**

```c++
#include <iostream>
#include <valarray>
#include <cstddef>

using namespace std;

int main(int argc, char const *argv[])
{
    valarray <double> myarray(16);
    for (int i = 0; i < 16; ++i)
    {
        myarray[i] = i;
    }
    for (int i = 0; i < 16; ++i)
    {
        cout<<myarray[i];
    }
    cout<<endl;

    size_t start =1;
    size_t lengths[] = {2,3};//两行三列
    size_t strides[] = {7,2};//行间距7，列间距2

    gslice mygslice(start,
        valarray<size_t> (lengths,2),
        valarray<size_t> (strides,2));//生成切割方式

    valarray <double> outarray = myarray[mygslice];
    for (int i = 0; i < 6; ++i)
    {
        cout<<outarray[i];
    }
    cout<<endl;

    return 0;
}
```

**分割方式**

```c++
                    [0][1][2][3][4][5][6][7][8][9][10][11][12][13][14][15]
start=1:                *
                        |
size=2, stride=7:       *--------------------*
                        |                    |
size=3, stride=2:       *-----*-----*        *------*------*
                        |     |     |        |      |      |
gslice:                 *     *     *        *      *      *
                    [0][1][2][3][4][5][6][7][8][9][10][11][12][13][14][15]
```

根据参数start=1可以判断是从下标为1开始，生成2行3列的数组，两个行之间的距离为7，两个列之间的距离为2，根据上图应该可以清楚的看到，从1开始取第一行数据，相隔7个的8开始取第二行数据，每行取三个，间隔为2，结果就为{1,3,5,8,10,12}

**mask_array**

valarray提供了一种描述子集的方式：屏蔽子集。可以使一些操作仅对某些元素有效，其原理相当于在原数组上叠加了一层数组，此叠加的数组位上如果是1则可以进行运算，若为0则不进行运算。类似于图像里的蒙版，该函数大大方便了操作向量。

**mask_array使用**

```c++
#include <iostream>
#include <valarray>

using namespace std;

int main(int argc, char const *argv[])
{
    valarray <int> myarray(10);
    valarray <bool> mymask(10);

    for (int i = 0; i < 10; ++i)
    {
        myarray[i] = i+1;
        mymask[i] = ((i%2)==1);
    }
    for (int i = 0; i < 10; ++i)
    {
        cout<<myarray[i];
    }
    cout<<endl;

    myarray[mymask] = valarray<int> (10,5);

    for (int i = 0; i < 10; ++i)
    {
        cout<<myarray[i];
    }
    cout<<endl;

    return 0;
}
```

**Indirect_array**

间接数据子集可以重新排列任意元素，利用下标操作，可以将原向量排列顺序重新排列或取部分元素。

**indirect_array使用**

```c++
#include <iostream>     // std::cout
#include <cstddef>      // std::size_t
#include <valarray>     // std::valarray

using namespace std;

int main ()
{
  valarray<int> foo (8);
  for (int i=0; i<8; ++i) foo[i]=i;             //  0  1  2  3  4  5  6  7

  size_t sel[] = {3,5,6};
  valarray<size_t> selection (sel,3); //           *     *  *

  foo[selection] = 0;                           //  0  1  2  0  4  0  0 7

  cout << "foo:";
  for (size_t i=0; i<foo.size(); ++i)
    cout << ' ' << foo[i];
  cout << '\n';

  return 0;
}
```

#### 通用数值计算

**求和算法**

accumulate()是针对数值的求和函数，其可以计算数组/向量的和。

**函数原型**

```c++
template <class InputIterator, class T>
   T accumulate (InputIterator first, InputIterator last, T init);//求first和last的和并放到init中
template <class InputIterator, class T, class BinaryOperation>
   T accumulate (InputIterator first, InputIterator last, T init,
                 BinaryOperation binary_op);//对first和last区间的值先进行binary_op操作，然后求和放到init中
```

**函数使用**

```c++
accumulate(numbers,numbers+3,init);
```

**内积算法**

inner_product()算法可以求两个数组/向量的对应位乘积之和

**函数原型**

```c++
template <class InputIterator1, class InputIterator2, class T>
   T inner_product (InputIterator1 first1, InputIterator1 last1,
                    InputIterator2 first2, T init);//求first1到last1区间的元素乘first对应位置的元素相加放到init中
template <class InputIterator1, class InputIterator2, class T,
          class BinaryOperation1, class BinaryOperation2>
   T inner_product (InputIterator1 first1, InputIterator1 last1,
                    InputIterator2 first2, T init,
                    BinaryOperation1 binary_op1,
                    BinaryOperation2 binary_op2);//将first1到last1区间的元素和first对应位置的元素先进行binary_op2运算，然后对每个结果进行binary_op1运算结果放到init中
```

**函数使用**

```c++
inner_product(series1,series1+3,series2,init);
```

**部分和算法**

partial_sum()算法也可以称作前n项和，在结果序列中，每一项等于原序列的前n项和：

y0 = x0
y1 = x0 + x1
y2 = x0 + x1 + x2
y3 = x0 + x1 + x2 + x3
y4 = x0 + x1 + x2 + x3 + x4
... ... ...

**函数原型**

```c++
template <class InputIterator, class OutputIterator>
   OutputIterator partial_sum (InputIterator first, InputIterator last,
                               OutputIterator result);//将first和last区间的前n项和结果放到result中
template <class InputIterator, class OutputIterator, class BinaryOperation>
   OutputIterator partial_sum (InputIterator first, InputIterator last,
                               OutputIterator result, BinaryOperation binary_op);//将first和last区间的元素执行binary_op操作，将结果放到result中
```

**函数使用**

```c++
partial_sum (val, val+5, result);
```

**序列相邻差算法**

adjacent_difference()算法可以计算相邻两个元素的差值，并将结果放到result中

y0 = x0
y1 = x1 - x0
y2 = x2 - x1
y3 = x3 - x2
y4 = x4 - x3
... ... ...

**函数原型**

```c++
template <class InputIterator, class OutputIterator>
   OutputIterator adjacent_difference (InputIterator first, InputIterator last,
                                       OutputIterator result);//求first到last的相邻差并将结果放到result序列中
template <class InputIterator, class OutputIterator, class BinaryOperation>
   OutputIterator adjacent_difference ( InputIterator first, InputIterator last,
                                        OutputIterator result, BinaryOperation binary_op );//对first到last区间的相邻元素执行binary_op操作，并将结果放到result中
```

**函数使用**

```c++
adjacent_difference (val, val+7, result);
```

#### 数学函数

前面已经提到了一些数学函数，下面进行以下总结

```c++
#include <cstdlib>
abs(int n);//求绝对值      参数:（long int n）（long long int n）
labs(long int n);//求绝对值
div(int numer,int denom);//带余数除法    参数：(long int numer,long int denom) (long long int numer, long long int denom)
ldiv(long int numer, long int denom);//带余数除法
srand(unsigned int seed);//生成随机数，和rand配合使用
```

```c++
#include <cmath>
cos(long double x);//余弦
sin(long double x);//正弦
tan(long double x);//正切
acos(long double x);//反余弦
asin(long double x);//反正弦
atan(long double x);//反正切
atan2(long double y, long double x);//反y/x正切
cosh(long double x);//双曲余弦
sinh(long double x);//双曲正弦
tanh(long double x);//双曲正切
acosh(long double x);//反双曲余弦
asinh(long double x);//反双曲正弦
atanh(long double x);//反双曲正切
pow(long double base, long double exponent);//幂运算
exp(long double x);//以e为底的幂运算
sqrt(long double x);//求平方根
log(long double x);//求e为底对数
log10(long double x);//求10为底对数
ceil(long double x);//向上舍入，返回不小于x的最小整数
floor(long double x);//向下舍入，返回不大于x的最大整数
abs(long double x);//求绝对值
fabs(long double x);//求绝对值
fmod(long double numer, long double denom);//求含浮点余数
frexp(long double x, int* exp);//把一个浮点数分解为尾数和指数
ldexp(long double x, int exp);//计算x乘2的exp次方
modf(long double x, long double* intpart);//将x分别为整数和小数，小数部分由形参返回
```

关于数值计算大概就了解这么多，这一章内容具有专业性，如果对这方面有兴趣可以深究