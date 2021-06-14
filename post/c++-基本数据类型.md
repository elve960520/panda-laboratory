# C++ learning
> 最近在看C++ Primer，感觉这本书讲的比较详细，就把知识点记录一下，留作以后参考，就写了这个C++学习笔记。然后就现在开始吧，注：本系列基于C++11

## 基本数据类型
#### 算数类型
|类型|含义|最小尺寸(位)|
|:----|:----|:----|
|bool|布尔类型|未定义|
|char|字符|8|
|wchar_t|宽字符|16|
|char16_t|Unicode字符|16|
|char32_t|Unicode字符|32|
|short|短整型|16|
|int|整型|16|
|long|长整型|32|
|long long|长整型^c++11^|64|
|float|单精度浮点数|6位有效数字|
|double|双精度浮点数|10位有效数字|
|long double|扩展双精度浮点数|10位有效数字|
> 一个char和一个机器字节相同
> int，short，long，long long带符号，前面加unsigned ，表示范围扩大两倍

#### 类型转换
```C++
bool b = 42; //b为真
int i = b; //i的值为1
i = 3.14; //i的值为3
double pi = i; // pi的值为3.0
unsigned char c = -1; // c的值为255
signed char c2 = 256; // c的值未定义
```
**不要混用符号类型和无符号类型**

#### 转义符
- 换行符 __\n__
- 横向制表符 __\t__
- 报警符 __\a__
- 纵向制表符 __\v__
- 退格符 __\b__
- 双引号 __\\"__
- 反斜线 __\\\\__
- 问号 __\\?__
- 单引号 __\\'__
- 回车符 __\r__
- 进纸符 __\f__

#### 变量
基本形式：类型说明符，随后紧跟一个或多个变量名组成的列表，变量名以逗号分割，最后以分号结束。以下几种方法均可定义。
```C++
int num = o;
int num = {0};
int num{0};//c++11
int num(0);
```
###### 变量声明
为了允许把程序拆分成多个逻辑部分来写，C++支持分离式编译机制，将声明和定义区分开来。例：
```C++
extern int i;//声明i而非定义i
int j;//声明并定义j
extern int k = 10;//定义并声明了k
```

#### const限定符
定义后值不能改变
```C++
const int i = get_size();//正确：运行时初始化
const int j = 42;//正确：编译时初始化
const int k;//错误：k未经初始化
```
多个文件时如果想要共享const，需要extern，否则默认仅对本文件内有效
###### 顶层const
当有const型指针时，顶层const表示指针本身是个常量，底层const表示指针所指对象是个常量

#### constexpr^C++11^
const定义需要常量表达式，而在实际运行中，很难分辨是不是常量表达式，故用**constexpr**来编译器验证是否是常量表达式，同const，在定义时同样需要**初始化**

#### 类型别名
- 传统方法：typedef
```C++
typedef double wages;//wages是double的同义词
typedef wages base,*p;//base是double的同义词，p是double*的同义词
```
- 别名声明^C++11^
```C++
using SI = Sales_item;//SI是Sales_item的同义词
```

#### auto类型说明符^C++11^
auto让编译器替我们去分析表达式所属的类型
```C++
auto item = val1 + val2;//auto会初始化为val1和val2相加的结果
```

#### decltype ^C++11^
从表达式推断出要定义的变量类型，但是不用该表达式的值初始化该变量，这时可以用decltype，它返回操作数的数据类型。
```C++
const int ci = 0, &cj = ci;
decltype(ci) x = 0;//x的类型是const int
decltype(cj) y = x;//y的类型是const int& ，y绑定到x
decltype(cj) z;//错误：z是一个引用，必须初始化
```
```C++
decltype((i))//注意，当decltype内部是一个括号时，结果将是引用，必须初始化。
```
本次写的比较笼统，就把大体知识点写上去了，以后慢慢更新，预计下次更新把指针的引用添加进去
初次编辑2019年10月28日