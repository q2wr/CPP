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

* 尾置返回

  `auto (parameter list) -> return type { function body }`

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

一般通过两个迭代器对容器进行操作

### 2.3.1 概述

* `b` `e`开始结束迭代器

* | 函数                            | 备注                                                         |
  | ------------------------------- | ------------------------------------------------------------ |
  | `find(b,e,val)`                 | 返回第一个指向`val`的迭代器                                  |
  | `accumulate(b,e,s)`             | `s`为初始值                                                  |
  | `equal(b,e,b2)`                 | 比较两个容器                                                 |
  | `fill(b,e,val)`                 | 填充                                                         |
  | `fill_n(b,n,val)`               | `b`开始至少n个元素                                           |
  | `back_insert(vec)`              | `vec`的后插迭代器，与与其它算法使用可向尾部添加新元素，头文件为`iterator` |
  | `copy(b,e,b2)`                  | `b to e`拷贝至`b2`，请保证有足够空间                         |
  | `repalce(b,e,val,val2)`         | `val`替换为`val2`                                            |
  | `replace_copy(b,e,b2,val,val2)` | 原容器保持不变                                               |
  | `sort(b,e)`                     | 排序                                                         |
  | `unique(b,e)`                   | 使用前排序，返回不重复的下一个迭代器，可以将后面的元素删除   |



### 2.3.2 定制操作

* 向算法传递函数，谓词（可调用的表达式，返回结果作为条件值），一元与二元谓词

  ~~~c++
  bool cmp(const string &s1, const string &s2){
      return s1.size()<s.size();
  }
  sort(s1.begin(),s1.end(),cmp);//按长度排序
  ~~~

* `lambda` 可认为是未命名的内联函数，可定义在函数内部

  可调用对象：如果可以对其使用调用运算符`e(args)`，则是可调用的，函数、函数指针、重载了函数调用运算符的类、`lambda`表达式，可以向算法传递任何类别的可调用对象。

  `[capture list](parameter list) -> return type { function body }`

  参数列表和返回类型可以省略

  ~~~c++
  auto f = [] { return 42 }
  cout << f();
  ~~~

  ~~~c++
  sort(s.begin(),s.end(),[](const string &a,const string &b)
       {return a.size()<b.size();})//长度排序
  ~~~

  ~~~c++
  auto b = find_if(s.begin(),s.end(),[sz](const string &a)
          {return a.size()>=sz})//找到第一个长度大于等于sz
  ~~~

  ~~~c++
  for_each(b,s.end(),[](const string &s){cout << s << " ";})//打印
  ~~~

  | 捕获方式              | 备注                               |
  | --------------------- | ---------------------------------- |
  | `[]`                  | 空                                 |
  | `[names]`             | 显式值捕获，添加`&`为引用捕获      |
  | `[&]`                 | 隐式引用捕获                       |
  | `[=]`                 | 隐式值捕获                         |
  | `[&,identifier_list]` | 后面的用值捕获，其它为隐式引用捕获 |
  | `[=,identifier_list]` | 后面的用引用捕获，其它为隐式值捕获 |

* `lambda`返回类型

  包含`return`外其它语句将被认为返回`void`，此时返回类型不可省略。



### 2.3.3 参数绑定`bind`

* 头文件为`functional`，接受一个可调用对象，生成一个新的可调用对象

  ~~~c++
  auto newCallable = bind(callable, arg_list);
  ~~~

  ~~~c++
  bool checkSize(string s, int size){
      return s.size()>=size;
  }
  auto newCheckSize = bind(checkSize,_1,6);//_1占位，6是参数size
  auto b = find_if(s.begin(),s.end(),bind(checkSize,_1,sz))//将sz传入
  ~~~

* `placeholders`占位，定义在`std::placeholders`命名空间中

  ~~~c++
  f(a1,a2,a3,a4,a5);
  auto g = bind(f,a,b,_2,c,_1);
  g(_1,_2);
  f(a,b,_2,c,_1);//映射关系
  sort(s.begin(),s.end(),cmp);//顺序
  sort(s.begin(),s.end(),bind(cmp,_2,_1));//逆序
  ~~~

* 绑定引用参数

  `ref(val)`返回`val`的引用



### 2.3.4 迭代器

* | 插入迭代器             | 备注                               |
  | ---------------------- | ---------------------------------- |
  | `back_inserter(list)`  | 调用`push_back`                    |
  | `front_inserter(list)` | 调用`push_front`                   |
  | `inserter(list,iter)`  | 需要第二参数`iter`，插入`iter`之前 |

  `*it` `it++` `++it`不会对`it`产生任何影响

  使用`it = val`进行插入

* `istream_iterator`迭代器

  ~~~c++
  istream_iterator<int>int_it(cin);//指定元素类型，绑定流
  istream_iterator<int>int_eof;//默认初始化，作为尾后迭代器
  ifstream in("afile");
  istream_iterator<string> str_in(in)//从afile读string
  ~~~

  ~~~c++
  //两种读取方式
  while(int_it != int_eof){
      vec.push_back(*int_it++);
  }
  vector<int>vec2(int_it,int_eof);
  ~~~

* `ostream_iterator`迭代器

  ~~~c++
  ostream_iterator<int>out(cout);
  ostream_iterator<int>out(cout,d);输出后添加字符串d
  ~~~



## 2.4 关联容器

### 2.4.1 概述

* | 关键字有序 | 备注        |
  | ---------- | ----------- |
  | `map`      |             |
  | `set`      |             |
  | `multimap` | `key`可重复 |
  | `multiset` | `key`可重复 |
  
  | 关键字无序           | 备注        |
  | -------------------- | ----------- |
  | `unordered_map`      | `hash` 无序 |
  | `unordered_set`      | `hash` 无序 |
  | `unordered_multimap` | `key`可重复 |
  | `unordered_multiset` | `key`可重复 |

* 有序容器 可自定义`<`，需要满足**严格弱序**

  两个关键字不能同时“小于等于”对方;

  若a<=b,b<=c,则a<=c;

  如果两个关键字都不小于等于对方，则是等价的。

* `pair`

  | 操作                   | 备注                        |
  | ---------------------- | --------------------------- |
  | `pair<T1,T2>p`         | 初始化                      |
  | `pair<T1,T2>p(v1,v2)`  | 初始化                      |
  | `pair<T1,T2>p={v1,v2}` | 初始化                      |
  | `make_pair(v1,v2)`     | 初始化                      |
  | `p.first;p,second`     | 访问                        |
  | `< <= > >= == !=`      | 先判断`first`在判断`second` |



### 2.4.2 关联容器操作

* `set`通过迭代器无法修改，**只读**

* | 插入                | 备注                                    |
  | ------------------- | --------------------------------------- |
  | `c.insert(v)`       | 返回`pair<iterator,bool>`               |
  | `c.emplace(args)`   | `first`为插入位置, `second`表示是否成功 |
  | `c.insert(b,e)`     |                                         |
  | `c,insert(il)`      | 插入元素列表                            |
  | `c.insert(p,v)`     | `v`插到迭代器`p`处                      |
  | `c.emplace(p,args)` |                                         |

* | 删除           | 备注             |
  | -------------- | ---------------- |
  | `c.erase(k)`   | 返回删除的数量   |
  | `c.erase(p)`   | 返回下一个迭代器 |
  | `c.erase(b,e)` | 返回`e`          |

* | `map`下标操作 | 备注                                       |
  | ------------- | ------------------------------------------ |
  | `c[k]`        | 返回`k`对应`v`, `k`不存在会新建并初始化    |
  | `c.at(k)`     | 有参数检查，若没有，抛出`out_of_range`异常 |

* | 访问元素           | 备注                           |
  | ------------------ | ------------------------------ |
  | `c.find(k)`        | 没有返回`end`                  |
  | `c.count(k)`       |                                |
  | `c.lower_bound(k)` | 第一个不小于`k`                |
  | `c.upper_bound(k)` | 第一个大于`k`                  |
  | `c.equal_range(k)` | 返回`pair<b,e>`，没有均是`end` |



### 2.4.3 无序容器

* | 桶管理               | 备注            |
  | -------------------- | --------------- |
  | `c.bucker_count`     | 当前桶数量      |
  | `c.max_bucket_count` | 最多桶数        |
  | `c.bucket_size(n)`   | 第n个桶元素数量 |
  | `c.bucket(k)`        | 关键字`k`在的桶 |

* | 桶迭代                  | 备注           |
  | ----------------------- | -------------- |
  | `local_iterator`        | 元素迭代器类型 |
  | `const_local_iterator`  |                |
  | `c.begin(n),c.end(n)`   | 桶`n`的迭代器  |
  | `c.cbegin(n),c.cend(n)` |                |

* | 哈希策略              | 备注                    |
  | --------------------- | ----------------------- |
  | `c.load_factor()`     | 每个桶平均元素          |
  | `c.max_load_factor()` | `c`试图维护的平均桶大小 |
  | `c.rehash(n)`         | 重组存储                |
  | `c.reserve(n)`        |                         |

  

## 2.5 动态内存

### 2.5.1 动态内存和智能指针

* `shared_ptr` 头文件为`memory`

  | `shared_ptr` 和 `unique_ptr`都支持 | 备注              |
  | ---------------------------------- | ----------------- |
  | `shared_ptr<T>sp`                  |                   |
  | `unique_ptr<T>up`                  |                   |
  | `p`                                | 空指针返回`false` |
  | `*p`                               |                   |
  | `p->mem`                           |                   |
  | `p.get()`                          | 返回`p`中的指针   |
  | `swap(p,q)`                        |                   |
  | `p.swap(q)`                        |                   |

  | 仅`shared_ptr`支持     | 备注                         |
  | ---------------------- | ---------------------------- |
  | `make_shared<T>(args)` | 根据`args`初始化对象         |
  | `shared_ptr<T>p(q)`    | 拷贝，会增加`q`计数器        |
  | `p=q`                  | `p` 计数减少`q`计数增加      |
  | `p.unique()`           | 若`p.use_count()`为1返回true |
  | `p.use_count()`        | 速度慢                       |

  当最后一个`shared_ptr`被销毁时，其指向对象被销毁。

* `new` `delete`

  ~~~c++
  int *pi1 = new int;//默认初始化，*pi1未定义
  int *pi2 = new int();//值初始化，*pi2为0
  const int *pci = new const int;//const对象
  int *p1 = new int;//分配失败抛出std::bad_alloc
  int *p2 = new (nothrow) int;//分配失败返回空指针
  delete p2;
  ~~~

* `new` 和`shared_ptr`结合使用

  ~~~c++
  shared_ptr<int> p2(new int(42));
  shared_ptr<int> p1 = new int(1024);//错误，由于这里构造函数是explicit
  ~~~

* | 定义和改变             | 备注                           |
  | ---------------------- | ------------------------------ |
  | `shared_ptr<T>p(q)`    | `q`必须指向`new`分配的内存     |
  | `shared_ptr<T>p(u)`    | 从`unique_ptr`接管`u`，`u`置空 |
  | `shared_ptr<T>p(q,d)`  | `d`为可调用对象，替代`delete`  |
  | `shared_ptr<T>p(p2,d)` | `p`是`p2`的拷贝                |
  | `p.reset()`            | 若只有`p`指向，会释放          |
  | `p.reset(q)`           | `p`会指向`q`                   |
  | `p.reset(q,d)`         | 调用`d`来释放`q`               |

* 自定义释放操作（对于不是`new`分配的内存）

  ~~~c++
  void end_connection(connection *p){disconnect(*p);}
  void f(destination &d){
      connection c = connect(&d);
      shared_ptr<connection>p(&c, end_connection);
  }
  ~~~

* `unique_ptr` 只能直接初始化，不支持赋值和拷贝，销毁时，指向对象也被销毁

  | 操作                  | 备注                                 |
  | --------------------- | ------------------------------------ |
  | `unique_ptr<T>u1`     |                                      |
  | `unique_ptr<T,D>u2`   | 使用类型`D`的可调用对象代替`delete`  |
  | `unique_ptr<T,D>u(d)` | 用类型`D`的可调用对象`d`代替`delete` |
  | `u=nullptr`           | 释放，置空                           |
  | `u.release()`         | 放弃对指针的控制，返回指针，置空     |
  | `u.reset()`           | 释放所指对象                         |
  | `u.reset(q)`          | 指向`q`                              |
  | `u.reset(nullptr)`    |                                      |

* `weak_ptr`  不控制所指对象生存期，指向一个由`shared_ptr`管理的对象，不改变计数

  | 操作               | 备注                                               |
  | ------------------ | -------------------------------------------------- |
  | `weak_ptr<T>w`     |                                                    |
  | `weak_ptr<T>w(sp)` |                                                    |
  | `w=p`              | `p`可以是`sp`或`wp`，赋值即共享                    |
  | `w.reset()`        | 置空                                               |
  | `w.use_count()`    | `sp`数量                                           |
  | `w.expired()`      | `sp`数量为0返回`true`                              |
  | `w.lock()`         | `sp`数量为0返回空`sp`，否则返回指向`w`的对象的`sp` |

### 2.5.2 动态数组

* `new`和数组

  ~~~c++
  int *pia = new int[size];//指向第一个 int
  typedef int arrT[42];//arrT表示42个int的数组类型
  int *p = new arrT;
  int *pia2 = new int[10]{0,1,2}//列表初始化
  delete [] pia;//pia指向首元素
  unique_ptr<int[]>up(new int[10]);
  up.release();//自动使用delete
  up[i];//访问
  ~~~

* `allocator`

  | 操作                  | 备注                                                   |
  | --------------------- | ------------------------------------------------------ |
  | `allocator<T> a`      | 可为`T`分配内存                                        |
  | `a.allocate(n)`       | 分配`n`个未构造内存                                    |
  | `a.deallocat(p,n)`    | 在`p`出释放`n`个内存，和分配时保持一致，释放前需要析构 |
  | `a.construct(p,args)` | 在`p`处构造                                            |
  | `a.destroy(p)`        | 析构`p`所指对象                                        |

  | 拷贝和填充                     | 备注         |
  | ------------------------------ | ------------ |
  | `uninitialized_copy(b,e,b2)`   | 拷贝至`b2`处 |
  | `uninitialized_copy_n(b,n,b2)` | 拷贝n个      |
  | `uninitialized_fill(b,e,t)`    | 填充为`t`    |
  | `uninitialized_fill_n(b,n,t)`  |              |

  

# 3 类进阶

