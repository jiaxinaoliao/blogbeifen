---
title: C++算法入门
tags: OI相关
categories: C++ OI 教程
abbrlink: 49328
date: 2026-05-16 13:05:07
---



# c++算法入门：基础篇

## 第1章 枚举算法

枚举，说白了就是 **把所有可能的情况都试一遍**。你想啊，如果一个问题可能的答案只有有限的几种，那最直接的办法不就是挨个儿试吗？暴力出奇迹嘛！

### 1.1 什么时候用枚举？

- 问题的可能情况是 **有限的**
- 数据范围 **比较小**（一般在几万到几十万以内）
- 实在想不出更好的算法了（保底方案）

### 1.2 例题1：求1~100中所有能被3整除的数

这题太简单了，直接遍历1到100，判断每个数能不能被3整除就行。

~~~c++
#include <iostream>
using namespace std;

int main()
{
    for (int i = 1; i <= 100; i++)
    {
        if (i % 3 == 0)
        {
            cout << i << " ";
        }
    }
    return 0;
}
~~~

运行结果会输出：3 6 9 12 ... 99。这就是最基础的枚举——**穷举所有可能，然后筛选出符合条件的**。

### 1.3 例题2：百钱买百鸡（经典枚举题）

这是中国古代数学家张丘建在《算经》里提出的问题：
> 公鸡5文钱一只，母鸡3文钱一只，小鸡3只1文钱。用100文钱买100只鸡，问公鸡、母鸡、小鸡各多少只？

我们设公鸡x只，母鸡y只，小鸡z只，那么：
- x + y + z = 100（总数量）
- 5x + 3y + z/3 = 100（总价钱，注意z必须是3的倍数）

最暴力的做法：x从0到100枚举，y从0到100枚举，z = 100 - x - y，然后检查价格条件。

~~~c++
#include <iostream>
using namespace std;

int main()
{
    for (int x = 0; x <= 100; x++)
    {
        for (int y = 0; y <= 100; y++)
        {
            int z = 100 - x - y;
            if (z >= 0 && z % 3 == 0 && 5 * x + 3 * y + z / 3 == 100)
            {
                cout << "公鸡:" << x << " 母鸡:" << y << " 小鸡:" << z << endl;
            }
        }
    }
    return 0;
}
~~~

**优化思路**：其实不需要枚举到100。公鸡5文一只，最多20只；母鸡3文一只，最多33只。这样能大大减少枚举次数：

~~~c++
#include <iostream>
using namespace std;

int main()
{
    for (int x = 0; x <= 20; x++)
    {
        for (int y = 0; y <= 33; y++)
        {
            int z = 100 - x - y;
            if (z >= 0 && z % 3 == 0 && 5 * x + 3 * y + z / 3 == 100)
            {
                cout << "公鸡:" << x << " 母鸡:" << y << " 小鸡:" << z << endl;
            }
        }
    }
    return 0;
}
~~~

### 1.4 枚举的要点总结

1. **确定枚举范围**——能缩多小缩多小
2. **确定枚举条件**——要检查哪些约束
3. **注意边界**——小心数组越界、整数溢出

**练习题**：
- 洛谷 P1008 [三连击](https://www.luogu.com.cn/problem/P1008)
- 洛谷 P1157 [组合的输出](https://www.luogu.com.cn/problem/P1157)
- 洛谷 P1618 [三连击（升级版）](https://www.luogu.com.cn/problem/P1618)

## 第2章 模拟算法

模拟，就是 **按照题目描述的过程，一步步用代码实现出来**。题目怎么说，你就怎么写，不需要想什么高深的数学公式，老老实实按规则来就行。

很多竞赛题看起来复杂，其实就是让你模拟一个过程——比如模拟排队、模拟游戏规则、模拟物理过程等等。

### 2.1 例题：开关灯问题

> 有n盏灯，编号1~n。第一个人把所有灯都打开（操作开关）；第二个人按下编号为2的倍数的开关；第三个人按下编号为3的倍数的开关……以此类推，一共k个人。问最后哪些灯是亮着的？

这道题就是典型的模拟——我们用一个数组表示灯的状态，然后模拟每个人的操作。

~~~c++
#include <iostream>
using namespace std;

bool light[1010]; // false表示关，true表示开

int main()
{
    int n, k;
    cin >> n >> k;

    // 模拟每个人
    for (int person = 1; person <= k; person++)
    {
        // 每个人操作自己编号倍数的灯
        for (int j = person; j <= n; j += person)
        {
            light[j] = !light[j]; // 取反操作：关变开，开变关
        }
    }

    // 输出亮着的灯
    for (int i = 1; i <= n; i++)
    {
        if (light[i] == true)
        {
            cout << i << " ";
        }
    }
    return 0;
}
~~~

### 2.2 例题2：约瑟夫问题

> n个人围成一圈，从第一个人开始报数，数到m的人出列，然后从下一个人重新开始报数，直到所有人都出列。求输出出列顺序。

这也是典型的模拟，可以用数组标记每个人是否还在圈里。

~~~c++
#include <iostream>
using namespace std;

bool inCircle[110]; // true表示还在圈里

int main()
{
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++) inCircle[i] = true;

    int count = 0;     // 报数计数器
    int outCount = 0;  // 出列人数
    int idx = 1;       // 当前报数的人

    while (outCount < n)
    {
        if (inCircle[idx] == true)
        {
            count++;
            if (count == m)
            {
                cout << idx << " ";
                inCircle[idx] = false;
                outCount++;
                count = 0;
            }
        }
        idx++;
        if (idx > n) idx = 1;
    }
    return 0;
}
~~~

### 2.3 模拟的要点总结

1. **读懂题目规则**——把题目里的流程搞清楚
2. **选择合适的数据结构**——数组、标记、队列等
3. **注意边界条件**——比如约瑟夫问题的"围成一圈"要用取模或判断
4. **仔细调试**——模拟题容易写错细节

**练习题**：
- 洛谷 P1042 [乒乓球](https://www.luogu.com.cn/problem/P1042)
- 洛谷 P1563 [玩具谜题](https://www.luogu.com.cn/problem/P1563)
- 洛谷 P1328 [生活大爆炸版石头剪刀布](https://www.luogu.com.cn/problem/P1328)

## 第3章 高精度算法

### 3.1 为什么需要高精度？

先看几个数：
- `int` 最大能存约 21亿（2^31-1 ≈ 2.1×10^9）
- `long long` 最大能存约 9×10^18（2^63-1）
- 但如果要算 **100的阶乘** 呢？100! ≈ 9.3×10^157，long long 根本装不下！

这时候就需要 **高精度算法**——用数组来模拟数字的每一位，像小学做竖式计算一样，一位一位地算。

### 3.2 高精度加法

回忆一下小学的竖式加法：
```
  1 2 3
+  4 5 6
--------
= 5 7 9
```

从个位开始，逐位相加，**满十进一**。我们用数组来模拟这个过程，为了方便，**把数字倒着存**（个位在数组下标0的位置），这样进位的时候只需要往后扩展。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int a[210], b[210], c[210]; // a + b = c

int main()
{
    string s1, s2;
    cin >> s1 >> s2;

    int len1 = s1.size();
    for (int i = 0; i < len1; i++)
        a[i] = s1[len1 - 1 - i] - 48;

    int len2 = s2.size();
    for (int i = 0; i < len2; i++)
        b[i] = s2[len2 - 1 - i] - 48;

    int len = max(len1, len2);
    for (int i = 0; i < len; i++)
    {
        c[i] += a[i] + b[i];
        if (c[i] >= 10)
        {
            c[i + 1] += c[i] / 10;
            c[i] %= 10;
        }
    }

    if (c[len] > 0) len++;

    for (int i = len - 1; i >= 0; i--)
        cout << c[i];
    return 0;
}
~~~

**核心思想**：用数组的每一个元素存一位数字，从低位到高位逐位计算，进位处理是关键！

### 3.3 高精度减法

减法比加法稍微复杂一点，因为涉及 **借位**。另外要处理谁减谁——如果被减数小于减数，结果就是负数。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int a[210], b[210], c[210];

bool cmp(string s1, string s2)
{
    if (s1.size() != s2.size())
        return s1.size() > s2.size();
    return s1 >= s2;
}

int main()
{
    string s1, s2;
    cin >> s1 >> s2;

    bool negative = false;
    if (!cmp(s1, s2))
    {
        swap(s1, s2);
        negative = true;
    }

    int len1 = s1.size();
    for (int i = 0; i < len1; i++)
        a[i] = s1[len1 - 1 - i] - 48;

    int len2 = s2.size();
    for (int i = 0; i < len2; i++)
        b[i] = s2[len2 - 1 - i] - 48;

    for (int i = 0; i < len1; i++)
    {
        if (a[i] < b[i])
        {
            a[i + 1]--;
            a[i] += 10;
        }
        c[i] = a[i] - b[i];
    }

    int len = len1;
    while (len > 1 && c[len - 1] == 0)
        len--;

    if (negative) cout << "-";
    for (int i = len - 1; i >= 0; i--)
        cout << c[i];
    return 0;
}
~~~

### 3.4 总结

高精度核心就三点：
1. **用字符串输入**，因为数字太大
2. **倒着存到数组**，方便进位/借位
3. **模拟竖式运算**，逐位处理

学会了加法和减法，乘法和除法的思路也是一样的——用数组模拟竖式。

**练习题**：
- 洛谷 P1601 [A+B Problem（高精）](https://www.luogu.com.cn/problem/P1601)
- 洛谷 P2142 [高精度减法](https://www.luogu.com.cn/problem/P2142)
- 洛谷 P1303 [A*B Problem（高精）](https://www.luogu.com.cn/problem/P1303)

## 第4章 递推

### 4.1 什么是递推

递推，就是从已知条件出发，利用**递推关系式**一步步推出未知结果。

递推有三个要素：
1. **初始条件**——已知的起点值
2. **递推关系**——当前项与前面项的关系式
3. **递推方向**——顺推（从前到后）或逆推（从后到前）

### 4.2 顺推

由初始条件开始，按顺序推导出后续结果，这叫**顺推**。

**最经典的例子：斐波那契数列**

斐波那契数列是这样的：1, 1, 2, 3, 5, 8, 13, 21...
- 第1项：F(1) = 1
- 第2项：F(2) = 1
- 从第3项开始：F(n) = F(n-1) + F(n-2)（前两项之和）

~~~c++
#include <iostream>
using namespace std;

int a[110];

int main()
{
    int n;
    cin >> n;

    a[1] = 1;   // 初始条件
    a[2] = 1;

    for (int i = 3; i <= n; i++)
    {
        a[i] = a[i - 1] + a[i - 2]; // 递推关系
    }
    
    cout << a[n];

    return 0;
}
~~~

**另一个例子：上楼梯问题**

> 有n级台阶，一次可以上1级或2级，问上到第n级有多少种不同的走法？

思考：要上到第n级，可以从第(n-1)级跨1步上来，也可以从第(n-2)级跨2步上来。
所以：F(n) = F(n-1) + F(n-2)，初始条件F(1)=1, F(2)=2。

~~~c++
#include <iostream>
using namespace std;

int f[50];

int main()
{
    int n;
    cin >> n;
    
    f[1] = 1;  // 1级台阶：1种走法（跨1步）
    f[2] = 2;  // 2级台阶：2种走法（1+1 或 2）
    
    for (int i = 3; i <= n; i++)
    {
        f[i] = f[i - 1] + f[i - 2];
    }
    
    cout << f[n] << endl;
    
    return 0;
}
~~~

### 4.3 逆推

从结果反推回初始条件，叫**逆推**。

**例子：猴子吃桃问题**

> 猴子第一天摘了若干桃子，当天吃了一半加一个；第二天又吃了剩下的一半加一个；以后每天如此。到第n天早上想再吃时，只剩下1个桃子了。问第一天摘了多少个桃子？

逆推思路：第n天剩1个，说明前一天剩下的数量 = (第n天的数量 + 1) × 2。

~~~c++
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;
    
    int peach = 1; // 第n天早上只剩下1个
    
    for (int i = n - 1; i >= 1; i--)
    {
        peach = (peach + 1) * 2; // 逆推：前一天 = (当天 + 1) × 2
    }
    
    cout << peach << endl;
    
    return 0;
}
~~~

### 4.4 递推的要点

1. **找递推关系**是最关键的一步——多举几个例子找规律
2. **确定初始条件**——边界值要算对
3. **注意数据范围**——斐波那契数列增长很快，int可能不够用

**练习题**：
- 洛谷 P1255 [数楼梯](https://www.luogu.com.cn/problem/P1255)
- 洛谷 P1028 [数的计算](https://www.luogu.com.cn/problem/P1028)
- ybt [斐波那契数列](http://ybt.ssoier.cn:8088/problem_show.php?pid=1159)

## 第5章 递归

### 5.1 什么是递归

递归，就是函数**自己调用自己**。你可以想象成"俄罗斯套娃"——打开一个，里面还有一个。

递归由两部分组成：
1. **递进**——函数不断调用自己，深入下去
2. **回归**——到达终止条件后，一层层返回结果

### 5.2 递进

函数自己调用自己，如果没有终止条件就会无限循环下去（栈溢出）。

~~~c++
#include <iostream>
using namespace std;

void mf()
{
    cout << "进入下一层" << endl;
    mf(); // 自己调用自己
}

int main()
{
    mf();
    return 0;
}
~~~

运行这个程序会发现它在输出很多行后崩溃退出——因为**栈空间是有限的**，无限递归会把栈撑爆。

### 5.3 回归

加一个终止条件，让递归在合适的时候"回头"：

~~~c++
#include <iostream>
using namespace std;

void mf(int x)
{
    cout << x << ": 进入下一层" << endl;
    x++;
    if (x < 5)      // 加条件限制
    {
        mf(x);      // 递进
    }
    return;         // 回归
}

int main()
{
    mf(0);
    return 0;
}
~~~

### 5.4 递进 + 回归 = 递归

下面的例子展示了递进时输出和回归时输出的区别：

~~~c++
#include <iostream>
using namespace std;

void fun(int x)
{
    cout << x << " "; // 递进时输出
    x++;
    if (x <= 10)
    {
        fun(x);
    }
    return;
}

void fun2(int n)
{
    if (n < 10)
    {
        fun2(n + 1);
    }
    int a = 11 - n;
    cout << a << " "; // 回归时输出
    return;
}

int main()
{
    fun(1);  // 输出：1 2 3 4 5 6 7 8 9 10
    cout << endl;
    fun2(1); // 输出：10 9 8 7 6 5 4 3 2 1
    return 0;
}
~~~

`fun`先输出再递归，所以是从小到大输出；`fun2`先递归再输出，所以是从大到小输出。

### 5.5 递归的经典应用：斐波那契数列

斐波那契数列用递归来实现非常直观：

~~~c++
#include <iostream>
using namespace std;

int fib(int n)
{
    if (n == 1 || n == 2)
        return 1;                   // 终止条件
    return fib(n - 1) + fib(n - 2); // 递归调用
}

int main()
{
    int n;
    cin >> n;
    cout << fib(n) << endl;
    return 0;
}
~~~

**注意**：这个递归实现虽然好理解，但效率很低——因为有很多重复计算。比如算fib(5)要算fib(4)和fib(3)，算fib(4)又要算fib(3)和fib(2)，fib(3)被重复算了两次。n越大，重复越严重。这就是为什么实际应用中常用**递推**（循环）代替递归。

### 5.6 递归 vs 递推

| 对比项 | 递归 | 递推 |
| :---- | :--- | :--- |
| 实现方式 | 函数调用自身 | 循环迭代 |
| 代码可读性 | 简洁、直观 | 稍复杂 |
| 效率 | 较低（有函数调用开销） | 高 |
| 可能的问题 | 栈溢出 | 无 |
| 适用场景 | 树形结构、分治 | 线性递推 |

### 5.7 递归的要点

1. **必须有终止条件**——否则会无限递归
2. **每次递归都要向终止条件靠近**——参数要变化
3. **递归能解决的问题通常有重复子结构**——一个大问题可以拆成小问题

**练习题**：
- 洛谷 P5739 [【深基7.例7】计算阶乘](https://www.luogu.com.cn/problem/P5739)
- 洛谷 P1464 [Function](https://www.luogu.com.cn/problem/P1464)
- ybt [递归求阶乘](http://ybt.ssoier.cn:8088/problem_show.php?pid=1150)


## 第6章 贪心

### 6.1 贪心思想

贪心，顾名思义就是 **只顾眼前**——在每一步都做出当前看起来最优的选择，期望最终得到全局最优解。

用大白话说就是：**"先挑最好的，剩下的再说"**。

但要注意：贪心并不是万能的！只有满足 **最优子结构** 和 **贪心选择性质** 的问题才能用贪心。

### 6.2 例题1：找零问题

> 有面值100元、50元、20元、10元、5元、1元的人民币，要凑出n元钱，问最少需要多少张纸币？

这个问题的贪心策略很简单：**能用大面额就用大面额**。比如要凑378元，先拿3张100（剩78），再拿1张50（剩28），再拿1张20（剩8），再拿1张5（剩3），最后拿3张1元。一共3+1+1+1+3=9张。

为什么这样是最优的？因为人民币的面额设计使得大面额是小面额的整数倍，所以用大面额永远不会亏。

~~~c++
#include <iostream>
using namespace std;

int money[] = {100, 50, 20, 10, 5, 1}; // 面额从大到小

int main()
{
    int n;
    cin >> n;

    int count = 0;
    for (int i = 0; i < 6; i++)
    {
        count += n / money[i];
        n %= money[i];
    }

    cout << count << endl;
    return 0;
}
~~~

### 6.3 例题2：活动安排（区间调度）

> 有n个活动，每个活动有开始时间s[i]和结束时间e[i]。一个人同时只能参加一个活动，问最多能参加多少个活动？

贪心策略：**按结束时间最早排序，每次选结束时间最早且不与之前冲突的活动**。

为什么？因为结束时间越早，留给后面的时间就越多。这就是典型的"局部最优→全局最优"。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

struct Activity
{
    int start, end;
};

bool cmp(Activity a, Activity b)
{
    return a.end < b.end;
}

Activity act[1010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> act[i].start >> act[i].end;

    sort(act + 1, act + n + 1, cmp);

    int ans = 0;
    int lastEnd = 0;
    for (int i = 1; i <= n; i++)
    {
        if (act[i].start >= lastEnd)
        {
            ans++;
            lastEnd = act[i].end;
        }
    }

    cout << ans << endl;
    return 0;
}
~~~

### 6.4 注意事项：贪心不一定正确！

贪心虽然好用，但 **不是所有问题都能用贪心**。比如下面这个例子：

> 背包问题：有一个容量为10的背包，有三样东西——A:重量6价值12，B:重量5价值10，C:重量5价值10。要装价值最大？

如果按"性价比"贪心，A的性价比是12/6=2，B和C都是2，都一样。但如果按"重量从小到大"贪心，选B+C=价值20，而选A+C=价值22（但超重了）。实际上选B+C=20是最优解，但如果物品换成A:重量6价值9，B:重量5价值10，C:重量5价值10，贪心就会选错。

所以 **用贪心之前一定要确认问题是否满足贪心性质**。

### 6.5 贪心要点

1. **确定贪心策略**——每一步选什么？
2. **证明贪心正确**——为什么局部最优能推出全局最优？
3. **如果贪心不对，试试动态规划**（以后会学）

**练习题**：
- 洛谷 P1223 [排队接水](https://www.luogu.com.cn/problem/P1223)
- 洛谷 P1094 [纪念品分组](https://www.luogu.com.cn/problem/P1094)
- 洛谷 P1803 [凌乱的yyy / 线段覆盖](https://www.luogu.com.cn/problem/P1803)

## 第7章 二分

### 7.1 二分思想

二分，就是 **每次把搜索范围缩小一半**。你想啊，如果有一本1000页的字典，要找第500页的单词，你不会从第1页开始翻——你会直接翻到中间，然后判断是往前找还是往后找。这就是二分的思想。

二分有两个主要应用：
1. **二分查找**——在有序数组中找某个数
2. **二分答案**——在范围内猜答案，验证对不对

### 7.2 二分查找（有序数组中找某个数）

给定一个有序数组（从小到大），找一个数x的位置。最笨的方法是遍历——O(n)。但用二分只需要O(log n)！100万的数组，遍历要100万次，二分只要20次。

~~~c++
#include <iostream>
using namespace std;

int a[100010];

int main()
{
    int n, x;
    cin >> n >> x;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    int left = 1, right = n;
    int ans = -1;

    while (left <= right)
    {
        int mid = (left + right) / 2;
        if (a[mid] == x)
        {
            ans = mid;
            break;
        }
        else if (a[mid] < x)
            left = mid + 1;
        else
            right = mid - 1;
    }

    cout << ans << endl;
    return 0;
}
~~~

**重要细节**：
- `mid = (left + right) / 2` 可能会溢出，可以写成 `mid = left + (right - left) / 2`
- 循环条件是 `left <= right` 还是 `left < right`，看具体需求
- 二分必须在 **有序** 的数据上进行

### 7.3 二分答案（最大值最小化/最小值最大化）

这是二分的高阶用法。有些问题我们不知道答案是多少，但知道答案的范围，而且可以快速验证某个值是否可行。这时候就可以 **在答案范围内二分，每次验证中间值是否可行**。

比如说：切木头问题——有n根木头，要切成k根长度相等的小段，问每段最长能有多长？

我们不知道答案是多少，但答案肯定在(0, 最长木头]之间。我们可以二分这个长度，每次检查能不能切出k根。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int wood[100010];
int n, k;

bool check(int len)
{
    long long count = 0;
    for (int i = 1; i <= n; i++)
        count += wood[i] / len;
    return count >= k;
}

int main()
{
    cin >> n >> k;
    int maxLen = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> wood[i];
        maxLen = max(maxLen, wood[i]);
    }

    int left = 1, right = maxLen;
    int ans = 0;

    while (left <= right)
    {
        int mid = (left + right) / 2;
        if (check(mid))
        {
            ans = mid;
            left = mid + 1;
        }
        else
        {
            right = mid - 1;
        }
    }

    cout << ans << endl;
    return 0;
}
~~~

**二分答案的套路**：
1. 确定答案的范围
2. 写一个check函数验证某个值是否可行
3. 在范围内二分，根据check结果缩小范围
4. 最终得到最优解

### 7.4 二分要点

1. **有序是二分的前提**（查找时必须有序）
2. **注意边界条件**——left、right、mid的取值
3. **二分答案的关键是check函数**——验证速度要快
4. **模板要熟练**——建议背下来

**练习题**：
- 洛谷 P2249 [【深基13.例1】查找](https://www.luogu.com.cn/problem/P2249)
- 洛谷 P1024 [一元三次方程求解](https://www.luogu.com.cn/problem/P1024)
- 洛谷 P2678 [跳石头](https://www.luogu.com.cn/problem/P2678)

## 第8章 前缀和、差分

### 8.1 一维前缀和

前缀和是一个 **预处理** 技巧，用来 **快速求区间和**。

假设有一个数组a[1..n]，我们想求区间[l, r]的和，最笨的方法是把l到r遍历一遍加起来——O(n)。但如果有m次询问，每次都是O(n)，总复杂度就O(mn)，太慢了。

前缀和的思路：先算出一个前缀和数组s，其中 **s[i] = a[1] + a[2] + ... + a[i]**（前i项的和）。那么区间[l, r]的和 = **s[r] - s[l-1]**。

看个例子：
```
a:   1  3  2  5  4
s:   1  4  6 11 15

问 a[2]~a[4]的和 = s[4] - s[1] = 11 - 1 = 10
（3+2+5=10）
```

~~~c++
#include <iostream>
using namespace std;

int a[100010], s[100010];

int main()
{
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    for (int i = 1; i <= n; i++)
        s[i] = s[i - 1] + a[i];

    for (int i = 1; i <= m; i++)
    {
        int l, r;
        cin >> l >> r;
        cout << s[r] - s[l - 1] << endl;
    }
    return 0;
}
~~~

**威力**：预处理一次O(n)，之后每次查询O(1)。如果n=10万，m=10万，暴力需要10^10次运算，前缀和只需要10^5+10^5=2×10^5次。

### 8.2 一维差分

差分是前缀和的 **逆运算**，用来 **快速区间修改**。

什么叫区间修改？就是给数组的某个区间 **所有元素加上同一个数**。

最笨的方法：遍历区间，每个元素都加——O(n)。有m次修改的话就是O(mn)。

差分数组的思想：我们维护一个差分数组d，使得 **d[i] = a[i] - a[i-1]**。对a的区间[l, r]加x，等价于 **d[l] += x, d[r+1] -= x**。

最后，对差分数组求前缀和，就能还原出修改后的a。



### 8.3 差分代码示例

~~~c++
#include <iostream>
using namespace std;

int a[100010], d[100010];

int main()
{
    int n, m;
    cin >> n >> m;
    
    // 读入原数组
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    
    // 构建差分数组 d[i] = a[i] - a[i-1]
    for (int i = 1; i <= n; i++)
        d[i] = a[i] - a[i - 1];
    
    // m次区间修改
    for (int i = 1; i <= m; i++)
    {
        int l, r, x;
        cin >> l >> r >> x;
        d[l] += x;
        d[r + 1] -= x;
    }
    
    // 对差分数组求前缀和，还原出修改后的a
    for (int i = 1; i <= n; i++)
    {
        a[i] = a[i - 1] + d[i];
        cout << a[i] << " ";
    }
    
    return 0;
}
~~~

**理解差分**：差分和前缀和是一对"逆运算"。你可以把差分想象成一个"修改记录"，最后通过一次前缀和把所有修改效果"累加"到原数组上。

### 8.4 前缀和与差分的对比

| 操作 | 暴力做法 | 优化做法 | 单次复杂度 |
| :--- | :------ | :------ | :-------- |
| 区间求和 | 遍历区间 | 前缀和 | O(n)预处理，O(1)查询 |
| 区间加数 | 遍历区间 | 差分 | O(1)修改，O(n)还原 |

**练习题**：
- 洛谷 P8218 [【深进1.例1】求区间和](https://www.luogu.com.cn/problem/P8218)
- 洛谷 P2367 [语文成绩](https://www.luogu.com.cn/problem/P2367)
- 洛谷 P3397 [地毯](https://www.luogu.com.cn/problem/P3397)

## 第9章 深度优先搜索（DFS）

### 9.1 什么是DFS

深度优先搜索，英文叫 Depth First Search，简称DFS。它的思想是：**一条路走到黑，走不通了再回头**。

想象你在走迷宫：每次遇到岔路，随便选一条往前走，如果发现走不通了，就退回到上一个岔路口，换一条路继续走。这就是DFS。

### 9.2 DFS的实现

DFS通常用**递归**来实现：

~~~c++
void dfs(当前状态)
{
    if (到达终点)
    {
        处理结果;
        return;
    }
    
    for (所有可能的下一步)
    {
        if (下一步合法)
        {
            标记已访问;
            dfs(下一步);
            取消标记;
        }
    }
}
~~~

### 9.3 例题2：八皇后问题

> 在8×8的棋盘上放8个皇后，使它们互不攻击（任何两个不在同一行、同一列、同一对角线）。

~~~c++
#include <iostream>
using namespace std;

int col[10];
bool rowUsed[10];
bool diag1[20];
bool diag2[20];
int ans = 0;

void dfs(int r)
{
    if (r > 8)
    {
        ans++;
        return;
    }
    
    for (int c = 1; c <= 8; c++)
    {
        if (!rowUsed[c] && !diag1[r - c + 8] && !diag2[r + c])
        {
            col[r] = c;
            rowUsed[c] = diag1[r - c + 8] = diag2[r + c] = true;
            dfs(r + 1);
            rowUsed[c] = diag1[r - c + 8] = diag2[r + c] = false;
        }
    }
}

int main()
{
    dfs(1);
    cout << ans << endl;
    return 0;
}
~~~

### 9.4 DFS的要点

1. **回溯是DFS的精髓**——递归前标记，递归后取消标记
2. **剪枝可以大幅提高效率**——提前判断不可行的情况，跳过
3. **DFS适合解决"是否存在"和"所有方案"的问题**

**练习题**：
- 洛谷 P1706 [全排列问题](https://www.luogu.com.cn/problem/P1706)
- 洛谷 P1219 [八皇后](https://www.luogu.com.cn/problem/P1219)
- 洛谷 P2404 [自然数的拆分问题](https://www.luogu.com.cn/problem/P2404)

## 第10章 广度优先搜索（BFS）

### 10.1 什么是BFS

广度优先搜索，英文叫 Breadth First Search，简称BFS。它的思想是：**层层推进，逐层扩展**。

还是走迷宫的比喻——DFS是一条路走到黑，而BFS是"广撒网"：先看离起点1步能到哪些位置，再看2步能到哪些位置，一层层往外扩散。

BFS最重要的特性：**第一次到达某个状态时，走的步数就是最短步数**。

### 10.2 BFS的实现

BFS通常用**队列**来实现：

~~~c++
#include <queue>
queue<状态类型> q;

void bfs(起点)
{
    q.push(起点);
    visited[起点] = true;
    
    while (!q.empty())
    {
        当前状态 = q.front();
        q.pop();
        
        for (所有可能的下一步)
        {
            if (下一步合法且未访问)
            {
                标记已访问;
                记录步数(步数 = 当前步数 + 1);
                q.push(下一步);
            }
        }
    }
}
~~~

### 10.3 例题1：走迷宫

> 给定一个n×m的迷宫，'S'表示起点，'T'表示终点，'#'表示墙，'.'表示路。求从起点到终点的最短步数。

~~~c++
#include <iostream>
#include <queue>
using namespace std;

char maze[110][110];
bool visited[110][110];
int step[110][110];

int dx[] = {-1, 1, 0, 0};
int dy[] = {0, 0, -1, 1};

struct Point
{
    int x, y;
};

int main()
{
    int n, m;
    cin >> n >> m;
    
    int sx, sy, tx, ty;
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            cin >> maze[i][j];
            if (maze[i][j] == 'S') { sx = i; sy = j; }
            if (maze[i][j] == 'T') { tx = i; ty = j; }
        }
    }
    
    queue<Point> q;
    q.push({sx, sy});
    visited[sx][sy] = true;
    step[sx][sy] = 0;
    
    while (!q.empty())
    {
        Point cur = q.front();
        q.pop();
        
        if (cur.x == tx && cur.y == ty)
        {
            cout << step[cur.x][cur.y] << endl;
            return 0;
        }
        
        for (int i = 0; i < 4; i++)
        {
            int nx = cur.x + dx[i];
            int ny = cur.y + dy[i];
            
            if (nx >= 1 && nx <= n && ny >= 1 && ny <= m
                && maze[nx][ny] != '#' && !visited[nx][ny])
            {
                visited[nx][ny] = true;
                step[nx][ny] = step[cur.x][cur.y] + 1;
                q.push({nx, ny});
            }
        }
    }
    
    cout << -1 << endl;
    return 0;
}
~~~

### 10.4 BFS的要点

1. **队列是BFS的核心数据结构**
2. **第一次到达就是最短路径**——因为BFS是逐层扩展的
3. **BFS适合解决"最短路径"和"最少步数"问题**

### 10.5 DFS vs BFS

| 对比项 | DFS（深度优先） | BFS（广度优先） |
| :---- | :------------- | :------------- |
| 实现方式 | 递归（栈） | 队列 |
| 空间复杂度 | 与深度有关 | 与宽度有关 |
| 能否找最短路径 | 不能（需全部搜索） | 能（第一次到达就是最短） |
| 适用场景 | 所有方案、存在性 | 最短路径、最少步数 |

**练习题**：
- 洛谷 P1443 [马的遍历](https://www.luogu.com.cn/problem/P1443)
- 洛谷 P1135 [奇怪的电梯](https://www.luogu.com.cn/problem/P1135)
- 洛谷 P2298 [Maze](https://www.luogu.com.cn/problem/P2298)

# 数据结构部分

## 第11章 栈

### 11.1 什么是栈

栈（Stack）是一种 **后进先出（LIFO, Last In First Out）** 的数据结构。就像一摞盘子——你洗好的盘子会放在最上面，要拿的时候也是先从最上面拿。**后放进去的先拿出来**。

栈的基本操作：
- `push(x)`：把x压入栈顶
- `pop()`：弹出栈顶元素
- `top()`：查看栈顶元素
- `empty()`：判断栈是否为空

### 11.2 C++ STL中的栈

~~~c++
#include <iostream>
#include <stack>
using namespace std;

int main()
{
    stack<int> s;
    
    s.push(10);
    s.push(20);
    s.push(30);
    
    cout << s.top() << endl;
    s.pop();
    cout << s.top() << endl;
    cout << s.size() << endl;
    cout << s.empty() << endl;
    
    return 0;
}
~~~

### 11.3 例题：括号匹配

> 给定一个只包含 `()` `[]` `{}` 的字符串，判断括号是否匹配。

思路：遇到左括号就入栈，遇到右括号就检查栈顶是否是对应的左括号。

~~~c++
#include <iostream>
#include <stack>
#include <string>
using namespace std;

bool match(char left, char right)
{
    return (left == '(' && right == ')') ||
           (left == '[' && right == ']') ||
           (left == '{' && right == '}');
}

int main()
{
    string s;
    cin >> s;
    
    stack<char> st;
    bool ok = true;
    
    for (char c : s)
    {
        if (c == '(' || c == '[' || c == '{')
        {
            st.push(c);
        }
        else
        {
            if (st.empty() || !match(st.top(), c))
            {
                ok = false;
                break;
            }
            st.pop();
        }
    }
    
    if (ok && st.empty())
        cout << "匹配" << endl;
    else
        cout << "不匹配" << endl;
    
    return 0;
}
~~~

**练习题**：
- 洛谷 P1739 [表达式括号匹配](https://www.luogu.com.cn/problem/P1739)
- 洛谷 P1449 [后缀表达式](https://www.luogu.com.cn/problem/P1449)

## 第12章 队列

### 12.1 什么是队列

队列（Queue）是一种 **先进先出（FIFO, First In First Out）** 的数据结构。就像排队买奶茶——先排队的人先买到，后排队的人后买到。**先来的先服务**。

队列的基本操作：
- `push(x)`：把x加入队尾
- `pop()`：弹出队首元素
- `front()`：查看队首元素
- `back()`：查看队尾元素
- `empty()`：判断队列是否为空

### 12.2 C++ STL中的队列

~~~c++
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    queue<int> q;
    
    q.push(10);
    q.push(20);
    q.push(30);
    
    cout << q.front() << endl;
    q.pop();
    cout << q.front() << endl;
    cout << q.size() << endl;
    cout << q.empty() << endl;
    
    return 0;
}
~~~

### 12.3 例题：约瑟夫问题（用队列实现）

约瑟夫问题我们在模拟算法那章用数组模拟过了，这次用队列来实现更直观：

~~~c++
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    int n, m;
    cin >> n >> m;
    
    queue<int> q;
    for (int i = 1; i <= n; i++)
        q.push(i);
    
    int count = 0;
    while (!q.empty())
    {
        count++;
        int cur = q.front();
        q.pop();
        
        if (count == m)
        {
            cout << cur << " ";
            count = 0;
        }
        else
        {
            q.push(cur);
        }
    }
    
    return 0;
}
~~~

**练习题**：
- 洛谷 P1996 [约瑟夫问题](https://www.luogu.com.cn/problem/P1996)
- 洛谷 P1540 [机器翻译](https://www.luogu.com.cn/problem/P1540)

## 第13章 树

### 13.1 什么是树

树是一种**非线性**的数据结构，由节点（node）和节点之间的边（edge）组成。

树的常见术语：
- **根节点**：树的最顶层节点
- **父节点/子节点**：上下层关系
- **叶子节点**：没有子节点的节点
- **深度**：从根到某个节点的层数
- **子树**：一个节点及其所有后代

### 13.2 二叉树

二叉树是最常用的树结构，每个节点**最多有2个子节点**（左孩子和右孩子）。

~~~c++
struct TreeNode
{
    int val;
    TreeNode* left;
    TreeNode* right;
};
~~~

### 13.3 树的存储（数组方式）

竞赛中最常用的存树方式是**数组模拟**：

~~~c++
int parent[1010];
int leftChild[1010], rightChild[1010];
#include <vector>
vector<int> children[1010];
~~~

**练习题**：
- 洛谷 P4913 [二叉树深度](https://www.luogu.com.cn/problem/P4913)
- 洛谷 P1305 [新二叉树](https://www.luogu.com.cn/problem/P1305)

## 第14章 图

### 14.1 什么是图

图（Graph）由**顶点**（vertex）和**边**（edge）组成。相比树，图更自由——可以有环、可以没有根。

图的分类：
- **有向图 vs 无向图**：边是否有方向
- **有权图 vs 无权图**：边是否有权重

### 14.2 图的存储

#### 1. 邻接矩阵

~~~c++
int graph[1010][1010];

void addEdge(int u, int v)
{
    graph[u][v] = 1;
    graph[v][u] = 1;
}
~~~

**优点**：直接判断两点间是否有边，O(1)
**缺点**：空间O(n²)，n=10万时存不下

#### 2. 邻接表

~~~c++
#include <vector>
vector<int> graph[1010];

void addEdge(int u, int v)
{
    graph[u].push_back(v);
    graph[v].push_back(u);
}
~~~

**优点**：空间O(n+m)，m是边数
**缺点**：判断两点是否相连需要遍历列表

### 14.3 练习题

- 洛谷 P5318 [查找文献](https://www.luogu.com.cn/problem/P5318)
- 洛谷 P3916 [图的遍历](https://www.luogu.com.cn/problem/P3916)

# 算法入门部分

## 第15章 动态规划

### 15.1 什么是动态规划

动态规划（Dynamic Programming，简称DP）是一种**把大问题拆成小问题，先解决小问题，再用小问题的答案拼出大问题答案**的方法。

DP的核心思想：**记住已经算过的结果，避免重复计算**。

### 15.2 DP三要素

1. **状态定义**——dp[i]表示什么
2. **状态转移方程**——dp[i]和前面的dp值有什么关系
3. **初始条件**——边界值

### 15.3 例题1：斐波那契数列（DP视角）

~~~c++
int dp[50];
dp[1] = dp[2] = 1;
for (int i = 3; i <= n; i++)
    dp[i] = dp[i-1] + dp[i-2];
~~~

### 15.4 例题2：最大子段和

> 给定一个数组a[1..n]，求连续子数组的最大和。

比如：[-2, 1, -3, 4, -1, 2, 1, -5, 4]，最大子段和是 [4, -1, 2, 1] = 6。

状态定义：`dp[i]` = 以a[i]结尾的最大子段和。
转移方程：`dp[i] = max(a[i], dp[i-1] + a[i])`

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int a[200010], dp[200010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    
    dp[1] = a[1];
    int ans = dp[1];
    
    for (int i = 2; i <= n; i++)
    {
        dp[i] = max(a[i], dp[i - 1] + a[i]);
        ans = max(ans, dp[i]);
    }
    
    cout << ans << endl;
    return 0;
}
~~~

### 15.5 例题3：01背包问题

> 有n件物品，每件有重量w[i]和价值v[i]。有一个容量为C的背包，问最多能带走多少价值的物品？

状态定义：`dp[i][j]` = 前i件物品中选，总重量不超过j的最大价值。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int w[1010], v[1010];
int dp[1010][1010];

int main()
{
    int n, C;
    cin >> n >> C;
    for (int i = 1; i <= n; i++)
        cin >> w[i] >> v[i];
    
    for (int i = 1; i <= n; i++)
    {
        for (int j = 0; j <= C; j++)
        {
            dp[i][j] = dp[i - 1][j];
            if (j >= w[i])
                dp[i][j] = max(dp[i][j], dp[i - 1][j - w[i]] + v[i]);
        }
    }
    
    cout << dp[n][C] << endl;
    return 0;
}
~~~

**空间优化**（一维数组）：

~~~c++
int dp[1010];
for (int i = 1; i <= n; i++)
    for (int j = C; j >= w[i]; j--)
        dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
~~~

### 15.6 DP的要点

1. **状态定义要准确**
2. **转移方程是核心**
3. **初始化很重要**
4. **DP需要满足"最优子结构"和"无后效性"**

**练习题**：
- 洛谷 P1115 [最大子段和](https://www.luogu.com.cn/problem/P1115)
- 洛谷 P1048 [采药](https://www.luogu.com.cn/problem/P1048)
- 洛谷 P1216 [数字三角形](https://www.luogu.com.cn/problem/P1216)

## 第16章 树的遍历

### 16.1 二叉树的三种遍历方式

#### 1. 前序遍历

顺序：**根节点 → 左子树 → 右子树**

~~~c++
void preorder(TreeNode* root)
{
    if (root == NULL) return;
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
}
~~~

#### 2. 中序遍历

顺序：**左子树 → 根节点 → 右子树**

~~~c++
void inorder(TreeNode* root)
{
    if (root == NULL) return;
    inorder(root->left);
    cout << root->val << " ";
    inorder(root->right);
}
~~~

#### 3. 后序遍历

顺序：**左子树 → 右子树 → 根节点**

~~~c++
void postorder(TreeNode* root)
{
    if (root == NULL) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->val << " ";
}
~~~

### 16.2 完整示例

~~~c++
#include <iostream>
using namespace std;

struct TreeNode
{
    char val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(char v) : val(v), left(NULL), right(NULL) {}
};

void preorder(TreeNode* r)
{
    if (!r) return;
    cout << r->val;
    preorder(r->left);
    preorder(r->right);
}

void inorder(TreeNode* r)
{
    if (!r) return;
    inorder(r->left);
    cout << r->val;
    inorder(r->right);
}

void postorder(TreeNode* r)
{
    if (!r) return;
    postorder(r->left);
    postorder(r->right);
    cout << r->val;
}

int main()
{
    TreeNode* root = new TreeNode('A');
    root->left = new TreeNode('B');
    root->right = new TreeNode('C');
    root->left->left = new TreeNode('D');
    root->left->right = new TreeNode('E');
    
    cout << "前序: "; preorder(root); cout << endl;
    cout << "中序: "; inorder(root); cout << endl;
    cout << "后序: "; postorder(root); cout << endl;
    
    return 0;
}
~~~

**练习题**：
- 洛谷 P4913 [二叉树深度](https://www.luogu.com.cn/problem/P4913)
- 洛谷 P1305 [新二叉树](https://www.luogu.com.cn/problem/P1305)

## 第17章 图的算法

### 17.1 图的遍历

#### DFS遍历图

~~~c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> graph[1010];
bool visited[1010];

void dfs(int u)
{
    visited[u] = true;
    cout << u << " ";
    for (int v : graph[u])
    {
        if (!visited[v])
            dfs(v);
    }
}
~~~

#### BFS遍历图

~~~c++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

vector<int> graph[1010];
bool visited[1010];

void bfs(int start)
{
    queue<int> q;
    q.push(start);
    visited[start] = true;
    
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        cout << u << " ";
        
        for (int v : graph[u])
        {
            if (!visited[v])
            {
                visited[v] = true;
                q.push(v);
            }
        }
    }
}
~~~

### 17.2 最短路径：Floyd算法

求**任意两点之间的最短路径**。核心思想：尝试让每个顶点作为"中转点"。

~~~c++
int dist[510][510];

void floyd(int n)
{
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                if (dist[i][k] + dist[k][j] < dist[i][j])
                    dist[i][j] = dist[i][k] + dist[k][j];
}
~~~

**注意**：Floyd的复杂度是O(n³)，只适合n ≤ 500的情况。

### 17.3 最小生成树：Prim算法

最小生成树：在一个无向连通图中，选n-1条边，把所有顶点连起来，且总权重最小。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int graph[1010][1010];
int dist[1010];
bool selected[1010];

int prim(int n)
{
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    int ans = 0;
    
    for (int i = 1; i <= n; i++)
    {
        int u = -1;
        for (int j = 1; j <= n; j++)
        {
            if (!selected[j] && (u == -1 || dist[j] < dist[u]))
                u = j;
        }
        
        if (dist[u] == 0x3f3f3f3f) break;
        selected[u] = true;
        ans += dist[u];
        
        for (int v = 1; v <= n; v++)
        {
            if (!selected[v] && graph[u][v] < dist[v])
                dist[v] = graph[u][v];
        }
    }
    
    return ans;
}
~~~

### 17.4 欧拉路径 & 哈密尔顿路径

**欧拉路径**：一笔画问题——从某点出发，经过**每条边恰好一次**，最后到达另一点（或回到起点）。

**哈密尔顿路径**：经过**每个顶点恰好一次**的路径。

欧拉路径有简洁的判定条件（度数为奇数的顶点个数为0或2），而哈密尔顿路径至今没有高效的判定算法（NP问题）。

**练习题**：
- 洛谷 P4779 [【模板】单源最短路径](https://www.luogu.com.cn/problem/P4779)
- 洛谷 P3366 [【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)
- 洛谷 P2731 [骑马修栅栏](https://www.luogu.com.cn/problem/P2731)
