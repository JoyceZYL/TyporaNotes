## Part Two STL 标准库

#### **STL**（Standard Template Library，**标准模板库**）

* 为了建立数据结构和算法的一套标准，诞生了**STL**
* 广义上分为：**容器(container)   算法(algorithm)   迭代器(iterator)**
* **容器**和**算法**之间通过**迭代器**进行无缝连接
* 几乎所有的代码都采用了模板类或者模板函数



#### STL六大组件

- 容器：数据结构，如 vector、list、map ，用来存放数据
- 算法：各种算法，如 sort、find、copy、for_each 
- 迭代器：容器与算法之间的胶合剂
- 仿函数：行为类似函数，可作为算法的某种策略
- 适配器：用来修饰容器、仿函数、迭代器的接口
- 空间配置器：负责空间的配置与管理



## Ⅴ 顺序容器



| 类别         | 特点                                                         |
| ------------ | ------------------------------------------------------------ |
| vector       | 可变大小数组，支持快速随机访问，在尾部之外的位置增减元素可能很慢 |
| deque        | 双端队列，支持快速随机访问，在头尾位置增删速度很快，中间位置增删元素的代价（可能）很高 |
| list         | 双向链表，只支持双向顺序访问，在任何位置增删都很快           |
| forward_list | 单向链表，只支持单向顺序访问，在任何位置增删都很快           |
| array        | 固定大小数组，支持快速随机访问，**不能添加或删除元素**       |
| string       | 与vector相似的容器，但用于保存字符，随机访问快，尾部增删快   |



### 1. 迭代器

- 容器和算法之间粘合剂
- 提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式

- 每个容器都有自己专属的迭代器



**迭代器种类**

| 种类           | 功能                               | 支持运算                        |
| -------------- | ---------------------------------- | ------------------------------- |
| 输入迭代器     | 只读访问                           | ++   ==   ! =                   |
| 输出迭代器     | 只写访问                           | ++                              |
| 前向迭代器     | 读写操作，能向前推进迭代器         | ++   ==   ! =                   |
| 双向迭代器     | 读写操作，能向前和向后推进         | ++   --                         |
| 随机访问迭代器 | 读写操作，以跳跃的方式访问任意数据 | ++   --     [n]   -n    <    <= |



**获取迭代器**

| 方式                 | 操作/意义                                      |
| -------------------- | ---------------------------------------------- |
| c.begin，c.end()     | 返回指向c首尾迭代器                            |
| c.cbegin()，c.cend() | 返回const_iterator                             |
| c.rbegin()，c.rend() | 反向迭代器reverse_iterator，即++则是上一个元素 |
| c.crbegin()          | 返回const_reverse_iterator                     |

- end表示的是指向最后一个元素的下一个位置的地址



**常用操作**

```
v.begin() == v.end();		相等，v为空向量
auto it = v.begin();		迭代器it指向第一个元素
*it;						返回迭代器it所指元素的引用
it -> mem;					等价于(*it).mem
++it;						迭代器指向下一个元素
--it;						迭代器指向上一个元素
it1 == it2;					若指向同一个元素，相等
v.cbegin();					只读
```



### 2. 一般顺序容器

#### 通用操作

##### 定义和初始化

~~~
C c;

C c1(c2); 					c1为 c2的拷贝

C c{a, b, c...};			初始化为列表中元素的拷贝
C c = {a, b, c...};

C c(n);						大小为 n
C c(n, t);					n个 t

C c(b, e);					迭代器 b和 e之间的元素拷贝
~~~

##### 赋值

```c++
c1 = c2;

c.assign(b, e);       		用迭代器b, e区间中的数据，拷贝赋值给c
c.assign(n, t);      		将n个t 拷贝赋值给c
```

##### 添加元素

~~~c++
c.push_back(t);       		尾部插入 t
c.push_front(t);	  		头部插入 t
    
c.insert(p, t);		  		迭代器 p前方插入 t
    				  		返回新添加的元素
    
c.insert(p, n, t);	  		迭代器 p前方插入 n个 t
    				  		返回新添加的元素，n为 0则返回 p
    
c.insert(p, b, e);    		迭代器 p前方插入 迭代器 b和 e范围内的元素，b和 e不能指向 c中元素
    				  		返回新添加的元素，范围为空则返回 p

c.insert(p, {..});	  		迭代器 p前方插入 给定系列值
    				  		返回新添加的元素，列表为空则返回 p

c.emplace_back(args);       尾部插入 由 args新创建的对象
c.emplace_front(args);	  	头部插入 由 args新创建的对象
    
c.emplace(p, args);		 	迭代器 p前方插入 由 args新创建的对象
    				     	返回新创建的对象
~~~

##### 访问元素

~~~c++
auto v = c.back();		    v是 c尾元素的拷贝
auto &v = c.front();		v是 c首元素的引用

c[n]						若下标越界, 函数行为未定义
c.at(n)						若下标越界, 抛出out_of_range异常
~~~

- 不要对空容器调用 front 和 back


##### 删除元素

~~~c++
c.pop_back();				删除 c尾元素
c.pop_front();				删除 c首元素

c.erase(p);					删除迭代器 p所指元素
							返回下一个元素的迭代器
							
c.erase(b, e);				删除迭代器 b，e范围内的元素
							返回下一个元素的迭代器

c.clear();					删除所有元素
~~~

- 删除deque中除首尾之外的任何元素，都会使所有迭代器、引用、指针失效
- vector和string中，指向删除点之后的所有迭代器、引用、指针失效

##### 容器大小

~~~c++
c.empty();              	容器为空返回1，不为空返回0
c.size();               	返回容器中元素的个数

c.resize(n, t=0);			调整c大小为n个元素
							如果容器变长，新添加的元素初始化为t

c1.swap(c2);				c1, c2互换容器		
~~~

**resize 函数**

- **重新分配**大小，改变容器的大小，并且创建对象
- n < size，size 减少到 n，超出 n 的元素删除，c.end() 重置到 n - 1 后方，不改变capacity
- n > size， 插入相应数量的元素，使得 size 值达到n，并对这些元素进行初始化
- n > capacity，会自动分配重新分配内存空间，重新分配的 capacity 不一定等于 n



#### vector

* vector数据结构和**数组非常相似**，也称为**单端数组**

* 不同之处在于数组是静态空间，而vector可以**动态扩展**

  * 找更大的内存空间，然后将原数据拷贝新空间，释放原空间
* 支持随机访问的迭代器

**特有操作**

- 不可在头部添加或删除元素（push_front,  pop_front）

```c++
c.capacity();           	返回容器的容量
c.swap(c);					若 size < capacity, 使 capacity = size
v.reserve(int len);			预留 len 个元素长度，不初始化，元素不可访问，c.end() = c.begin()
```



#### list

- 链式存储，物理存储单元上非连续，由一系列**结点**组成
- 结点：包含数据域，指针域
- STL中的链表是一个双向循环链表
- 迭代器只支持前移和后移，属于**双向迭代器**

**优点**

* 插入和删除不会造成迭代器失效
* 采用动态存储分配，不会造成内存浪费和溢出
* 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

**缺点**

* 但是空间（指针域）和 时间（遍历）额外耗费较大

- 不可通过 at 和 [ ] 访问元素



#### deque

- deque访问内部元素比 vector 慢

**内部工作原理**

- deque内部有一个**中控器**，数个**缓冲区**
- 中控器维护每段缓冲区中的地址，缓冲区中存放真实数据
- 中控器储存每个缓冲区的地址，使得使用deque时像一片连续的内存空间





#### stack

- 先进后出 (First In Last Out, FILO) 的数据结构，只有一个出口
- 只有顶端的元素才可以被外界接触，无迭代器
- 大部分通用操作不可用，可用如下：

```c++
C c;
C c1(c2); 					c1为 c2的拷贝
c1 = c2;
    
c.push(t);
c.pop();
c.top();
c.emplace();

c.empty();
c.size();
c.swap();
```



#### queue

- 先进先出 (First In First Out, FIFO) 的数据结构，有两个出口
- 从一端新增元素，从另一端移除元素，无迭代器
- 大部分通用操作不可用，可用如下：

```c++
C c;
C c1(c2); 					c1为 c2的拷贝
c1 = c2;

c.push(t);
c.pop();
c.back();
c.front();
c.emplace();

c.empty();
c.size();
c.swap();
```



### 3. String

* string 是C++风格的字符串
* string 本质上是一个类，内部封装了char\*，是一个char*型的容器

- `string 变量名 = "字符串值" ` 

#### 相关函数

vector 可用函数都可用

**构造函数**

```c++
string();
string(const char* s);
string(const string& str);		// 拷贝构造
string(int n, char c);			// 使用n个 字符c初始化
```

##### 赋值函数

```c++
string& operator=(const char* s);
string& operator=(const string &s);
string& operator=(char c);

string& assign(const char *s);
string& assign(const char *s, int n);		//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);
string& assign(int n, char c);				//用n个字符c赋给当前字符串

//例
string str;
str.assign("Hello, c++", 5);				//str 为 “Hello”
```

##### 相加

```c++
string s1 = "hello";
string s2 = "world";

string s = s1 + s2;				//√
string s = s1 + 'W';			//√，可以与char相加
string s = "Hello" + "world";	//×，不能把字面值直接相加
```

##### 拼接

- 把参数中字符串，拼接到当前字符串结尾

```c++
string& operator+=(const char* str);
string& operator+=(const char c);
string& operator+=(const string& str);

string& append(const char *s);
string& append(const char *s, int n);                   //连接字符串s的前n个字符
string& append(const string &s);
string& append(const string &s, int pos, int n);		//连接字符串s中从pos开始的n个字符
```

##### 比较

```
s1.compare(s2);					s1和s2比较
s1.compare(0, 2, s2, 3, 5);		s1前两位和s2三四位比较
```

- 若 s1 = s2，返回 0
- 若 s1 < s2，返回负值
- 若 s1 > s2，返回正值
- "ab" > "a" > "2" > "12"

##### 查找和替换

```c++
//查找第一次出现的位置
int find(const string& str, int pos = 0) const;
int find(const char* s, int pos = 0) const;
int find(const char* s, int pos, int n) const; 		      //查找s的前n个字符
int find(const char c, int pos = 0) const;

//查找最后一次出现的位置（从后向前查找）
int rfind(const string& str, int pos = npos) const;
int rfind(const char* s, int pos = npos) const;
int rfind(const char* s, int pos, int n) const; 		  //查找s的前n个字符
int rfind(const char c, int pos = npos) const;

//其他查找函数，每个函数的参数都可变为上面四种中，任意一种
int find_first_of(const string& str, int pos = 0);		  //查找参数中任何一个字符，第一次出现的位置
int find_last_of(const char* s, int pos = npos);	 	  //查找参数中任何一个字符，最后一次出现的位置
int find_first_not_of(const char* s, int pos, int n); 	  //查找第一个不在参数中的字符
int find_last_not_of(const char c, int pos = npos);		  //查找最后一个不在参数中的字符

//替换
string& replace(int pos, int n, const string& str);       //把从pos开始的n个字符，替换为str
string& replace(int pos, int n,const char* s);            //把从pos开始的n个字符，替换为str

//例
string str = "helloworld";
cout << str.rfind("l", 7);								  //3
```

- 查找函数返回找到的位置，找不到则返回 -1
- pos：开始查找的位置
- str、s、c：查找内容

##### 字符存取

```c++
char& operator[](int n);
char& at(int n);

//例
string str = "Smash";
str[3] = 'r';
str.at(4) = 't';
cout << str;				//Smart
```

##### 插入和删除

```c++
string& insert(int pos, const char* s);            //插入字符串
string& insert(int pos, const string& str);        //插入字符串
string& insert(int pos, int n, char c);            //在指定位置插入n个字符c
string& erase(int pos, int n = npos);              //删除从Pos开始的n个字符
```

##### 子串

```c++
string substr(int pos = 0, int n = npos) const;		//返回从pos开始，长度为n的子串
```

##### 标准库函数

```标准库函数 to_string()
to_string(123);					标准库函数，可以把所有数字转换为string
```



#### ASCII码

- 0-9 < A-Z < a-z
- 同字母的大写比小写小32
- “0” =  48；“A” = 65；“a” = 97



#### Char

- -127 ~ 128

- 用数字表示，可以直接当作数字参与运算

- char 转换成 string

  ```c++
  char a = '0';
  string s(1, a);		//s = "0"
  ```

  

#### C 风格字符串

- `char 变量名[] = "字符串值"`
- char* 本质上是一个指针



### 4. Array

- 存放相同对象的容器
- 大小固定，不能随意添加元素
- 不适用于上述顺序容器操作

#### 3.1 定义与初始化

**定义**

- `type arr[n]`
- n 是数组的维度，必须是常量表达式

**初始化**

```c++
int n = 3;
int a1[n] = {0, 1, 2};
int a2[]  = {0, 1, 2};
int a3[5] = {0, 1, 2};
string a4[] = {"0", "1", "2"}; 

int a[2] = {0, 1, 2};   //error!
```



**不允许赋值和拷贝**

```c++
int a5[] = a;	//error!
a5 = a;			//error!
```



**数组名**

- 查看数组所占内存空间

```c++
int arr[5] = {1, 2, 3, 4, 5};

cout << "整个数组所占内存空间为" << sizeof(arr);			// 20
cout << "每个元素所占内存空间为" << sizeof(arr[0]);		// 4
cout << "数组元素个数为" << sizeof(arr)/sizeof(arr[0]);   // 5
```

- 获取数组在内存中的首地址

```c++
cout << "数组首地址为" << (int)arr;
cout << "数组第一个元素地址为" << (int)&arr[0];
cout << "数组第二个元素地址为" << (int)&arr[1];
```



#### 3.2 二维数组

**定义及初始化**

- `int  arr[ 2 ][ 3 ]`
- `int  arr[ 2 ][ 3 ] = { {1, 2, 3}, {4, 5, 6} }`     **√ 推荐使用**
- `int  arr[ 2 ][ 3 ] = { 1, 2, 3, 4, 5, 6 }`
- `int  arr[   ][ m ] = { 1, 2, 3, 4, 5, 6 }`



**二维数组数组名**

- 查看二维数组所占内存空间
- 获取二维数组在内存中的首地址



#### 3.3 结构体数组

```c++
struct student{
    string name;
    int age;
    int score;
}

int main(){
    struct student arr[3] = 
    {   {"小红", 12, 50},
        {"小明", 12, 80},
        {"李雷", 13, 76} };
}
```



#### 3.4 指针与数组

**数组名**

- 数组名是常量指针，指向数组中第一个元素

  ```c++
  double arr[10];
  ```

- 以上声明中，`arr` 是指向 `arr[0]` 的指针



**指针与迭代器**

- 定义指针 p 为指向数组第一个元素的指针

  ```c++
  double *p = arr;
  ```

- 可用以下方法访问   `arr[1] == *(p + 1) == *(arr + 1)`

- p 可以作为迭代器使用

  - 如  `p++`



**数组指针**

- 指向数组的指针

- 语法：

  ```c++
  int a[5] = {1, 2, 3, 4, 5};
  int (*p)[5] = &a;		// p 指向 a
  
  // 输出 a 中每个数
  for(int i = 0; i < 5; ++i)
      cout << (*p)[i] << endl;
  ```

- 赋值语句，左边是 int *[5] 类型，右边是 int * 类型



**指针数组**

- 存储指针的数组

- 每个元素分别赋值

  ```c++
  int a[5] = {1, 2, 3, 4, 5};
  int *p[5];
  for(int i = 0; i < 5; ++i){
      p[i] = &a[i];
      cout << *p[i] << endl;
  }
  ```

- 在堆区开辟内存

  ```c++
  int* pr = new int(10);
  int** ppr = new int*[10];
  ppr[1] = pr;
  cout << *ppr[1];		// 10
  ```





## Ⅵ 泛型算法

- 大多定义在 `<algorithm>` 中，涉及到比较、 交换、查找、遍历操作、复制、修改等
- `<numeric>`定义了一组数值泛型算法，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类,用以声明函数对象
- 适用于顺序容器
- 算法不会改变底层容器的大小
  - 添加元素的算法，需要提前给目标容器开辟空间



### 1. 函数对象

- `#include<functional>`
- 函数对象的本质是类



#### 算数仿函数

- negate 是一元运算，其余是二元运算

```c++
T plus<T>            加
T minus<T>           减
T multiplies<T>      乘
T divides<T>         除
T modulus<T>         取模，余数
T negate<T>          取反
```

**例**

```c++
modulus<int> m;
cout << m(7, -3);	// 1
cout << m(-7, 3);	//-1

negate<int> n;
cout << n(-7);		//7
```



#### 关系仿函数

- 二元运算

```c++
bool equal_to<T>            等于
bool not_equal_to<T>        不等于
bool greater<T>             大于
bool greater_equal<T>       大于等于
bool less<T>                小于
bool less_equal<T>          小于等于
```

**例**

```c++
greater<int> g;
cout << g(10, 5);			// 1
```



#### 逻辑仿函数

- 二元运算

```c++
bool logical_and<T>      	逻辑与
bool logical_or<T>       	逻辑或
bool logical_not<T>      	逻辑非
```

**例**

```c++
logical_and<int> l;
cout << l(1, 0);			// 0
cout << l(-1, 1);			// 1
```



### 2. 常用算法

**普通函数与函数对象作为形参**

- 普通函数作为形参时，传递函数名或函数指针，不带括号

```c++
void print01(int val){
	cout << val << " ";
}
int main(){
	vector<int> v = {1, 2, 3, 4, 5};
	for_each(v.begin(), v.end(), print01);
}
```

- 函数对象作为形参时，可直接用类名带括号；也可先初始化实例，传递实例（不带括号）

```c++
struct print02{
	void operator()(int val){
		cout << val << " ";
	}
};
int main(){
	vector<int> v = {1, 2, 3, 4, 5};
	for_each(v.begin(), v.end(), print02());
    
    print02 p;
    for_each(v.begin(), v.end(), p);
}
```

- 以下参数中 fun 皆为函数与仿函数



#### 2.1 遍历算法

```c++
for_each(b, e, fun)				遍历容器，容器中每个元素作为形参，运行fun函数
transform(b, e, b2, fun)		搬运迭代器b，e之间的值，到迭代器b2之后
    							目标容器需先开辟空间
```

**例**  

```c++
int twice(int val){
    return val*2;
}

struct print{
	void operator()(int val){
		cout << val << " ";
	}
};

int main() {
    vector<int> v1 = {0, 1, 2, 3, 4};
    vector<int> v2;
    v2.resize(5);

	transform(v1.cbegin(), v1.cend(), v2.begin(), twice);
    for_each(v2.begin(), v2.end(), print());
}
```



#### 2.2 查找算法

```
find(b, e, val)					查找指定的元素，返回第一个等于给定值的迭代器，失败返回e
find_if(b, e, 谓词)			   返回符合条件的值的迭代器

binary_search(b, e, val)		查找指定的元素，查到则返回1，否则0
								高效，但只能查找有序序列

adjacent_find(b, e)				查找相邻重复元素,返回相邻元素的第一个位置的迭代器

count(b, e, val)				返回元素出现次数（统计自定义数据类型，需要重载 ==）
count_if(b, e, 谓词)			   返回满足条件元素出现次数			
```



#### 2.3 排序算法

- 无返回值

```c++
sort(b, e)	            		排序
reverse(b, e)       	 		反转
random_shuffle(b, e)			洗牌

merge(b1, e1, b2, e2, b3)       合并两个容器的元素，存到b3所在容器中
    							顺序容器可排序。两个指针分别指向两个容器头部，小的先转移
```

#### 2.4 拷贝和替换算法

- 无返回值

```c++
copy(b1, e1, b2)                容器内指定范围的元素拷贝到另一容器中
replace(b, e, val, new_val)     指定范围内，val替换为new_val
replace_if(b, e, 谓词, new_val)  指定范围内，满足条件的元素替换为new_val
swap(c1, c2)                    互换两个同种容器的元素
```



#### 2.5 算术生成算法

- 头文件 `<numeric>`

```c++
accumulate(b, e, sum)      		返回元素总和，起始值为sum

fill(b, e, val)                 指定范围内，元素填充为val，无返回值
fill_n(b, n, val)				b开始，n个值填充为val，无返回值
```

**例**

- 将 vector 中的 string 连接起来

```c++
string sum = accumulate(v.cbegin(), v.cend(), string(""));
```



#### 2.6 集合算法

- 返回第三个容器中，最后一个元素的位置

```c++
set_intersection(b1, e1, b2, e2, b3)      求交集，存储于第三个容器（size等于两个容器中较小值）

set_union(b1, e1, b2, e2, b3)             求并集，存储于第三个容器（size等于两个容器中之和）

set_difference(b1, e1, b2, e2, b3)        求差集，存储于第三个容器（size等于两个容器中较大值）
```



#### 2.7 比较算法

```c++
equal(b1, e1, b2)				两个序列相等则返回true
								v2不能短于v1
```

 



## Ⅶ 关联容器

- 按照关键字顺序来保存和访问
- map：元素包含 key - value
- set：每个元素包含一个关键字，支持高效查询
- 底层结构由二叉树实现

| 关联容器类型 |                    |                     | 头文件        |
| ------------ | ------------------ | ------------------- | ------------- |
| **有序容器** | map                | key - value         | map           |
|              | set                | 只有 key            | set           |
|              | multimap           | key 可重复出现的map | map           |
|              | multiset           | key 可重复出现的set | set           |
| **无序容器** | unordered_map      | 哈希map             | unordered_map |
|              | unordered_set      | 哈希set             | unordered_set |
|              | unordered_multimap | 哈希可重复map       | unordered_map |
|              | unordered_multiset | 哈希可重复set       | unordered_set |



#### pair类型

map中的元素为 pair类型

```c++
pair<T1, T2> p;					定义
pair<T1, T2> p(v1, v2);			定义和初始化
pair<T1, T2> p = {v1, v2};
p = make_pair(v1, v2);

p.first;						返回 p的 first成员
p.second;						返回 p的 second成员
    
p1 < p2;						p1.first < p2.first 
    							或 p1.first == p2.first && p1.second < p2.second

p1 == p2;						first和 second分别相等
```



**pair对象的函数**

```c++
pair<string, int> fun(){
    return {"abc", 5};			 //列表初始化
    return pair<string, int>();	 //隐式构造返回值
    return make_pair("abc", 5);
}
```



#### **额外类型别名**

|             | map返回               | set返回   |
| ----------- | --------------------- | --------- |
| key_type    | key                   | key       |
| value_type  | pair <const key, val> | const key |
| mapped_type | val                   | -         |

- value_type 为迭代器解引用返回类型，因此迭代器不能改变 key

**例**





### 关联容器操作

#### 定义和初始化

```c++
set<int> b = {1, 2, 3};
set<int> c(b);
set<int> d = b;

map<int, int> a = {{1, 5},
                   {2, 4}};

vector<int> v = {2，2, 1, 1, 0, 0};
set<int> iset(v.begin(), v.end());			set只包含 0，1，2
multiset<int> miset(v.begin(), v.end());	multiset包含全部重复元素
```



#### 添加元素

| <span style="display:inline-block;width:100px">函数</span> | 含义                                     | 返回值                                                       | 例子                                 |
| ---------------------------------------------------------- | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| **c.insert({})**                                           | value_type 值列表                        | void                                                         | c.instert ({2，1})                   |
| **c.insert(v)**                                            | v 代表value_type对象                     | pair {迭代器：指向给定元素，bool：插入是否成功}（multi只返回迭代器） | make_pair(2, 1) pair<int, int>(2, 1) |
| **c.insert(b, v)**                                         | 从b开始搜寻、插入 v                      | 迭代器，指向给定元素                                         |                                      |
| **c.insert(b, e)**                                         | b, e 为迭代器，包含value_type 类型的范围 | void                                                         |                                      |

**例**

- map记录单词出现次数

```c++
map<string, size_t> word_count;
string word;

while(cin >> word){
    auto ret = word_count.insert({word, 1});
    if(!ret.second)
        ++ret.first -> second;
}
```



#### 删除元素

| 函数              | 含义                               | 返回值               |
| ----------------- | ---------------------------------- | -------------------- |
| **c.erase(k)**    | 删除所有关键字为 k的元素           | size_type：删除数量  |
| **c.erase(b)**    | 删除迭代器 b指定元素，b != c.end() | 指向 p后元素的迭代器 |
| **c.erase(b, e)** | 删除迭代器 b，e之间元素            | 迭代器 e             |
| **c.clear()**     | 清除所有元素                       |                      |



#### 下标操作

|          | 含义                                                         |      |
| -------- | ------------------------------------------------------------ | ---- |
| c [k]    | 返回关键字为k的元素；若查找失败，添加关键字为 k的元素，并初始化 |      |
| c.at (k) | 访问关键字为k的元素；若查找失败，抛出一个out_of_range异常    |      |



#### 访问元素

- lower_bound 和 upper_bound不适用于无序容器

- 下标和 at 操作只适用于非const


| 函数              | 返回值                                    | 查找失败返回值       |
| ----------------- | ----------------------------------------- | -------------------- |
| c.find (k)        | 迭代器，指向第一个关键字为k的元素         | c.end ()             |
| c.count (k)       | 关键字个数                                | 0                    |
| c.lower_bound (k) | 迭代器，指向第一个关键字不小于 k的元素    |                      |
| c.upper_bound (k) | 迭代器，指向第一个关键字大于 k的元素      |                      |
| c.equal_range (k) | 迭代器 pair，包含关键字等于 k的元素的范围 | {c.end (), c.end ()} |



#### 容器大小与交换

| 函数         | 返回值                       | 作用             |
| ------------ | ---------------------------- | ---------------- |
| c.size ()    | 容器中元素的数目             |                  |
| c.empty ()   | 容器为空返回 1，不为空返回 0 |                  |
| c1.swap (c2) | -                            | 交换两个集合容器 |



#### 排序

- 自定义数据类型，必须指定排序规则
- 利用仿函数，改变排序规则
- 语法
  - `set<type, 函数对象>`
  -  `map<type1, type2, 函数对象>`

**例**

- 自定义数据类型排序，按年龄升序；年龄相同，则按身高降序

```c++
struct person{
    int age;
    float hight;
    person(int a, float h): age(a), hight(h){}

    friend ostream& operator<<(ostream& cout, const person& p){
        cout << p.age << " " << p.hight << " ";
        return cout;
    }
};

struct compare{
    bool operator()(const person& p1, const person& p2){
        if(p1.age != p2.age)
            return p1.age < p2.age;
        else
            return p1.hight > p2.hight;
    }    
};

int main(){
    person p1(3, 1.5);
    person p2(3, 1.8);
    person p3(38, 1.5);
    set<person, compare> s = {p1, p2, p3};
    cout << *s.begin() << *++s.begin() << *++(++s.begin());
}
```





# Ⅴ I/O

## iostream

#### cin

- 读取规定类型，到空格，tab (\t) ，或者换行 (\n) 截止。

```c++
string s;
int a;

cin >> s;				输入为 "aa b", 则s = "aa"
cin >> a;				输入为 "11 2", 则a = "11"
```

##### cin.get()

- 可读取空格，空格，tab (\t) ，或者换行 (\n) 
- 可用来暂停程序，单机回车继续

```c++
string s;

cin >> s;				输入为 "bb\na", 则s = "bb"
s = cin.get();			s = '\n'
cin.get();				暂停程序
```



#### getline

读取一行 (\n ) 截止的内容

```c++
string s;
getline(cin, s);		输入为"aa b\nc"，则s = "aa b"
```



## 文件输入输出

### 1. 基础知识

- 程序运行时产生的数据属于临时数据，运行结束会被释放
- 通过文件将数据持久化



#### 文件输入输出三大类

**储存在头文件 fstream 中**

- ofstream：写操作
- ifstream：读操作
- fsteam：读写操作



#### 文件打开方式

| 打开方式    | 解释                           |
| ----------- | ------------------------------ |
| ios::in     | 为**读**文件而打开文件         |
| ios::out    | 为**写**文件而打开文件         |
| ios::ate    | 初始位置：**文件尾**           |
| ios::app    | **追加**方式写文件             |
| ios::trunc  | 如果文件存在**先删除**，再创建 |
| ios::binary | 二进制方式                     |

**注意：** 文件打开方式可以配合使用，利用|操作符

**例如：**用二进制方式写文件 `ios::binary |  ios:: out`



### 2. 文本文件

- 以文本的 ASCII 码存储在计算机中



#### 2.1 写文件

**写文件步骤**

- 创建流对象  				ofstream ofs;

- 打开文件                      ofs.open ( "路径" ,   打开方式 ) ;

- 写数据                          ofs << "写入的数据";

- 关闭文件                      ofs.close();


```c++
int main()
{
	ofstream ofs;
	ofs.open("test.txt", ios::out);
	ofs << "Hello, file world!" << endl;
	ofs.close();
}
```

- 写文件可以利用 ofstream  ，或者fstream类



#### 2.2 读文件

**读文件步骤**

- 创建流对象  				ifstream ifs;
- 打开文件                      ifs. open ( "路径" ,   打开方式 ) ;
- 判断是否打开成功       ifs. is_open()
- 判断文件是否为空       ifs. eof ()
- 读数据                          四种读取方式
- 关闭文件                      ifs.close();

```c++
int main()
{
    //打开文件
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	
    //判断是否打开成功
	if (!ifs.is_open()){
		cout << "文件打开失败" << endl;
    
    //判断是否为空
    if (ifs.eof()){
		cout << "文件为空！" << endl;
    
    //读取数据，方式一
    char buf[1024] = { 0 };
	while (ifs >> buf)
		cout << buf << endl;
    
	//方式二
	while (ifs.getline(buf,sizeof(buf)))
		cout << buf << endl;
    
    //方式三
    string sbuf;
    while (getline(ifs, sbuf))
		cout << sbuf << endl;
    
    //方式四，一个一个字符读取，不推荐
    //EOF：end of file，文件尾
    char c;
	while ((c = ifs.get()) != EOF)
		cout << c;
    
    ifs.close();
}
```

- 读文件可以用 ifstream，或者 fstream 类



### 3. 二进制文件

**二进制文件的优点**

- 节约空间。储存字符型数据时没有差别。但是在储存数字，特别是实型数字时，二进制更节省空间。比如储存 Real*4  的数据：3.1415927，文本文件需要 9 个字节，分别储存：3 . 1 4 1 5 9 2 7 这 9 个 ASCII  值，而二进制文件只需要 4 个字节（DB 0F 49  40）
- 内存中参加计算的数据，是用二进制无格式储存。储存为文本文件，则需要一个转换的过程。因此，使用二进制储存到文件更快捷。如果数据量大时，两者会有明显的速度差别。　　
- 就是一些比较精确的数据，使用二进制储存不会造成有效位的丢失。



#### 3.1 写文件

- 利用流对象，调用成员函数 write

- 函数 ：`ostream& write(const char * buffer, int len);`

- 参数：字符指针 buffer 指向内存中一段存储空间，len是读写的字节数

```c++
class Person
{
public:
	char m_Name[64];
	int m_Age;
};

//二进制文件  写文件
int main(){
    Person p = {"张三"  , 18};
    
	ofstream ofs("person.txt", ios::out | ios::binary);
	ofs.write((const char *)&p, sizeof(p));

	ofs.close();
}
```



#### 5.2.2 读文件

- 利用流对象，调用成员函数 read

- 函数原型：`istream& read(char *buffer,int len);`

- 参数解释：字符指针 buffer 指向内存中一段存储空间，len是读写的字节数


```C++
#include <fstream>
#include <string>

class Person
{
public:
	char m_Name[64];
	int m_Age;
};

int main(){
	ifstream ifs("person.txt", ios::in | ios::binary);
    if (!ifs.is_open())
		cout << "文件打开失败" << endl;

	Person p;
	ifs.read((char *)&p, sizeof(p));

	cout << "姓名： " << p.m_Name << " 年龄： " << p.m_Age << endl;
}
```

- 文件输入流对象 可以通过read函数，以二进制方式读数据



