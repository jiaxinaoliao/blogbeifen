---
title: C++STL总结
tags: OI相关
categories: C++ OI
abbrlink: 15915
date: 2026-05-10 19:00:47
---

# C++ STL 总结-基于算法竞赛

本文介绍常用STL知识，注重应用，强调用法，不强调原理和繁杂的记忆。看过之后请多运用，多敲代码试。

> 费尽心思重新梳理了一下，注意了些美观性，修改了部分错误，添加了部分解释，编写过程非常难。

编译时建议添加编译参数 `-std=c++11`（C++11 即可，C++17 或 C++20 更好）。

> 使DEV支持C++20 ： https://blog.csdn.net/qq_50285142/article/details/122930647


## 1 vector

### 1.1 介绍

#### 1.1.1 简介

`vector`为可变长数组（动态数组），定义的`vector`数组可以随时添加数值和删除元素。

> 注意：**在局部区域中（比如局部函数里面）开vector数组，是在堆空间里面开的。**
>
> 在局部区域开数组是在栈空间开的，而栈空间比较小，如果开了非常长的数组就会发生爆栈。
>
> 故局部区域**不可以**开大长度数组，但是可以开大长度`vector`。

包含头文件：

```cpp
#include <vector>
```

#### 1.1.2 初始化

- 一维初始化：

```cpp
vector<int> a;            // 定义了一个名为 a 的一维数组，存储 int 类型数据
vector<double> b;         // 定义了一个名为 b 的一维数组，存储 double 类型数据
vector<node> c;           // 定义了一个名为 c 的一维数组，存储结构体类型数据
```

指定**长度**和**初始值**的初始化：

```cpp
vector<int> v(n);         // 定义一个长度为 n 的数组，初始值默认为 0，下标范围 [0, n-1]
vector<int> v(n, 1);      // 所有元素初始值均为 1
```

初始化中有多个元素：

```cpp
vector<int> a{1, 2, 3, 4, 5};   // 数组 a 中有五个元素，长度就为 5
```

拷贝初始化：

```cpp
vector<int> a(n + 1, 0);
vector<int> b(a);          // 拷贝初始化，a 和 b 完全相同
vector<int> c = a;         // 也是拷贝初始化
```

- 二维初始化：

第一维固定长度，第二维可变：

```cpp
vector<int> v[5];          // 行数固定为 5，列可变
// v[1].push_back(2); 等操作
```

行列均可变：

```cpp
vector<vector<int>> v;     // 定义行和列均可变的二维数组
```

行列长度均固定，初始值为 0：

```cpp
vector<vector<int>> a(n + 1, vector<int>(m + 1, 0));
```

C++17 及以上支持自动推导模板参数：

```cpp
vector a{1, 2, 3, 4, 5};               // 自动推导为 vector<int>
vector b(n + 1, vector(m + 1, 0));     // 自动推导为 vector<vector<int>>
```


### 1.2 方法函数

#### 1.2.1 函数总结

（`c` 为数组名称）

| 代码                   | 算法复杂度 | 返回值类型 | 含义                                                         |
| :--------------------- | :--------- | :--------- | :----------------------------------------------------------- |
| `c.front()`            | O(1)       | 引用       | 返回容器中的第一个数据                                       |
| `c.back()`             | O(1)       | 引用       | 返回容器中的最后一个数据                                     |
| `c.at(idx)`            | O(1)       | 引用       | 返回 `c[idx]`，会进行边界检查，越界抛异常，比 `[]` 更安全    |
| `c.size()`             | O(1)       | size_t     | 返回实际数据个数（unsigned 类型）                            |
| `c.begin()`            | O(1)       | 迭代器     | 返回首元素的迭代器                                           |
| `c.end()`              | O(1)       | 迭代器     | 返回最后一个元素**后一个位置**的迭代器                       |
| `c.empty()`            | O(1)       | bool       | 判断是否为空，空返回 true                                    |
| `c.reserve(sz)`        | O(N)       | void       | 提前分配 `sz` 的内存，改变 `capacity`，避免多次拷贝          |
| `c.assign(beg, end)`   | O(N)       | void       | 将另一个容器 `[beg, end)` 里的内容拷贝到 `c` 中              |
| `c.assign(n, val)`     | O(N)       | void       | 将 `n` 个 `val` 拷贝到 `c` 中（清除原内容）                  |
| `c.pop_back()`         | O(1)       | void       | 删除最后一个数据                                             |
| `c.push_back(element)` | O(1)       | void       | 在尾部添加一个数据                                           |
| `c.emplace_back(ele)`  | O(1)       | void       | 在尾部添加一个数据，效率通常比 `push_back` 高，支持传多个构造参数 |
| `c.clear()`            | O(N)       | void       | 清除容器中的所有元素                                         |
| `c.resize(n, v)`       | O(N)       | void       | 改变数组大小为 `n`，新增部分赋值为 `v`（若省略 `v` 则默认 0） |
| `c.insert(pos, x)`     | O(N)       | 迭代器     | 在迭代器 `pos` 处插入元素 `x`                                |
| `c.erase(first, last)` | O(N)       | 迭代器     | 删除 `[first, last)` 的所有元素                              |

#### 1.2.2 注意情况

- `end()` 返回的是最后一个元素的后一个位置的地址，**所有 STL 容器均是如此**。
- 使用 `resize(n, v)` 时，若原大小 `pre > n`，则保留前 `n` 个元素（原值）；若 `pre < n`，则新增 `n-pre` 个值为 `v` 的元素。
- 使用 `sort` 排序要：`sort(c.begin(), c.end());`
  对指定区间排序可对迭代器进行加减操作：`sort(a.begin() + 1, a.end());`

### 1.3 元素访问

#### 1.3.1 下标访问

```cpp
for (int i = 0; i < vi.size(); ++i) cout << vi[i] << " ";
```

#### 1.3.2 迭代器访问

```cpp
vector<int>::iterator it;
for (it = vi.begin(); it != vi.end(); ++it) cout << *it << " ";
```

#### 1.3.3 范围 for 循环 (C++11)

```cpp
for (auto &x : v) cin >> x;    // 输入（注意引用）
for (auto val : v) cout << val << " ";
```


------

## 2 stack

### 2.1 介绍

栈 —— 先进后出，后进先出。

```cpp
#include <stack>
stack<int> s;
```

### 2.2 方法函数

| 代码          | 含义                     | 复杂度 |
| :------------ | :----------------------- | :----- |
| `s.push(ele)` | 元素 `ele` 入栈          | O(1)   |
| `s.pop()`     | 移除栈顶元素             | O(1)   |
| `s.top()`     | 取得栈顶元素（不删除）   | O(1)   |
| `s.empty()`   | 检测栈内是否为空，空为真 | O(1)   |
| `s.size()`    | 返回栈内元素的个数       | O(1)   |

### 2.3 栈遍历

栈不能直接遍历，只能不断弹出：

```cpp
while (!st.empty()) {
    int tp = st.top(); st.pop();
}
```

**数组模拟栈**（速度更快，便于遍历）：

```cpp
int s[100];      // 栈数组，从左至右为栈底到栈顶
int tt = -1;     // 栈顶指针
s[++tt] = 1;     // 入栈
int top = s[tt--]; // 出栈
```

------

## 3 queue

### 3.1 介绍

队列 —— 先进先出。

```cpp
#include <queue>
queue<int> q;
```

### 3.2 方法函数

| 代码              | 含义                 | 复杂度 |
| :---------------- | :------------------- | :----- |
| `q.front()`       | 返回队首元素         | O(1)   |
| `q.back()`        | 返回队尾元素         | O(1)   |
| `q.push(element)` | 尾部添加元素         | O(1)   |
| `q.pop()`         | 删除队首元素         | O(1)   |
| `q.size()`        | 返回元素个数         | O(1)   |
| `q.empty()`       | 判断是否为空，空为真 | O(1)   |

### 3.3 数组模拟队列

```cpp
int q[N];
int hh = 0, tt = -1;   // hh 队首下标，tt 队尾下标
q[++tt] = 1;           // 入队
int front = q[hh++];   // 出队
```

------


## 4 deque

### 4.1 介绍

双端队列 —— 首尾都可插入和删除。

```cpp
#include <deque>
deque<int> dq;
```

### 4.2 方法函数

| 代码                                   | 含义                          | 复杂度 |
| :------------------------------------- | :---------------------------- | :----- |
| `push_back(x)` / `push_front(x)`       | 在队尾 / 队首插入 x           | O(1)   |
| `back()` / `front()`                   | 返回队尾 / 队首元素           | O(1)   |
| `pop_back()` / `pop_front()`           | 删除队尾 / 队首元素           | O(1)   |
| `erase(iterator it)`                   | 删除迭代器指向的元素          | O(N)   |
| `erase(iterator first, iterator last)` | 删除 `[first, last)` 中的元素 | O(N)   |
| `empty()`                              | 判断是否为空                  | O(1)   |
| `size()`                               | 返回元素个数                  | O(1)   |
| `clear()`                              | 清空所有元素                  | O(N)   |

**注意**：双端队列常数较大。

排序：

```cpp
sort(dq.begin(), dq.end());                     // 升序
sort(dq.begin(), dq.end(), greater<int>());     // 降序
```

------

## 5 priority_queue

### 5.1 介绍

优先队列 —— 每次取出的元素都是优先级最大的（底层是堆）。

```cpp
#include <queue>
priority_queue<int> q;   // 默认大根堆（最大元素在堆顶）
```

### 5.2 函数方法

| 代码        | 含义         | 复杂度   |
| :---------- | :----------- | :------- |
| `q.top()`   | 访问堆顶元素 | O(1)     |
| `q.push(x)` | 插入元素     | O(log N) |
| `q.pop()`   | 弹出堆顶元素 | O(log N) |
| `q.size()`  | 元素个数     | O(1)     |
| `q.empty()` | 是否为空     | O(1)     |

**注意**：没有 `clear()`，只能逐个弹出。

### 5.3 设置优先级

#### 5.3.1 基本数据类型

```cpp
priority_queue<int> q1;                     // 大根堆
priority_queue<int, vector<int>, less<int>> q2;   // 大根堆（与上相同）
priority_queue<int, vector<int>, greater<int>> q3; // 小根堆
```

#### 5.3.2 结构体类型

**方法一**：在结构体内重载 `<` 运算符

```cpp
struct Point {
    int x, y;
    bool operator < (const Point &a) const {
        return x < a.x;   // 大根堆，按 x 降序（x 大的在堆顶）
    }
};
priority_queue<Point> q;
```

**方法二**：使用自定义比较结构体

```cpp
struct cmp {
    bool operator()(const Point &a, const Point &b) {
        return a.x > b.x;   // 小根堆，按 x 升序
    }
};
priority_queue<Point, vector<Point>, cmp> q;
```

#### 5.3.3 存储 pair 类型

默认先按 `first` 降序，再按 `second` 降序。

------



## 6 map

### 6.1 介绍

映射 —— 每个键对应一个值（键值对），键唯一且自动按键排序。

```cpp
#include <map>
map<string, int> mp;
```

### 6.2 方法函数

| 代码                    | 含义                                         | 复杂度     |
| :---------------------- | :------------------------------------------- | :--------- |
| `mp.find(key)`          | 返回键为 key 的迭代器，不存在返回 `mp.end()` | O(log N)   |
| `mp.erase(it)`          | 删除迭代器对应的键值对                       | O(log N)   |
| `mp.erase(key)`         | 根据键删除                                   | O(log N)   |
| `mp.erase(first,last)`  | 删除区间 `[first, last)` 中的元素            | O(K log N) |
| `mp.size()`             | 返回键值对个数                               | O(1)       |
| `mp.clear()`            | 清空                                         | O(N)       |
| `mp.insert({key, val})` | 插入元素                                     | O(log N)   |
| `mp.empty()`            | 是否为空                                     | O(1)       |
| `mp.count(key)`         | 存在返回 1，不存在返回 0（因为键唯一）       | O(log N)   |
| `mp.lower_bound(key)`   | 返回第一个键 >= key 的迭代器                 | O(log N)   |
| `mp.upper_bound(key)`   | 返回第一个键 > key 的迭代器                  | O(log N)   |

**重要提醒**：使用 `mp[key]` 访问不存在的键时会自动创建该键值对（值为默认构造），**不建议用于查询**。推荐先 `find` 或 `count` 判断。

### 6.3 遍历

```cpp
for (auto it = mp.begin(); it != mp.end(); ++it)
    cout << it->first << " " << it->second << "\n";

for (auto &p : mp)
    cout << p.first << " " << p.second << "\n";
```

### 6.4 unordered_map

- 内部是哈希表，元素无序，查找平均 O(1)，但常数较大。
- 需要自定义哈希函数（如对 `pair`）。

------

## 7 set

### 7.1 介绍

`set` 容器中的元素**不重复且自动排序**（默认升序）。

```cpp
#include <set>
set<int> s;
```

### 7.2 方法函数

| 代码                    | 复杂度   | 含义                             |
| :---------------------- | :------- | :------------------------------- |
| `s.begin()` / `s.end()` | O(1)     | 迭代器                           |
| `s.insert(x)`           | O(log N) | 插入元素，已存在则忽略           |
| `s.erase(x)`            | O(log N) | 删除值为 x 的元素                |
| `s.erase(it)`           | O(log N) | 删除迭代器指向的元素             |
| `s.find(x)`             | O(log N) | 返回迭代器，不存在返回 `s.end()` |
| `s.count(x)`            | O(log N) | 存在返回 1，不存在返回 0         |
| `s.lower_bound(x)`      | O(log N) | 返回第一个 >= x 的迭代器         |
| `s.upper_bound(x)`      | O(log N) | 返回第一个 > x 的迭代器          |
| `s.size()`              | O(1)     | 元素个数                         |
| `s.empty()`             | O(1)     | 是否为空                         |
| `s.clear()`             | O(N)     | 清空                             |

### 7.3 自定义排序

```cpp
set<int, greater<int>> s2;            // 降序

struct cmp {
    bool operator()(int a, int b) const { return a > b; }
};
set<int, cmp> s3;

// C++11 匿名函数
auto cmp = [](int a, int b) { return a > b; };
set<int, decltype(cmp)> s4(cmp);
```

### 7.4 multiset 和 unordered_set

- `multiset`：允许重复元素，其他同 `set`。删除时注意 `erase(val)` 会删除所有等于 `val` 的元素，删除单个应 `erase(find(val))`。
- `unordered_set`：哈希实现，无序，不可重复。
- `unordered_multiset`：哈希实现，无序，可重复。

------



## 8 pair

### 8.1 介绍

`pair` 将两个值组合成一个单元。

```cpp
#include <utility>
pair<string, int> p("abc", 123);
p = make_pair("def", 456);
p = {"ghi", 789};
```

### 8.2 访问

```cpp
cout << p.first << " " << p.second << "\n";
```

------

## 9 string

### 9.1 介绍

C++ 字符串类。

```cpp
#include <string>
string s;
```

### 9.2 常用操作

- 输入输出：`cin >> s` （遇空白停止）；`getline(cin, s)` （读一行）
- 拼接：`s1 + s2`
- 比较：支持 `<`、`>`、`==` 等（字典序）
- 长度：`s.size()` 或 `s.length()`
- 遍历：`for (char c : s)`
- 子串：`s.substr(pos, len)`
- 查找：`s.find(t)` 返回位置，未找到返回 `string::npos`
- 插入删除：插入`s.insert(pos, "abc")`  删除`s.erase(pos, len)`
- 转换大小写：

```cpp
transform(s.begin(), s.end(), s.begin(), ::tolower); // 需 <cctype>
transform(s.begin(), s.end(), s.begin(), ::toupper);
```

- 数字转换：

```cpp
int x = stoi(s);       // string -> int
long long y = stoll(s);
string s2 = to_string(x);
```

------

## 10 bitset

### 10.1 介绍

固定长度的二进制位集合，每位占 1 bit。

```cpp
#include <bitset>
bitset<100> b;   // 长度为 100，初始全 0
```

### 10.2 常用操作

```cpp
bitset<8> b("1100");       // 从字符串初始化
b[0] = 1;                  // 最低位（最右边）
b.set();                   // 全部置 1
b.reset();                 // 全部置 0
b.flip();                  // 按位取反
bool any = b.any();        // 是否存在 1
size_t cnt = b.count();    // 1 的个数
string str = b.to_string();
unsigned long val = b.to_ulong();
```

### 10.3 位运算

支持 `&`、`|`、`^`、`~`、`<<`、`>>` 等运算符。

------

## 11 array

### 11.1 介绍

固定大小数组，比普通数组更安全，提供了 STL 接口。

```cpp
#include <array>
array<int, 5> arr = {1,2,3,4,5};
```

### 11.2 常用方法

```cpp
arr.size();          // 返回大小（固定）
arr.at(2);           // 带边界检查的访问
arr.fill(0);         // 全部填充为 0
sort(arr.begin(), arr.end());
```

------

## 12 tuple

### 12.1 介绍

`tuple` 是 `pair` 的泛化，可以存储任意数量、任意类型的元素。

```cpp
#include <tuple>
tuple<int, double, string> t(1, 2.5, "hello");
```

### 12.2 访问

```cpp
int i = get<0>(t);
double d = get<1>(t);
string s = get<2>(t);
```

### 12.3 解包

```cpp
int a; double b; string c;
tie(a, b, c) = t;
```

------



### STL 常用算法

### accumulate

```cpp
#include <numeric>
int sum = accumulate(a.begin(), a.end(), 0);          // 求和
int sum2 = accumulate(a.begin(), a.end(), 1, multiplies<int>()); // 求积
```

### fill

```cpp
fill(a.begin(), a.end(), 0);   // 全部赋值为 0
```

### iota

```cpp
iota(a.begin(), a.end(), 0);   // 填充 0,1,2,...
```

### lower_bound / upper_bound

```cpp
auto it = lower_bound(a.begin(), a.end(), x);   // 第一个 >= x
auto it2 = upper_bound(a.begin(), a.end(), x);  // 第一个 > x
```

### max_element / min_element

```cpp
int mx = *max_element(a.begin(), a.end());
int mn = *min_element(a.begin(), a.end());
```

### nth_element

```cpp
nth_element(a.begin(), a.begin() + k, a.end());  // 第 k 小放在 a[k] 位置
```

### next_permutation

```cpp
do { ... } while (next_permutation(a.begin(), a.end()));
```

### reverse

```cpp
reverse(a.begin(), a.end());
```

### sort / stable_sort

```cpp
sort(a.begin(), a.end());
sort(a.begin(), a.end(), greater<int>());   // 降序
stable_sort(a.begin(), a.end());            // 稳定排序
```

### unique

```cpp
auto it = unique(a.begin(), a.end());   // 将重复元素移到末尾，返回新逻辑末尾
a.erase(it, a.end());                   // 实际删除重复元素
```

### 集合运算（需有序）

```cpp
set_union(a.begin(), a.end(), b.begin(), b.end(), back_inserter(c));
set_intersection(...);
set_difference(...);
```

### 常用内置函数 (GCC)

- `__gcd(a, b)`：最大公约数
- `__lg(x)`：floor(log2(x))
- `__builtin_popcount(x)`：二进制中 1 的个数
- `__builtin_clz(x)`：前导 0 个数
- `__builtin_ctz(x)`：末尾 0 个数
- `__builtin_ffs(x)`：最后一个 1 的位置（从 1 开始）

### C++20 ranges（部分）

```cpp
#include <ranges>
#include <algorithm>
ranges::sort(a);                       // 等价于 sort(a.begin(), a.end())
ranges::reverse(a);
int mx = *ranges::max_element(a);
```


------

### 结束语

以上是算法竞赛中常用的 STL 容器和函数总结。理解并熟练运用这些工具，能极大提高编码效率。
**多练习，多敲代码，才是掌握的关键。**
