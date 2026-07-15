---
title: C++语法进阶
tags: OI相关
categories: C++ OI 教程
abbrlink: 65489
date: 2026-05-16 10:37:16
---


# c++语法进阶：进阶篇

## 第1章 二维数组以及多维数组

数组都是线性存储的，但我们可以用“数组的数组”来表示多维结构。最常用的是**二维数组**，它就像一个表格或矩阵。

### 1.1 二维数组的定义

```cpp
// 定义在全局区（自动初始化为0）
int a[110][110];   // 110 行，110 列，下标从 0 到 109

// 定义在局部时，需要手动初始化
int b[5][5] = {0};
```



### 1.2 二维数组的使用

通过 `[行下标][列下标]` 访问每个元素，下标从 0 开始。

```cpp
#include <iostream>
using namespace std;

int main() {
    int a[3][4];   // 3行4列
    
    // 赋值
    a[0][0] = 1;
    a[1][2] = 5;
    
    // 输入
    cin >> a[2][3];
    
    // 输出
    cout << a[1][2] << endl;
    
    return 0;
}
```



### 1.3 二维数组的遍历

使用嵌套 `for` 循环。

```c++
int n = 3, m = 4;
int a[110][110];

// 读入一个 n 行 m 列的矩阵
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        cin >> a[i][j];
    }
}

// 输出矩阵
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        cout << a[i][j] << " ";
    }
    cout << endl;
}
```



### 1.4 二维数组的初始化

```c++
// 完全初始化
int a[2][3] = { {1,2,3}, {4,5,6} };

// 省略行数（必须指定列数）
int b[][3] = {1,2,3,4,5,6,7,8,9};  // 自动推导为 3 行

// 部分初始化（其余元素为 0）
int c[2][3] = { {1,2} };   // 第一行: 1,2,0；第二行: 0,0,0

// 全部初始化为 0 的简便写法
int d[10][10] = {0};
```



### 1.5 常见操作

#### 矩阵加法

```c++
int a[110][110], b[110][110], c[110][110];
int n, m;
cin >> n >> m;
for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
        cin >> a[i][j];
for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
        cin >> b[i][j];
for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
        c[i][j] = a[i][j] + b[i][j];
```



#### 蛇形填数（经典练习）

```cpp
// 从 (0,0) 开始向右，遇到边界或已填数则转向（下、左、上）循环
int a[20][20] = {0};
int n;
cin >> n;
int x = 0, y = 0, tot = 1;
a[x][y] = tot++;
while (tot <= n * n) {
    while (y + 1 < n && !a[x][y+1]) a[x][++y] = tot++;
    while (x + 1 < n && !a[x+1][y]) a[++x][y] = tot++;
    while (y - 1 >= 0 && !a[x][y-1]) a[x][--y] = tot++;
    while (x - 1 >= 0 && !a[x-1][y]) a[--x][y] = tot++;
}
```



### 1.6 多维数组简介

三维数组可看作“立方体”：`int a[10][10][10];`，访问时使用三个下标。一般 OI 中不超过三维。

### 1.7 练习题

- [矩阵交换行](http://ybt.ssoier.cn:8088/problem_show.php?pid=1121)
- [矩阵加法](http://ybt.ssoier.cn:8088/problem_show.php?pid=1124)
- [蛇形填数](https://www.luogu.com.cn/problem/P5731)




## 第2章 字符数组与字符串（C风格）

### 一、字符

字符串是由字符构成的，所以我们先学习字符。

#### 1.1 字符基础

- 字符用**单引号**括起来，例如 `'a'`、`'1'`。
- 类型：`char`，占用 1 字节。
- ASCII 码表中共 128 个字符（0~127）。

| 字符 | ASCII 值 |
| :--- | :------- |
| `'0'` | 48       |
| `'a'` | 97       |
| `'A'` | 65       |
| `' '`（空格） | 32       |
| `'\0'`（空字符） | 0        |

#### 1.2 字符的输入输出与类型转换

```cpp
#include <iostream>
using namespace std;

int main() {
    char a = 'a';
    char b = 65;            // 直接用 ASCII 码赋值
    char c = 'x';
    int  d = 'c';           // 字符提升为 int，d = 99

    cout << a << endl;      // 输出 a
    cout << b << endl;      // 输出 A
    cout << (int)c << endl; // 输出 120（'x' 的 ASCII 码）
    cout << d << endl;      // 输出 99
    return 0;
}
```



#### 1.3 字符判断与转换

##### 手工判断（利用 ASCII 码范围）


```cpp
#include <iostream>
using namespace std;

int main() {
    char a;
    cin >> a;

    // 判断小写字母（'a' ~ 'z' 对应 97 ~ 122）
    if (a >= 'a' && a <= 'z') {
        cout << "小写字母" << endl;
    }
    // 判断大写字母（'A' ~ 'Z' 对应 65 ~ 90）
    if (a >= 'A' && a <= 'Z') {
        cout << "大写字母" << endl;
    }
    // 判断数字字符
    if (a >= '0' && a <= '9') {
        cout << "数字字符" << endl;
    }

    // 大小写转换（利用差 32）
    a = 'a';
    a -= 32;                // 小写转大写
    cout << a << endl;      // 输出 'A'

    a = 'C';
    a += 32;                // 大写转小写
    cout << a << endl;      // 输出 'c'

    return 0;
}
```



> **注意**：小写字母的 ASCII 比对应大写字母大 32。

##### 使用 `<cctype>` 头文件（推荐）

`<cctype>` 提供了一系列方便的函数，返回值是 `int`，非零表示真，零表示假。

| 函数         | 作用                           |
| :----------- | :----------------------------- |
| `isalnum(c)` | 是否是字母或数字               |
| `isalpha(c)` | 是否是字母                     |
| `isdigit(c)` | 是否是数字                     |
| `islower(c)` | 是否是小写字母                 |
| `isupper(c)` | 是否是大写字母                 |
| `tolower(c)` | 转换为小写（若不能转换则不变） |
| `toupper(c)` | 转换为大写                     |



```cpp
#include <iostream>
#include <cctype>
using namespace std;

int main() {
    char a;
    cin >> a;

    cout << isalnum(a) << endl;   // 非零表示是数字或字母
    cout << isalpha(a) << endl;   // 非零表示是字母
    cout << isdigit(a) << endl;   // 非零表示是数字
    cout << islower(a) << endl;   // 非零表示是小写字母
    cout << isupper(a) << endl;   // 非零表示是大写字母

    // 转换函数返回转换后的字符（可赋值）
    cout << (char)tolower(a) << endl; // 需要强制转为 char，否则输出 ASCII 码
    cout << (char)toupper(a) << endl;

    return 0;
}
```




### 二、C 风格字符串（字符数组）

C++ 继承了 C 语言的字符串表示法：**以 `'\0'` 结尾的字符数组**。
（下一章将学习更安全的 `string` 类）

#### 2.1 字符数组的定义

```cpp
char s[110];      // 可以存放最长 109 个有效字符 + 1 个结束符 '\0'
char str[20];
```



#### 2.2 字符数组的初始化

```cpp
char s1[] = "hello";        // 自动添加 '\0'，长度为 6（5 个字符 + '\0'）
char s2[10] = "world";      // 前 5 个字符 + '\0'，其余位置自动补 '\0'
char s3[5] = {'a','b','c'}; // 未指定的元素被初始化为 '\0'
```

> **重要**：字符串结束标志 `'\0'` 必不可少。很多字符串函数（如 `strlen`）依赖它来确定字符串的结尾。如果缺少 `'\0'`，这些函数会一直读取直到遇到内存中的随机 `0`，造成未定义行为。



#### 2.3 字符数组的输入输出

```cpp
char s[100];

// 1. cin >> s：遇到空格或换行停止，不会读取空白字符
cin >> s;
cout << s << endl;

// 2. cin.getline(s, 100)：读取整行（包含空格），最多读入 99 个字符 + '\0'
cin.getline(s, 100);

// 3. getchar()：读取一个字符（包括换行符）
char ch = getchar();

// 4. cin.get(s, 100)：类似 getline，但不丢弃换行符，常与 cin.get() 配合使用
cin.get(s, 100);
```

**访问字符数组中的元素**（下标从 0 开始）：

```cpp
cout << s[0] << endl;   // 第一个字符
cout << s[3] << endl;   // 第四个字符
```

#### ⚠️ `cin >>` 与 `getline` 混用问题

当 `cin >>` 读取后，换行符会留在输入缓冲区中，导致接下来的 `getline` 直接读到空串。解决方案：

```cpp
cin >> n;                    // 读取一个整数（或字符串）
cin.ignore();                // 忽略一个字符（通常是换行符）
cin.getline(s, 100);         // 正常读取整行
```



#### 2.4 常用 C 风格字符串函数（头文件 `<cstring>`）

| 函数                 | 作用                                                         | 示例                                       |
| :------------------- | :----------------------------------------------------------- | :----------------------------------------- |
| `strlen(s)`          | 返回字符串长度（不包含 `'\0'`）                              | `int len = strlen(s);`                     |
| `strcpy(s1, s2)`     | 将 `s2` 复制到 `s1`（包括 `'\0'`），需保证 `s1` 空间足够     | `strcpy(a, b);`                            |
| `strncpy(s1, s2, n)` | 最多复制 `n` 个字符到 `s1`，不自动添加 `'\0'`（谨慎使用）    | `strncpy(a, b, 5); a[5] = '\0';`           |
| `strcat(s1, s2)`     | 将 `s2` 连接到 `s1` 末尾，需保证 `s1` 空间足够               | `strcat(a, b);`                            |
| `strncat(s1, s2, n)` | 最多连接 `n` 个字符到 `s1` 末尾，会自动添加 `'\0'`           | `strncat(a, b, 3);`                        |
| `strcmp(s1, s2)`     | 字典序比较：`s1 < s2` 返回负数，相等返回 `0`，`s1 > s2` 返回正数 | `if (strcmp(a, b) == 0) { ... }`           |
| `strchr(s, c)`       | 在 `s` 中查找字符 `c` 第一次出现的位置，返回指针（地址）     | `char *p = strchr(s, 'a'); if (p) { ... }` |
| `strstr(s1, s2)`     | 在 `s1` 中查找子串 `s2` 第一次出现的位置，返回指针           | `char *p = strstr(s, "abc");`              |

**示例**：

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    char a[100] = "hello";
    char b[100] = "world";

    cout << strlen(a) << endl;   // 5

    strcat(a, b);                // a 变为 "helloworld"
    cout << a << endl;           // helloworld

    cout << strcmp(a, b) << endl; // 正数（因为 "helloworld" > "world"）

    // 查找字符
    if (strchr(a, 'o')) {
        cout << "找到了 o" << endl;
    }
    return 0;
}
```



#### 2.5 字符数组的遍历

使用 `for` 循环，以 `'\0'` 作为结束条件：

```cpp
char s[100] = "hello";
for (int i = 0; s[i] != '\0'; i++) {
    cout << s[i] << ' ';
}
// 输出：h e l l o
```

也可以使用 `while`：

```cpp
int i = 0;
while (s[i] != '\0') {
    cout << s[i];
    i++;
}
```



#### 2.6 常见错误与注意事项

1. **忘记 `'\0'`**：手动构造字符数组时，一定要在末尾添加 `'\0'`，否则字符串函数会越界。

   ```cpp
   char bad[] = {'a','b','c'};        // 没有 '\0'，危险！
   char good[] = {'a','b','c','\0'};  // 正确
   ```

2. **数组越界**：使用 `strcpy`、`strcat` 时要保证目标数组足够大，否则会覆盖其他数据。

3. **`cin >>` 不能读入空格**：需要读整行时用 `cin.getline`。

4. **`strncpy` 不自动添加 `'\0'`**：使用后建议手动加上 `'\0'`。

5. **比较字符串不能用 `==`**：`if (s1 == s2)` 比较的是两个字符数组的首地址，而不是内容。必须使用 `strcmp`。



#### 2.7 练习题

- [统计数字字符个数](http://ybt.ssoier.cn:8088/problem_show.php?pid=1129)
- [找第一个只出现一次的字符](http://ybt.ssoier.cn:8088/problem_show.php?pid=1130)



------

## 第3章 string 类

C++ 标准库中的 `string` 类提供了更安全、更方便的字符串操作，需要头文件 `<string>`。

### 3.1 定义与初始化

```cpp
#include <string>
using namespace std;

string s1;              // 空字符串
string s2 = "hello";
string s3("world");
string s4 = s2;
```



### 3.2 输入输出

```cpp
string s;
cin >> s;               // 读入一个单词（空格/换行分隔）
cout << s << endl;

// 读入一整行（包含空格）
getline(cin, s);
```



### 3.3 常用操作

| 操作                     | 示例                                           |
| :----------------------- | :--------------------------------------------- |
| 长度                     | `int len = s.length();` 或 `s.size()`          |
| 拼接                     | `s = s + "abc";` 或 `s += "abc";`              |
| 比较（字典序）           | `if (s1 == s2)`，支持 `> < >= <= !=`           |
| 访问单个字符（类似数组） | `char ch = s[0];` `s[0] = 'A';`                |
| 获取 C 风格字符串        | `const char* p = s.c_str();`                   |
| 查找子串                 | `int pos = s.find("abc");` 返回下标 否则返回-1 |
| 截取子串                 | `string sub = s.substr(pos, len);`             |
| 插入/删除                | `s.insert(pos, "abc");` `s.erase(pos, len);`   |

find 没有找到 特殊标记 string::npos   可以使用 -1 代替

**示例**：

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string a = "apple", b = "banana";
    cout << (a < b) << endl;   // 1 (apple < banana)
    string c = a + b;
    cout << c << endl;         // applebanana
    cout << c.find("ban") << endl; // 5
    string d = c.substr(0, 5);
    cout << d << endl;         // apple
    return 0;
}
```



### 3.4 遍历 string

```cpp
// 下标遍历
for (int i = 0; i < s.size(); i++) {
    cout << s[i];
}

// 范围 for 循环 (C++11)
for (char ch : s) {
    cout << ch;
}
```



### 3.5 练习题

- [P5733 自动修正](https://www.luogu.com.cn/problem/P5733)
- [P5734 文字处理软件](https://www.luogu.com.cn/problem/P5734)

------

## 第4章 函数

函数是将一段代码封装起来，实现代码复用和模块化。

### 4.1 函数定义与调用

```cpp
返回值类型 函数名(参数列表) {
    函数体
    return 返回值;
}
```

**示例**：

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) {
    return a + b;
}

int main() {
    int x = 3, y = 5;
    int z = add(x, y);
    cout << z << endl;   // 8
    return 0;
}
```



### 4.2 函数声明

如果函数的定义写在调用之后，需要先声明。

```cpp
int add(int a, int b);   // 声明

int main() {
    cout << add(2,3) << endl;
}

int add(int a, int b) {  // 定义
    return a + b;
}
```



### 4.3 参数传递

- **值传递**：形参是实参的拷贝，函数内修改不影响实参。
- **引用传递**：形参是实参的别名，修改会影响实参（使用 `&`）。

```cpp
void swap1(int a, int b) {       // 值传递，无效
    int t = a; a = b; b = t;
}
void swap2(int &a, int &b) {     // 引用传递，有效
    int t = a; a = b; b = t;
}
int main() {
    int x=1, y=2;
    swap1(x,y);  // x,y 不变
    swap2(x,y);  // x=2, y=1
}
```



### 4.4 默认参数

```cpp
void print(int n, int base = 10) {   // base 默认10
    // ...
}
print(100);     // base=10
print(100, 8);  // base=8
```



### 4.5 函数重载

函数名相同，参数列表不同（类型、个数、顺序）。

```cpp
int max(int a, int b) { return a>b?a:b; }
double max(double a, double b) { return a>b?a:b; }
```



### 4.6 递归

函数调用自身。必须有一个**终止条件**，否则无限递归。

**阶乘示例**：

```cpp
int fact(int n) {
    if (n == 0 || n == 1) return 1;
    return n * fact(n - 1);
}
```

**斐波那契数列**：

```cpp
int fib(int n) {
    if (n == 1 || n == 2) return 1;
    return fib(n-1) + fib(n-2);
}
```



递归效率通常较低，但实现简单，常用于 DFS。

### 4.7 练习题

- [P5735 距离函数](https://www.luogu.com.cn/problem/P5735)
- [P5736 质数筛](https://www.luogu.com.cn/problem/P5736)
- [P5737 闰年展示](https://www.luogu.com.cn/problem/P5737)

------

## 第5章 结构体

结构体可以将不同数据类型的变量组合成一个新的数据类型。

### 5.1 定义与使用

```cpp
struct Student {
    string name;
    int age;
    double score;
};

int main() {
    Student stu1;
    stu1.name = "Tom";
    stu1.age = 15;
    stu1.score = 98.5;
    
    Student stu2 = {"Jerry", 14, 95.0};  // 初始化列表
    
    cout << stu1.name << endl;
    return 0;
}
```



### 5.2 结构体数组

```cpp
Student a[100];
a[0].name = "Alice";
cin >> a[1].age;
```



### 5.3 结构体与函数

可以传递结构体（值传递或引用传递）。大型结构体建议使用引用避免拷贝。

```cpp
void print(const Student &s) {
    cout << s.name << " " << s.age << " " << s.score << endl;
}
```



### 5.4 结构体排序（结合 sort）

需要自定义比较函数或重载 `<` 运算符（将在排序章节演示）。

### 5.5 练习题

- [P5740 最厉害的学生](https://www.luogu.com.cn/problem/P5740)
- [P5741 旗鼓相当的对手](https://www.luogu.com.cn/problem/P5741)

------

## 第6章 基础排序

排序是 OI 中最基础的算法之一。本节介绍四种基础排序。

### 6.1 桶排序

适用于数据范围小且非负整数的场景。统计每个值出现的次数，然后按顺序输出。

```cpp
int bucket[10010] = {0};
int n, x;
cin >> n;
for (int i = 0; i < n; i++) {
    cin >> x;
    bucket[x]++;
}
for (int i = 0; i <= 10000; i++) {
    for (int j = 0; j < bucket[i]; j++) {
        cout << i << " ";
    }
}
```



### 6.2 冒泡排序

相邻元素比较，将大的向后“冒泡”，每轮确定一个最大元素。

```cpp
void bubbleSort(int a[], int n) {
    for (int i = 0; i < n-1; i++) {
        for (int j = 0; j < n-1-i; j++) {
            if (a[j] > a[j+1]) {
                swap(a[j], a[j+1]);
            }
        }
    }
}
```



### 6.3 选择排序

每轮选择剩余元素中的最小值，放到已排序末尾。

```cpp
void selectionSort(int a[], int n) {
    for (int i = 0; i < n-1; i++) {
        int minIdx = i;
        for (int j = i+1; j < n; j++) {
            if (a[j] < a[minIdx]) minIdx = j;
        }
        swap(a[i], a[minIdx]);
    }
}
```



### 6.4 插入排序

将当前元素插入到已排序序列的合适位置。

```cpp
void insertionSort(int a[], int n) {
    for (int i = 1; i < n; i++) {
        int key = a[i];
        int j = i-1;
        while (j >= 0 && a[j] > key) {
            a[j+1] = a[j];
            j--;
        }
        a[j+1] = key;
    }
}
```



### 6.5 使用 STL sort（推荐）

`#include <algorithm>` 中的 `sort` 函数，效率高且简单。

```cpp
int a[100];
int n;
cin >> n;
for (int i = 0; i < n; i++) cin >> a[i];
sort(a, a + n);                // 升序
sort(a, a + n, greater<int>()); // 降序

// 自定义排序规则（如按成绩降序，同成绩按姓名升序）
struct Student { string name; int score; };
bool cmp(const Student &a, const Student &b) {
    if (a.score != b.score) return a.score > b.score;
    return a.name < b.name;
}
sort(stu, stu + n, cmp);
```



### 6.6 练习题

- [P1177 快速排序](https://www.luogu.com.cn/problem/P1177)（用 sort 即可）
- [P1093 奖学金](https://www.luogu.com.cn/problem/P1093)（结构体排序）

------

## 第7章 指针基础

指针是存储变量地址的变量。OI 中指针使用较少，但理解指针有助于掌握动态内存和数据结构。

### 7.1 指针的定义与使用

```cpp
int a = 10;
int *p = &a;    // p 指向 a 的地址
cout << *p;     // 输出 10 (解引用)
*p = 20;        // 修改 a 的值
cout << a;      // 20
```



### 7.2 指针与数组

数组名就是首元素地址。

```cpp
int arr[5] = {1,2,3,4,5};
int *p = arr;   // 等价于 &arr[0]
cout << *(p+2); // 输出 3
```



### 7.3 动态内存分配

`new` 和 `delete` 在堆上分配内存。

```cpp
int *p = new int;   // 分配一个 int
*p = 100;
delete p;           // 释放

int *arr = new int[10];  // 分配数组
delete[] arr;
```



### 7.4 引用回顾（与指针对比）

引用是变量的别名，必须初始化且不能改变指向。

```cpp
int x = 5;
int &r = x;   // r 是 x 的引用
r = 10;       // x 变为 10
```



### 7.5 练习题

- [P5742 评级](https://www.luogu.com.cn/problem/P5742)（选做）

------

## 第8章 文件操作

OI 竞赛中常用文件输入输出，有两种方式：**freopen** 和 **fstream**。

### 8.1 使用 freopen（推荐 OI）

```cpp
#include <cstdio>
int main() {
    freopen("in.txt", "r", stdin);   // 从文件读入
    freopen("out.txt", "w", stdout); // 输出到文件
    // 正常的 cin/cout 或 scanf/printf
    int a, b;
    cin >> a >> b;
    cout << a + b << endl;
    fclose(stdin); fclose(stdout); // 可省略
    return 0;
}
```



### 8.2 使用 fstream

```cpp
#include <fstream>
using namespace std;
ifstream fin("in.txt");
ofstream fout("out.txt");
int a, b;
fin >> a >> b;
fout << a + b << endl;
fin.close();
fout.close();
```



### 8.3 练习题

- [P1001 A+B Problem](https://www.luogu.com.cn/problem/P1001)（尝试重定向到文件）

------

## 第9章 枚举类型

枚举用于定义一组命名的整数常量，提高代码可读性。

### 9.1 普通枚举

```cpp
enum Weekday { MON, TUE, WED, THU, FRI, SAT, SUN };
// MON = 0, TUE = 1, ... 

Weekday today = FRI;
if (today == FRI) cout << "TGIF" << endl;
```

可以手动赋值：

```cpp
enum Color { RED = 1, GREEN = 2, BLUE = 4 };
```



### 9.2 枚举与 switch 配合

```cpp
enum Op { ADD, SUB, MUL, DIV };
int main() {
    Op op = ADD;
    switch (op) {
        case ADD: cout << "+"; break;
        case SUB: cout << "-"; break;
        // ...
    }
}
```



### 9.3 强类型枚举（C++11）(可选)

使用 `enum class`，避免名称污染，且不能隐式转换为整型。

```cpp
enum class Color { Red, Green, Blue };
Color c = Color::Red;
// int x = c;  // 错误，不能隐式转换
int x = (int)c; // 强制转换
```



### 9.4 练习题

- 自定义枚举表示四季，并输出对应月份。
