# C++

# 1 基础语法

## 1.1 变量和基本类型

### 1.1.1 基本内置类型

* `int` `float` `double` `long long` ……

* 赋值过程中的强制类型转换 

  ~~~c++
  int -> bool// 0 -> false; 非0 -> true
  bool ->int//false -> 0; true -> 1
  float -> int//保留整数部分
  int -> float// 小数部分记0, 可能出现精度丢失
  超范围的数 -> unsigned char// %255
  超范围的数 -> char//异常结果
  ~~~

* 字面值常量

  ~~~c++
  20/*十进制*/	024/*八进制*/	0x14/*十六进制*/
  3.14159	3.14169E0 0. 0e0 .001//小数或整数部分为0可省略
  'a' "abc"
  '\n' '\t' '\0' '\\' '\"' '\40' //转义字符
  //用前后缀指定字面值的类型
  234U 123L 123LL 123ULL 3.14f 3.14L//后缀
  ~~~



### 1.1.2 变量

* 定义与初始化

  ~~~c++
  int a = 0;
  int a(0);
  int a{0};//列表初始化，存在信息丢失风险时报错
  int a;//默认初始化
  ~~~

* 声明和定义

  ~~~c++
  extern int i;//声明
  extern int i = 1;//显示初始化，声明变为定义
  ~~~

* 名字作用域(根据定义的位置判断)

  ~~~c++
  全局作用域: 所有{}外 
  块作用域: 仅在{}内有作用
  嵌套作用域: 内层可访问外层定义的变量，也可以重新定义以替代
  ~~~


* 引用

  ~~~c++
  int a = 2;
  int &b = a;//与a绑定在一起，必须初始化，后续无法重新绑
  int &c = 1;//错误，无法初始化为字面值
  ~~~

* 指针  

  ~~~c++
  int a = 1;
  int *b = &a;
  int *c = nullptr;
  int *c = 0;
  void *p = &a;//可以存放任何类型的指针，但无法直接访问所指对象
  ~~~

* `const`

  ~~~c++
  const int a = 1;//必须初始化
  ~~~

* **`const`的引用**

  ~~~c++
  const int a = 1;
  const int &b = a;
  const int &c = 21;//可以对字面值进行常量引用
  int d = 2;
  const int &e = d;//允许对非常量进行常量引用，不能通过别名进行修改
  ~~~

* **指针和`const`**

  ~~~c++
  const int a = 1;
  const int *b = &a;//const的指针，底层const，不能修改所指对象
  int c = 1;
  int *const c = &a;//常量指针，顶层const，不能修改指针
  //拷贝赋值时，底层const需要指明，顶层const几乎不受影响
  const int d = *b;
  int e = *c;
  ~~~
  
* `constexpr`和常量表达式 

  ~~~c++
  //编译时计算并获得结果
  constexpr int a = 1;//可以让编译器验证是否为常量表达式，只有字面值类型可用
  constexpr int *a = nullptr;//顶层指针，constexpr只对指针有效
  ~~~

* `typedef`  `using ` `auto` `decltype` 

  ~~~c++
  typedef char* pstring;
  using pstring = char*;
  const pstring c;//pstring类型的常量，即常量 char指针，不要将原表达式带回
  auto a = 1;
  decltype(a) b = 2;
  decltype((a)) c = a;//添加括号则是引用，需要初始化
  ~~~

* `struct`  `class`



## 1.2 字符串、向量和数组

### 1.2.1 字符串

* `string`

  ~~~c++
  //除=拷贝初始化、括号直接初始化外
  string s1(10,'a');//单个字符重复n次
  getline(is,s1);//读is一行至s，返回is
  s1.empty();
  string::size_type s = s1.size();//无符号，最小为0
  //支持 + = == != < > <= >=
  string s2 = s1 + "abc" + "def";//正确
  string s3 = "abc" + s1 + "def";//正确
  string s4 = "abc" + "def" + s1;//错误，字符串字面值不能直接相加
  ~~~



### 1.2.2 向量

* `vector`

  ~~~c++
  //初始化
  vector<int> v2(v1);
  vector<int> v2 = v1;
  vector<int> v3(10,123);
  vector<int> v4(10);
  vector<int> v5{1,2,3};
  vector<int> v5 = {1,2,3};
  vector<int>::size_type = v5.size()
  //支持 + = == != < > <= >=
  ~~~



### 1.2.3 数组

* `auto arr[n]`

  ~~~c++
  int arr1[10];
  int arr1[] = {1,2,3};
  int arr1[10] = {1,2,3};//后面默认初始化
  //不允许数组间的拷贝与赋值
  int *(&arry)[10] = ptrs;//int *ptrs[10]的引用
  ~~~



## 1.3 表达式

包含一个或多个**运算对象**，对表达式求值会得到一个结果。

### 1.3.1 基础

* 运算符（一元、二元、三元）

* 运算对象类型转换

* 重载运算符

* 左值右值（右值无法作为赋值对象）

* 优先级和结合律

* **求值顺序**

  与优先级无关，若在表达式中修改了某个运算对象的值，避免在这个表达式其他地方再使用这个运算对象。



### 1.3.2 运算符

* 算数运算符

  `+` `-` 一元运算符；  

  `+` `-` `*` `/` `%` 二元运算符；  

  运算中**小整数**类型运算对象变为**大整数**类型运算对象。

* 逻辑和关系运算符

  `!`  一元，满足**右结合律**；  

  `<` `<=` `>` `>=` `==` `!=` `&&` `||` 二元，满足**左结合律**；  

  `&&` `||` 先对左侧运算对象求值，再根据结果对右侧选择性求值。


* **赋值运算符**`=`

  只能给可修改左值赋值，**优先级低**。

* 递增递减运算符`++` `--`

* 成员访问运算符`.` `->`

* **条件运算符** `a<90?c:d`满足**右结合律**

* 位运算符`~` `<<` `>>` `&`  `^` `|`

* `sizeof`

  ~~~c++
  int size = sizeof(arr)/sizeof(*arr) //数组元素数量
  ~~~

* `,`从左往右求值，返回最右侧表达式的结果



### 1.3.3 类型转换和运算符优先级

* 隐式转换（赋值，条件语句中转`bool`，小整数转大整数，算数运算）

* 显示转换 `cast-name<type>(expression)`

  ~~~c++
  double slope = static_cast<double>(j)/i;//不包含底层const可用
  const char* pc;
  char *p = const_cast<char*>(pc);//只对底层const使用，解掉const,不能改变类型
  int *ip;
  char *pc = reinterpret_cast<char*>(ip);
  ~~~

* 优先级（优先级从上至下，同一表内相同）

  | 运算符 | 功能           | 结合律 |
  | ------ | -------------- | ------ |
  | `::`   | 全局作用域     | 左     |
  | `::`   | 类作用域       | 左     |
  | `::`   | 命名空间作用域 | 左     |

  
  | 运算符 | 功能           | 结合律 |
  | ------ | -------------- | ------ |
  | `.`    | 成员选择       | 左     |
  | `->`   | 成员选择       | 左     |
  | `[]`   | 下标访问       | 左     |
  | `()`   | 函数调用       | 左     |
  | `()`   | 类型构造       | 左     |
  
  | 运算符          | 功能         | 结合律 |
  | --------------- | ------------ | ------ |
  | `++`            | 后置递增     | 右     |
  | `--`            | 后置递减     | 右     |
  | `typeid`        | 类型ID       | 右     |
  | `typeid`        | 运行时类型ID | 右     |
  | `explicit cast` | 类型转换     | 右     |
  
  
  | 运算符     | 功能         | 结合律 |
  | ---------- | ------------ | ------ |
  | `++`       | 前置递增     | 右     |
  | `--`       | 前置递减     | 右     |
  | `~`        | 位求反       | 右     |
  | `!`        | 非           | 右     |
  | `-`        | 负号         | 右     |
  | `+`        | 正好         | 右     |
  | `*`        | 解引用       | 右     |
  | `&`        | 取地址       | 右     |
  | `()`       | 类型转换     | 右     |
  | `sizeof`   | 对象大小     | 右     |
  | `new`      | 创建对象     | 右     |
  | `delete`   | 释放对象     | 右     |
  | `noexcept` | 能否抛出异常 | 右     |
  
  | 运算符     | 功能         | 结合律 |
  | ---------- | ------------ | ------ |
  | `->*`       | 指向成员选择的指针 | 左     |
  | `.*`       | 指向成员选择的指针 | 左     |

  | 运算符 | 功能 | 结合律 |
  | ------ | ---- | ------ |
  | `*`    | 乘法 | 左     |
  | `/`    | 除法 | 左     |
  | `%`    | 取余 | 左     |

  | 运算符 | 功能   | 结合律 |
  | ------ | ------ | ------ |
  | `+`    | 加法   | 左     |
  | `-`    | 减法法 | 左     |
  
  | 运算符 | 功能 | 结合律 |
  | ------ | ---- | ------ |
  | `<<`   | 左移 | 左     |
  | `>>`   | 右移 | 左     |
  
  | 运算符     | 功能         | 结合律 |
  | ---------- | ------------ | ------ |
  | `<`       | 小于     | 左    |
  | `<=`       | 小于等于     | 左    |
  | `>`        | 大于       | 左    |
  | `>=`        | 大于等于           | 左    |
  
  | 运算符     | 功能         | 结合律 |
  | ---------- | ------------ | ------ |
  | `==`     | 相等   | 左    |
  | `！=`     | 不等   | 左    |
  
* 下表优先级表内从上至下
  | 运算符     | 功能     | 结合律 |
  | ---------- | -------- | ------ |
  | `&`        | 位与     | 左     |
  | `^`        | 位异或   | 左     |
  | `\|`        | 位或     | 左     |
  | `&&`       | 与       | 左     |
  | `\|\|`      | 或       | 左     |
  | `?:`       | 条件     | 右     |
  | `=` `*=`等 | 赋值     | 右     |
  | `throw`    | 抛出异常 | 右     |
  | `,`        | 逗号     | 左     |



## 1.4 语句

### 1.4.1 条件语句

* `if` `else` `else if`

* `switch`

  ~~~c++
  switch(ch){
      case 'a':
      	cout << 1;
          break;
      case 'b':
      	cout << 2;
          break;    
  }
  ~~~

  ~~~c++
  switch(ch){
      case 'a':case 'b':
      	cout << 1;
          break;
      default:
      	cout << 0;
          break;    
  }
  ~~~



### 1.4.2 迭代语句

* `while`

* `do while`

* `for`

  ~~~c++
  vector<int>arr(10,1);
  for(int i = 0;i < arr.size();i++){
      ++arr[i];
  }
  ~~~

  ~~~c++
  for(auto &x: arr){
      ++x;
  }
  ~~~



### 1.4.3 跳转语句

* `break`

* `continue`

* `goto`

  ~~~c++
  int i = 0;
  label:
  	if(++i<10){
          goto label;
      }
  ~~~



### 1.4.4 异常

* `throw`

  ~~~c++
  if (!course.meb(stu.name)){
  	throw runtime_error("未选修此课程");
  }
  cout<<stu.name<<"选修了"<<course.name;
  ~~~

* `try`

  ~~~c++
  try{
      if (!course.meb(stu.name)){
  		throw runtime_error("未选修此课程");
  	}
  }catch(runtime_error err){
      cout<<err.what();
  }
  ~~~

* 标准异常类

  ~~~c++
  exception;
  runtime_error;
  range_error;
  overflow_error;
  underflow_error;
  logic_error;
  domain_error;
  invalid_argument;
  length_error;
  out_of_range;
  ~~~



## 1.5 函数

### 1.5.1 基础

* 实参和形参

* **静态局部对象`static`**
* 函数声明



### 1.5.2 参数传递

* 传值

* 传指针（传数组等同于传指针）

* 传引用

* **形参的顶层`const`会被忽略**

  ~~~c++
  //形参列表相同，无法区分
  int f(const int a);
  int f(int a);
  ~~~

* **不修改实参时尽量使用常量引用**

* 可变形参，类型相同

  ~~~c++
  //与vector不同，initializer_list对象元素恒为常量值，无法修改
  void f(initializer_list<int> l1){
      for(auto x: l1){
          cout << x <<' ';
      }
  }
  initializer_list<int> l1{1,2,3};
  f(l1);
  ~~~

* 省略符形参（`varargs` c标准库）

* 默认实参



### 1.5.3 返回值

* **不要返回局部变量的指针或引用**

* **引用返回左值（可赋值）**

  ~~~c++
  int &f(vector<int> &x){
      return x[0];
  }
  f(x)=2;//x[0]=2
  ~~~

* 列表初始化返回值

  ~~~c++
  vector<int> f(){
      return {1,2,3};
  }
  ~~~



### 1.5.4 重载

* 根据形参列表匹配函数（忽略顶层`const` 和形参名）

* **`const_cast`重载**

  ~~~c++
  const string& f1(const string& a, const string& b) {
  	return a.size() < b.size() ?a:b;
  }
  //非常量引用返回常量引用
  const string& f1(string& a, string& b) {
  	auto &res = f1(const_cast<const string&>(a), 
                     const_cast<const string&>(b));
  	return const_cast<const string&>(res);
  }
  ~~~

* 重载和作用域  

  局部作用域中声明函数会屏蔽函数的其它形式



### 1.5.5 内联函数和`constexpr`

* 内联函数`inline`，编译时”内联地“展开，消除运行函数的开销；

  ~~~c++
  //没有while switch 不递归调用
  inline int f(int a){
      return a*2;
  }
  ~~~

* `constexpr`函数（能用于常量表达式的函数）

  ~~~c++
  //返回值和形参类型都为字面值类型，只有一条return;
  constexpr int f(int a){
      return a+2;
  }
  ~~~



### 1.5.6 函数指针

* 定义和初始化

  ~~~c++
  int f(int a, int b);
  int (*pf)(int a, int b) = nullptr;//f 的函数指针
  pf = &f;//&可选择性加
  ~~~

* 调用

  ~~~c++
  pf(1,2);
  (*pf)(1,2);
  ~~~

* 重载函数的指针

* **函数指针形参**

  ~~~c++
  void f2(int c, int pf(int a, int b));
  void f2(int c, int (*pf)(int a, int b));
  ~~~

* **返回函数指针**

  ~~~c++
  using F = int(int, int);
  using PF = int(*)(int,int);
  PF t1 = f;
  F* t1 = f;
  //确定函数类型时可以使用auto/decltype
  ~~~



## 1.6 类

### 1.6.1 基础

* 成员变量
* `mutable`可变数据成员，`const`成员函数也可以对其修改
* 成员函数，类内定义（隐式inline）,类外定义
* 类型成员，类内自定义别名

* `this`指针，常量指针，保存这个对象的地址，无法修改

* `const`成员函数

  ~~~c++
  //给this增加底层const，表明此函数不会修改对象
  Student::prtName()const{
      return this->m_name;
  }
  ~~~

* 返回`*this`对象



### 1.6.2 访问控制

* `struct` 默认`public`

* `class`默认`private`

* `friend` 使其它函数或类访问`private`成员

  ~~~c++
  friend ostream &print(ostream&,Student&);//函数做友元
  friend class Teacher;//类做友元
  //令其它类的成员函数为友元
  class Student;
  struct Teacher{
      void printStu(Student a);
  };
  class Student{
      friend void Teacher::printStu(Student);
      int m_id;
  };
  void Teacher::printStu(Student a){
      cout << a.m_id;
  }
  ~~~
  



### 1.6.3 构造函数

* 构造函数

  ~~~c++
  Student::Student() = default;//可以不定义，没初始值的成员变量默认初始化
  Student::Student(int id):m_id(id){}//初始化顺序与定义顺序相同
  ~~~

* 委托构造函数

  ~~~c++
  Student::Student(int id,int age):m_id(id),m_age(age){}
  Student::Student():Student(-1,0){}
  Student::Student(int id):Student(id,0){}
  ~~~

* 类类型转换

  ~~~c++
  //构造函数提供了从其他类型向这个类型转换的方式
  Student::f1(Student s1);
  Student s1;
  s1.f1(1);//int会被隐式地转换为Student
  ~~~

* `explicit`

  ~~~c++
  explicit Student::Student(int id):Student(id,0){}//抑制隐式转换
  s1.f1(static_cast<Student>(1));//仍然可以显示转换
  ~~~

* 聚合类（没有成员函数，所有成员`public`）

  ~~~c++
  struct Stu{
      int m_id;
      string m_name;
  };
  //通过参数列表初始化，顺序同定义顺序
  Stu s1={2021,"zhangSan"};
  ~~~

* 字面值常量类



### 1.6.3 静态成员

* 声明与定义

  类内声明需要加`static`，类外定义不需要加。

* 使用

  ~~~c++
  struct Stu{
      static m_id;
      static printMid(){
          cout << m_id;
      }
  };
  Stu::printMid()//使用作用域运算符进行访问
  //通过对象或对象指针进行访问
  ~~~

* 初始化

  ~~~c++
  static constexpr int m_id = 1;//类内初始化，只能constexpr
  ~~~

  

# 2 标准库

## 2.1 IO库

### 2.1.1 IO类
* 
  | 头文件     | 类型            |
  | ---------- | --------------- |
  | `iostream` | `istream`       |
  | `iostream` | `ostream`       |
  | `iostream` | `iostream`      |
  | `fstream`  | `ifstream`      |
  | `fstream`  | `ofstream`      |
  | `fstream`  | `fstream`       |
  | `sstream`  | `istringstream` |
  | `sstream`  | `ostringstream` |
  | `sstream`  | `stringstream`  |

* IO对象无法拷贝或赋值

* 刷新输出缓冲区 `endl` `ends` `flush` `unitbuf` `nounitbuf`

  ~~~c++
  cout << unitbuf;//后续所有操作都会立即刷新缓冲区
  cout << nounitbuf;//恢复正常模式
  ~~~



### 2.1.2 文件输入输出 

* `fstream`
  ~~~c++
  fstream fstrm;
  fstream fstrm(s);
  fstream fstrm(s,mode);
  fstrm.open(s);
  fstrm.close();
  fstrm.is_open();
  ~~~

* `ifstream` 输入

* `ofstream` 输出

* `mode`

  | `mode`         | 作用                   | 备注 |
  | ------------ | ------------------------ | ------------ |
  | `in`         | 读方式                   |  |
  | `out`    | 写方式               | 丢弃原始数据 |
  | `app`   | 每次写定位至末尾     | 与`out`共用避免丢失原数据 |
  | `ate`    | 打开文件后定位至末尾 |  |
  | `trunc`  | 截断文件             |  |
  | `binary` | 二进制方式           |  |
  
  ~~~c++
  ofstream app("file", ofstream::out|ofstream::app);
  ~~~



### 2.1.3 string流

* `stringstream`

  ~~~c++
  stringstream strm;
  stringstream strm(s);//保存s的拷贝
  strm.str();//返回strm中string的拷贝
  strm.strm(s);//拷贝s至strm中
  ~~~



## 2.2 顺序容器 

### 2.2.1 概述

* | 容器           | 备注                   |
  | -------------- | ---------------------- |
  | `vector`       |                        |
  | `deque`        | 双端队列               |
  | `list`         | 双向链表               |
  | `forward_list` | 单向链表               |
  | `array`        | 固定数组，无法添加删除 |
  | `string`       |                        |

* | 类型              | 备注        |
  | ----------------- | ----------- |
  | `iterator`        | 迭代器      |
  | `const_iterator`  | 底层`const` |
  | `size_type`       | 无符号      |
  | `differnece_typr` | 有符号      |
  | `value_type`      |             |
  | `reference`       |             |
  | `const_reference` |             |

* | 构造函数     | 备注                          |
  | ------------ | ----------------------------- |
  | `C c`        | 默认构造                      |
  | `C c1(c2)`   | 拷贝构造                      |
  | `C c(b,e)`   | 拷贝迭代器范围（array不支持） |
  | `C c{a,b,c}` | 列表初始化                    |
  | `C seq(n)`   | `string`不支持                |
  | `C seq(n,t)` |                               |

* | 赋值和`swap`      | 备注               |
  | ----------------- | ------------------ |
  | `c1 = c2`         | 赋值               |
  | `c1 = {a,b,c}`    | 赋值               |
  | `a.swap(b)`       |                    |
  | `swap(a,b)`       |                    |
  | `seq.assign(b,e)` | 替换               |
  | `seq.ssign(il)`   | 替换为`il`元素列表 |
  | `seq.assign(n,t)` |                    |

* | 大小           | 备注                    |
  | -------------- | ----------------------- |
  | `c.size()`     |                         |
  | `c.max_size()` | `c`中可保存的最大元素数 |
  | `c.empty()`    |                         |

* | 反向容器                  | 备注    |
  | ------------------------- | ------- |
  | `reverse_iterator`        |         |
  | `const_reverse_iterator`  |         |
  | `c.rbegin()` `c.rend()`   |         |
  | `c.crbegin()` `c.crend()` | `const` |



### 2.2.2 添加元素

* | 方法                    | 备注                     |
  | ----------------------- | ------------------------ |
  | `c.push_back(t)`        | `forward_list`不支持     |
  | `c.emplace_back(args)`  | 原地调用构造函数         |
  | `c.push_front(t)`       | `vector` `string` 不支持 |
  | `c.emplace_front(args)` |                          |
  | `c.insert(p,t)`         |                          |
  | `c.emplace(p,args)`     |                          |
  | `c.insert(p,n,t)`       |                          |
  | `c.insert(p,b,e)`       | `b` `e`不能是`c`的迭代器 |
  | `c.insert(p,il)`        | `il`是元素列表           |



### 2.2.3 访问元素

* | 方法        | 备注         |
  | ----------- | ------------ |
  | `c.back()`  | 需要判空     |
  | `c.front()` | 需要判空     |
  | `c[n]`      | 需要检查越界 |
  | `c.at(n)`   | 越界抛出异常 |



### 2.2.4 删除元素

* | 方法            | 备注                    |
  | --------------- | ----------------------- |
  | `c.pop_back()`  |                         |
  | `c.pop_front()` | `vector` `string`不支持 |
  | `c.erase(p)`    |                         |
  | `c.erase(b,e)`  |                         |
  | `c.clear()`     |                         |
  | `c.resize(n)`   | 多余被删除，不够则添加  |
  | `c.resize(n,t)` |                         |

* **添加或删除元素后`end`迭代器会失效**



### 2.2.5 容器大小管理

* | 方法                | 备注                     |
  | ------------------- | ------------------------ |
  | `c.shrink_to_fit()` | `capacity`缩小至`size()` |
  | `c.capacity()`      |                          |
  | `c.reserve(n)`      | 至少分配n个元素的空间    |



### 2.2.6 string

* | 构造方法                 | 备注                |
  | ------------------------ | ------------------- |
  | `string s(cp,n)`         | 数组`cp`的前n个字符 |
  | `string s(s2,pos2)`      |                     |
  | `string s(s2,pos2,len2)` |                     |

* 子串 `s.substr(pos,n)`

* | 修改方法                | 备注                           |
  | ----------------------- | ------------------------------ |
  | `s.insert(pos,args)`    | `pos`可以是下标或迭代器        |
  | `s.erase(pos,len)`      | `len`省略则删光                |
  | `s.assign(args)`        | 替换                           |
  | `s.append(args)`        |                                |
  | `s.replace(range,args)` | `range`为`pos`和`len`或`b` `e` |

* | 搜索方法                    | 备注                         |
  | --------------------------- | ---------------------------- |
  | `s.find(args)`              | 第一次出现位置               |
  | `s.rfind(args)`             | 最后一次出现位置             |
  | `s.find_first_of(args)`     | 任意字符第一次出现位置       |
  | `s.find_last_of(args)`      | 任意字符最后出现位置         |
  | `s.find_first_not_of(args)` | 第一个不在`args`的字符位置   |
  | `s.find_last_not_of(args)`  | 最后一个不在`args`的字符位置 |

* | 数值转换         | 备注                                         |
  | ---------------- | -------------------------------------------- |
  | `to_string(val)` |                                              |
  | `stoi(s,p,b)`    | b基数默认为10, p `size_t`类型, 起始位置默认0 |
  | `stol(s,p,b)`    |                                              |
  | `stoul(s,p,b)`   |                                              |
  | `stoll(s,p,b)`   |                                              |
  | `stof(s,p)`      |                                              |
  | `stod(s,p)`      |                                              |
  | `stold(s,p)`     |                                              |

  ~~~c++
  double x = stod(s.substr(s.find_first_of("+-.0123456789")))
  ~~~

### 2.2.7 容器适配器

* `stack`

  ~~~c++
  s.pop();
  s.push(item);
  s.emplace(args)
  s.top()
  ~~~

* `queue`

  ~~~c++
  q.pop();
  q.front();
  q.back();
  q.push(item);
  q.emplace(args);
  ~~~

* `priority_queue`

  ~~~c++
  q.top();
  ~~~



## 2.3 泛型算法

