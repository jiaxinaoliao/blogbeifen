---
title: C++算法进阶
tags: OI相关
categories: C++ OI 教程
abbrlink: 49329
date: 2026-05-16 20:36:00
---

# c++算法进阶：进阶篇

欢迎来到进阶篇！如果你已经学完了入门篇的枚举、模拟、递归、贪心、二分、前缀和、DFS、BFS、栈、队列、树、图、动态规划和基础图论算法，那你已经具备了不错的基础。接下来我们要往更深处探索——高精度乘除法、排序算法、二分进阶、前缀和与差分进阶、动态规划的更多模型、图论中最短路和并查集、字符串匹配、数论、STL进阶以及各种优化技巧。准备好了吗？Let's go！

## 第1章 高精度乘除法

在入门篇我们学了高精度加法和减法，原理就是用数组模拟竖式，倒着存数字，逐位运算。这一章我们继续学高精度的乘法和除法，思路一样，但细节更丰富。

### 1.1 高精度乘法（模拟竖式）

高精度乘法的思路：还是模拟竖式。想象一下小学做乘法：
```
   1 2 3
×   4 5
--------
   6 1 5
+ 4 9 2
--------
= 5 5 3 5
```

核心规律：a的第i位乘以b的第j位，结果会累加到c的第i+j位上。然后统一处理进位。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int a[1010], b[1010], c[2010];

int main()
{
    string s1, s2;
    cin >> s1 >> s2;

    int len1 = s1.size(), len2 = s2.size();
    for (int i = 0; i < len1; i++)
        a[i] = s1[len1 - 1 - i] - 48;
    for (int i = 0; i < len2; i++)
        b[i] = s2[len2 - 1 - i] - 48;

    // 核心：逐位相乘，累加到对应位置
    for (int i = 0; i < len1; i++)
    {
        for (int j = 0; j < len2; j++)
        {
            c[i + j] += a[i] * b[j];
            // 直接在累加时处理进位
            c[i + j + 1] += c[i + j] / 10;
            c[i + j] %= 10;
        }
    }

    int len = len1 + len2;
    while (len > 1 && c[len - 1] == 0)
        len--;

    for (int i = len - 1; i >= 0; i--)
        cout << c[i];
    return 0;
}
~~~

这里的关键是 c[i + j] += a[i] * b[j]——两个数字的第i位和第j位相乘，结果放在结果的第i+j位。最后统一处理进位。结果的长度最多是 len1 + len2，但要去掉前导零。

### 1.2 高精度除法

高精度除法分两种：高精度除以低精度（一个大数除以一个小整数）和高精度除以高精度。我们重点讲第一种，竞赛中最常用。

高精度除以低精度的思路：从高位到低位逐位计算。每次取当前位的数字，加上上一位的余数乘以10，然后除以除数，商就是当前位的值，余数留给下一位。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int a[1010], ans[1010];

int main()
{
    string s;
    int b;
    cin >> s >> b;

    int len = s.size();
    for (int i = 0; i < len; i++)
        a[i] = s[i] - 48; // 注意：除法是从高位开始，所以不用倒着存

    long long remainder = 0;
    int pos = 0; // 商的位数

    for (int i = 0; i < len; i++)
    {
        remainder = remainder * 10 + a[i];
        ans[pos++] = remainder / b;
        remainder %= b;
    }

    // 去掉前导零
    int start = 0;
    while (start < pos - 1 && ans[start] == 0)
        start++;

    for (int i = start; i < pos; i++)
        cout << ans[i];
    cout << endl;
    cout << "余数: " << remainder << endl;

    return 0;
}
~~~

注意和加、减、乘的区别：除法是从高位往低位算的，所以数字不用倒着存。每一步把上一步的余数"拉下来"乘以10，再加当前位的数字，然后除以除数。

### 1.3 高精度模板封装

在实际竞赛中，每次都重写一遍高精度很麻烦。我们可以把高精度封装成一个结构体，方便复用。

~~~c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

struct BigInt
{
    int d[1010];
    int len;

    BigInt() { memset(d, 0, sizeof(d)); len = 1; }

    BigInt(string s)
    {
        memset(d, 0, sizeof(d));
        len = s.size();
        for (int i = 0; i < len; i++)
            d[i] = s[len - 1 - i] - 48;
    }

    // 高精度加法
    BigInt operator+(const BigInt &b) const
    {
        BigInt res;
        int maxLen = max(len, b.len);
        for (int i = 0; i < maxLen; i++)
        {
            res.d[i] += d[i] + b.d[i];
            if (res.d[i] >= 10)
            {
                res.d[i + 1] += res.d[i] / 10;
                res.d[i] %= 10;
            }
        }
        res.len = maxLen + (res.d[maxLen] > 0);
        return res;
    }

    // 高精度乘法
    BigInt operator*(const BigInt &b) const
    {
        BigInt res;
        for (int i = 0; i < len; i++)
        {
            for (int j = 0; j < b.len; j++)
            {
                res.d[i + j] += d[i] * b.d[j];
                res.d[i + j + 1] += res.d[i + j] / 10;
                res.d[i + j] %= 10;
            }
        }
        res.len = len + b.len;
        while (res.len > 1 && res.d[res.len - 1] == 0)
            res.len--;
        return res;
    }

    void print()
    {
        for (int i = len - 1; i >= 0; i--)
            cout << d[i];
    }
};

int main()
{
    string s1, s2;
    cin >> s1 >> s2;
    BigInt a(s1), b(s2);
    BigInt c = a + b;
    c.print(); cout << endl;
    BigInt d = a * b;
    d.print(); cout << endl;
    return 0;
}
~~~

封装之后，用起来就像用内置类型一样方便。你可以继续完善这个模板，加上减法、除法、比较等功能。

**练习题**：
- 洛谷 P1303 [A*B Problem（高精）](https://www.luogu.com.cn/problem/P1303)
- 洛谷 P1480 [A/B Problem（高精除以低精）](https://www.luogu.com.cn/problem/P1480)
- 洛谷 P2005 [A/B Problem II](https://www.luogu.com.cn/problem/P2005)

## 第2章 排序算法

排序是计算机科学中最基础的问题之一。虽然C++有现成的 sort 函数，但理解不同排序算法的原理，能帮你深入理解算法思想，也方便应对一些特殊场景。

### 2.1 冒泡排序

冒泡排序的思路：每一轮遍历数组，比较相邻的两个数，如果顺序不对就交换。这样每轮结束后，最大的数会"冒泡"到最后面。

想象一下水里的气泡——大的气泡先浮到水面。这就是名字的由来。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int a[1010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    for (int i = 1; i <= n - 1; i++)
    {
        for (int j = 1; j <= n - i; j++)
        {
            if (a[j] > a[j + 1])
            {
                swap(a[j], a[j + 1]);
            }
        }
    }

    for (int i = 1; i <= n; i++)
        cout << a[i] << " ";
    return 0;
}
~~~

时间复杂度：O(n^2)。优点是简单直观，缺点是太慢了。

**优化**：如果某一轮没有发生交换，说明已经有序，可以提前结束。

~~~c++
for (int i = 1; i <= n - 1; i++)
{
    bool swapped = false;
    for (int j = 1; j <= n - i; j++)
    {
        if (a[j] > a[j + 1])
        {
            swap(a[j], a[j + 1]);
            swapped = true;
        }
    }
    if (!swapped) break;
}
~~~

### 2.2 选择排序

选择排序的思路：每一轮从剩余元素中选出最小的，放到已排序部分的末尾。就像整理扑克牌，每次挑最小的那张放左边。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int a[1010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    for (int i = 1; i <= n - 1; i++)
    {
        int minIdx = i;
        for (int j = i + 1; j <= n; j++)
        {
            if (a[j] < a[minIdx])
                minIdx = j;
        }
        swap(a[i], a[minIdx]);
    }

    for (int i = 1; i <= n; i++)
        cout << a[i] << " ";
    return 0;
}
~~~

时间复杂度 O(n^2)。和冒泡相比，交换次数少很多，但比较次数一样。

### 2.3 插入排序

插入排序的思路：像打牌时整理手牌，每次把一张牌插入到已排序部分的正确位置。

~~~c++
#include <iostream>
using namespace std;

int a[1010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    for (int i = 2; i <= n; i++)
    {
        int key = a[i];
        int j = i - 1;
        while (j >= 1 && a[j] > key)
        {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = key;
    }

    for (int i = 1; i <= n; i++)
        cout << a[i] << " ";
    return 0;
}
~~~

最坏 O(n^2)，但如果数据基本有序，可以快到 O(n)。

### 2.4 快速排序（分治思想）

快排是竞赛中最常用的排序，C++ 的 sort 底层就是快排的优化版。

快排思想（分治）：
1. 选一个基准值（pivot）
2. 分区：比基准小的放左边，大的放右边
3. 递归排序左右两边

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int a[100010];

void quickSort(int l, int r)
{
    if (l >= r) return;

    int pivot = a[(l + r) / 2];
    int i = l, j = r;

    while (i <= j)
    {
        while (a[i] < pivot) i++;
        while (a[j] > pivot) j--;
        if (i <= j)
        {
            swap(a[i], a[j]);
            i++;
            j--;
        }
    }

    if (l < j) quickSort(l, j);
    if (i < r) quickSort(i, r);
}

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    quickSort(1, n);

    for (int i = 1; i <= n; i++)
        cout << a[i] << " ";
    return 0;
}
~~~

平均 O(n log n)，最坏 O(n^2)。竞赛中直接用 sort 即可：
~~~c++
#include <algorithm>
sort(a + 1, a + n + 1);
~~~

**练习题**：
- 洛谷 P1177 [【模板】排序](https://www.luogu.com.cn/problem/P1177)
- 洛谷 P1923 [求第k小的数](https://www.luogu.com.cn/problem/P1923)
- 洛谷 P1068 [分数线划定](https://www.luogu.com.cn/problem/P1068)

## 第3章 二分进阶

入门篇我们学了二分查找和二分答案。这一章来深入探索左右边界查找、实数二分和三分。

### 3.1 二分查找模板（左边界/右边界）

当数组中有重复元素时，我们可能需要找某个数第一次或最后一次出现的位置。

**左边界查找**：找第一个 >= target 的位置。

~~~c++
#include <iostream>
using namespace std;

int a[100010];

int lowerBound(int l, int r, int target)
{
    while (l < r)
    {
        int mid = (l + r) / 2;
        if (a[mid] >= target)
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}

int upperBound(int l, int r, int target)
{
    while (l < r)
    {
        int mid = (l + r) / 2;
        if (a[mid] > target)
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}

int main()
{
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    while (m--)
    {
        int x;
        cin >> x;
        int pos = lowerBound(1, n, x);
        if (a[pos] == x)
            cout << pos << " ";
        else
            cout << -1 << " ";
    }
    return 0;
}
~~~

STL中有现成的：
~~~c++
lower_bound(a + 1, a + n + 1, x) - a; // 第一个>=x的位置
upper_bound(a + 1, a + n + 1, x) - a; // 第一个>x的位置
~~~

### 3.2 实数二分

当答案不是整数时，需要在实数范围内二分。比如求方程的根。

~~~c++
#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;

double f(double x)
{
    return x * x * x - x - 1;
}

int main()
{
    double left = 1, right = 2;
    double eps = 1e-7;

    while (right - left > eps)
    {
        double mid = (left + right) / 2;
        if (f(mid) * f(left) <= 0)
            right = mid;
        else
            left = mid;
    }

    cout << fixed << setprecision(6) << left << endl;
    return 0;
}
~~~

如果要求精确到小数点后k位，eps 一般取 10^(-k-2)。

### 3.3 三分

三分用来求凸函数的最大值或凹函数的最小值（单峰/单谷函数）。

思路：在区间中取两个三等分点，比较函数值，排除较差的1/3区间。

~~~c++
#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;

double f(double x)
{
    return -(x - 3) * (x - 3) + 5;
}

int main()
{
    double l = -10, r = 10;
    double eps = 1e-8;

    while (r - l > eps)
    {
        double m1 = l + (r - l) / 3;
        double m2 = r - (r - l) / 3;

        if (f(m1) < f(m2))
            l = m1;
        else
            r = m2;
    }

    cout << fixed << setprecision(6) << f(l) << endl;
    return 0;
}
~~~

**练习题**：
- 洛谷 P2249 [【深基13.例1】查找](https://www.luogu.com.cn/problem/P2249)
- 洛谷 P1024 [一元三次方程求解](https://www.luogu.com.cn/problem/P1024)
- 洛谷 P2678 [跳石头](https://www.luogu.com.cn/problem/P2678)
- 洛谷 P3382 [【模板】三分法](https://www.luogu.com.cn/problem/P3382)

## 第4章 前缀和、差分进阶

入门篇我们学过了一维前缀和和一维差分。这一章我们拓展到二维，同时讲讲更灵活的应用场景。

### 4.1 二维前缀和

二维前缀和用来快速求**子矩阵的和**。

定义：s[i][j] 表示从 (1,1) 到 (i,j) 这个矩形内所有数的和。

递推公式：s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j]

求 (x1,y1) 到 (x2,y2) 的子矩阵和：
sum = s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1-1][y1-1]

这两个公式用了**容斥原理**——画个矩形图就明白了。

~~~c++
#include <iostream>
using namespace std;

int a[1010][1010], s[1010][1010];

int main()
{
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            cin >> a[i][j];

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + a[i][j];

    while (q--)
    {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        int sum = s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1];
        cout << sum << endl;
    }
    return 0;
}
~~~

### 4.2 二维差分

二维差分是二维前缀和的逆运算，用来快速**给子矩阵所有元素加同一个数**。

对子矩阵 (x1,y1) 到 (x2,y2) 加 x：

~~~c++
d[x1][y1] += x;
d[x2 + 1][y1] -= x;
d[x1][y2 + 1] -= x;
d[x2 + 1][y2 + 1] += x;
~~~

最后对差分数组求二维前缀和还原。

~~~c++
#include <iostream>
using namespace std;

int a[1010][1010], d[1010][1010];

int main()
{
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            cin >> a[i][j];

    while (q--)
    {
        int x1, y1, x2, y2, x;
        cin >> x1 >> y1 >> x2 >> y2 >> x;
        d[x1][y1] += x;
        d[x2 + 1][y1] -= x;
        d[x1][y2 + 1] -= x;
        d[x2 + 1][y2 + 1] += x;
    }

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1];
            a[i][j] += d[i][j];
            cout << a[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
~~~

二维的情况就是一维的推广——理解了容斥原理，公式就很好记了。

**练习题**：
- 洛谷 P3397 [地毯](https://www.luogu.com.cn/problem/P3397)
- 洛谷 P2004 [领地选择](https://www.luogu.com.cn/problem/P2004)
- 洛谷 P1719 [最大加权矩形](https://www.luogu.com.cn/problem/P1719)

## 第5章 动态规划进阶

入门篇我们学了01背包、最大子段和等基础DP。这一章深入学习完全背包、多重背包、区间DP和记忆化搜索。

### 5.1 完全背包

01背包中每件物品只能拿一次。**完全背包中每件物品可以拿无限次**。

状态定义：dp[j] = 容量为j的背包能装的最大价值。

转移方程：dp[j] = max(dp[j], dp[j - w[i]] + v[i])

区别在哪？01背包的j从大到小遍历，完全背包的j从小到大遍历。

为什么？因为01背包要保证每件物品只用一次（用旧数据更新），完全背包要保证可以重复使用（用新数据更新）。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int w[1010], v[1010];
int dp[1010];

int main()
{
    int n, C;
    cin >> n >> C;
    for (int i = 1; i <= n; i++)
        cin >> w[i] >> v[i];

    for (int i = 1; i <= n; i++)
    {
        for (int j = w[i]; j <= C; j++) // 注意：从小到大
        {
            dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
        }
    }

    cout << dp[C] << endl;
    return 0;
}
~~~

看到没？和01背包的唯一区别就是内层循环的方向——记住这个区别就够了！

### 5.2 多重背包

多重背包：每件物品有**有限个**（第i件有k[i]个）。

最朴素的做法：把k[i]件物品看成k[i]个独立的01背包物品。如果k[i]很大，会超时。

优化：**二进制拆分**。把k[i]拆成1, 2, 4, 8, ... 2^m, 剩余部分。这样原来要处理k[i]件，现在只需要处理log(k[i])件。

比如k=13，拆成1+2+4+6。这4个数可以组合出0~13的任何数量。

~~~c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int dp[1010];

struct Item { int w, v; };

int main()
{
    int n, C;
    cin >> n >> C;

    vector<Item> items;

    for (int i = 1; i <= n; i++)
    {
        int w, v, k;
        cin >> w >> v >> k;

        for (int t = 1; t <= k; t *= 2)
        {
            items.push_back({w * t, v * t});
            k -= t;
        }
        if (k > 0)
            items.push_back({w * k, v * k});
    }

    for (auto item : items)
    {
        for (int j = C; j >= item.w; j--)
        {
            dp[j] = max(dp[j], dp[j - item.w] + item.v);
        }
    }

    cout << dp[C] << endl;
    return 0;
}
~~~

二进制拆分是多重背包的核心技巧，一定要掌握！

### 5.3 区间DP

区间DP是在**区间上做动态规划**。状态通常定义为 dp[l][r] 表示区间 [l, r] 上的最优解。

经典问题：**石子合并**——n堆石子排成一排，每次合并相邻两堆，代价是两堆石子数之和，求最小总代价。

~~~c++
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;

int a[310], sum[310];
int dp[310][310];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        sum[i] = sum[i - 1] + a[i];
    }

    memset(dp, 0x3f, sizeof(dp));
    for (int i = 1; i <= n; i++)
        dp[i][i] = 0;

    for (int len = 2; len <= n; len++)
    {
        for (int l = 1; l + len - 1 <= n; l++)
        {
            int r = l + len - 1;
            for (int k = l; k < r; k++)
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k + 1][r] + sum[r] - sum[l - 1]);
            }
        }
    }

    cout << dp[1][n] << endl;
    return 0;
}
~~~

区间DP套路：枚举长度 → 枚举左端点 → 枚举分割点 → 合并。

### 5.4 记忆化搜索

记忆化搜索 = 递归 + 记录中间结果。本质就是DP，用递归实现。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

long long memo[100];

long long fib(int n)
{
    if (n <= 2) return 1;
    if (memo[n] != -1) return memo[n];
    return memo[n] = fib(n - 1) + fib(n - 2);
}
~~~

数字三角形的记忆化搜索版：

~~~c++
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;

int a[1010][1010];
int memo[1010][1010];
int n;

int dfs(int i, int j)
{
    if (i > n) return 0;
    if (memo[i][j] != -1) return memo[i][j];
    return memo[i][j] = a[i][j] + max(dfs(i + 1, j), dfs(i + 1, j + 1));
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= i; j++)
            cin >> a[i][j];

    memset(memo, -1, sizeof(memo));
    cout << dfs(1, 1) << endl;
    return 0;
}
~~~

**练习题**：
- 洛谷 P1616 [疯狂的采药](https://www.luogu.com.cn/problem/P1616)（完全背包）
- 洛谷 P1776 [宝物筛选](https://www.luogu.com.cn/problem/P1776)（多重背包）
- 洛谷 P1880 [石子合并](https://www.luogu.com.cn/problem/P1880)
- 洛谷 P1434 [滑雪](https://www.luogu.com.cn/problem/P1434)（记忆化搜索）

## 第6章 图论进阶

入门篇我们学了图的遍历和Floyd最短路算法。这一章深入学习Dijkstra、Bellman-Ford和并查集。

### 6.1 Dijkstra最短路

Dijkstra求**单源最短路**（一个点到所有其他点的最短路径）。它只适用于**没有负权边**的图。

思想：每次从未确定最短路的点中选距离最小的，用这个点去更新其他点的距离。

~~~c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int graph[1010][1010];
int dist[1010];
bool visited[1010];

void dijkstra(int start, int n)
{
    memset(dist, 0x3f, sizeof(dist));
    dist[start] = 0;

    for (int i = 1; i <= n; i++)
    {
        int u = -1;
        for (int j = 1; j <= n; j++)
        {
            if (!visited[j] && (u == -1 || dist[j] < dist[u]))
                u = j;
        }

        if (dist[u] == 0x3f3f3f3f) break;
        visited[u] = true;

        for (int v = 1; v <= n; v++)
        {
            if (!visited[v] && graph[u][v] != 0x3f3f3f3f)
            {
                dist[v] = min(dist[v], dist[u] + graph[u][v]);
            }
        }
    }
}
~~~

上面是O(n^2)的朴素版。用**优先队列**优化到O(m log n)：

~~~c++
#include <iostream>
#include <cstring>
#include <vector>
#include <queue>
using namespace std;

struct Edge { int to, w; };

struct Node
{
    int u, dist;
    bool operator<(const Node &other) const
    {
        return dist > other.dist;
    }
};

vector<Edge> graph[100010];
int dist[100010];
bool visited[100010];

void dijkstra(int start)
{
    memset(dist, 0x3f, sizeof(dist));
    dist[start] = 0;

    priority_queue<Node> pq;
    pq.push({start, 0});

    while (!pq.empty())
    {
        Node cur = pq.top(); pq.pop();
        int u = cur.u;
        if (visited[u]) continue;
        visited[u] = true;

        for (Edge e : graph[u])
        {
            if (dist[e.to] > dist[u] + e.w)
            {
                dist[e.to] = dist[u] + e.w;
                pq.push({e.to, dist[e.to]});
            }
        }
    }
}
~~~

堆优化的Dijkstra能处理n=10万级别的图，是竞赛中最常用的最短路算法。

### 6.2 Bellman-Ford

Bellman-Ford可以处理**负权边**和检测**负环**。

思想：对所有边进行n-1轮松弛操作。第k轮后找到的是最多经过k条边的最短路。如果第n轮还能松弛，说明有负环。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

struct Edge { int u, v, w; };

Edge edges[100010];
int dist[1010];

void bellmanFord(int n, int m, int start)
{
    memset(dist, 0x3f, sizeof(dist));
    dist[start] = 0;

    for (int i = 1; i <= n - 1; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            int u = edges[j].u, v = edges[j].v, w = edges[j].w;
            if (dist[u] != 0x3f3f3f3f && dist[v] > dist[u] + w)
                dist[v] = dist[u] + w;
        }
    }

    // 检测负环
    for (int j = 1; j <= m; j++)
    {
        int u = edges[j].u, v = edges[j].v, w = edges[j].w;
        if (dist[u] != 0x3f3f3f3f && dist[v] > dist[u] + w)
        {
            cout << "存在负环！" << endl;
            return;
        }
    }
}
~~~

复杂度O(n*m)。实际竞赛中更常用SPFA（队列优化版），但SPFA最坏情况会退化，所以无负权时还是用Dijkstra更安全。

### 6.3 并查集

并查集（Union-Find）高效管理元素的所属集合，支持查找和合并操作。

用树表示集合，根节点代表整个集合。

~~~c++
#include <iostream>
using namespace std;

int parent[10010];

void init(int n)
{
    for (int i = 1; i <= n; i++)
        parent[i] = i;
}

// 查找根节点（带路径压缩）
int find(int x)
{
    if (parent[x] != x)
        parent[x] = find(parent[x]);
    return parent[x];
}

void unionSet(int a, int b)
{
    int ra = find(a), rb = find(b);
    if (ra != rb)
        parent[ra] = rb;
}

int main()
{
    int n, m;
    cin >> n >> m;
    init(n);

    while (m--)
    {
        int op, x, y;
        cin >> op >> x >> y;
        if (op == 1)
            unionSet(x, y);
        else
        {
            if (find(x) == find(y))
                cout << "Y" << endl;
            else
                cout << "N" << endl;
        }
    }
    return 0;
}
~~~

**路径压缩**是关键优化——把路径上所有点的父节点直接设为根节点，这样下次查找就快了。

并查集的应用：判断连通性、Kruskal最小生成树、连通块问题等。

**练习题**：
- 洛谷 P4779 [【模板】单源最短路径](https://www.luogu.com.cn/problem/P4779)
- 洛谷 P3385 [【模板】负环](https://www.luogu.com.cn/problem/P3385)
- 洛谷 P3367 [【模板】并查集](https://www.luogu.com.cn/problem/P3367)
- 洛谷 P1551 [亲戚](https://www.luogu.com.cn/problem/P1551)

## 第7章 字符串算法

字符串处理在竞赛中很常见。这一章我们学习KMP匹配、Trie树和字符串哈希——三大字符串利器。

### 7.1 KMP匹配

KMP用来解决**字符串匹配**问题：在文本串中找模式串出现的位置。

暴力做法：从文本串的每个位置开始依次比较，复杂度O(n*m)。

KMP的核心：匹配失败时，利用已经匹配的信息，把模式串向右移动尽可能多的距离。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

void getNext(string pattern, int next[])
{
    int m = pattern.size();
    next[0] = 0;
    int j = 0;

    for (int i = 1; i < m; i++)
    {
        while (j > 0 && pattern[i] != pattern[j])
            j = next[j - 1];

        if (pattern[i] == pattern[j])
            j++;

        next[i] = j;
    }
}

void kmp(string text, string pattern)
{
    int n = text.size(), m = pattern.size();
    int next[1010];
    getNext(pattern, next);

    int j = 0;
    for (int i = 0; i < n; i++)
    {
        while (j > 0 && text[i] != pattern[j])
            j = next[j - 1];

        if (text[i] == pattern[j])
            j++;

        if (j == m)
        {
            cout << "找到匹配，起始位置：" << i - m + 1 << endl;
            j = next[j - 1];
        }
    }
}

int main()
{
    string text = "ABABDABACDABABCABAB";
    string pattern = "ABABCABAB";
    kmp(text, pattern);
    return 0;
}
~~~

next数组是KMP的精髓——记录模式串中"前缀和后缀相同的最长长度"。

### 7.2 Trie树（字典树）

Trie树将字符串集合组织成一棵树，从根到叶子的一条路径就是一个单词。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

struct TrieNode
{
    TrieNode* next[26];
    bool isEnd;
    TrieNode() { memset(next, 0, sizeof(next)); isEnd = false; }
};

TrieNode* root = new TrieNode();

void insert(string word)
{
    TrieNode* node = root;
    for (char c : word)
    {
        int idx = c - 'a';
        if (node->next[idx] == NULL)
            node->next[idx] = new TrieNode();
        node = node->next[idx];
    }
    node->isEnd = true;
}

bool search(string word)
{
    TrieNode* node = root;
    for (char c : word)
    {
        int idx = c - 'a';
        if (node->next[idx] == NULL) return false;
        node = node->next[idx];
    }
    return node->isEnd;
}
~~~

竞赛中更常用**数组实现**，效率更高：

~~~c++
int trie[100010][26];
int cnt[100010];
int idx = 0;

void insert(string s)
{
    int p = 0;
    for (char c : s)
    {
        int u = c - 'a';
        if (trie[p][u] == 0) trie[p][u] = ++idx;
        p = trie[p][u];
    }
    cnt[p]++;
}

int query(string s)
{
    int p = 0;
    for (char c : s)
    {
        int u = c - 'a';
        if (trie[p][u] == 0) return 0;
        p = trie[p][u];
    }
    return cnt[p];
}
~~~

### 7.3 字符串哈希

字符串哈希把字符串映射成一个整数，O(1)判断两个子串是否相等。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

typedef unsigned long long ULL;
const ULL BASE = 131;
ULL h[100010], p[100010];

void init(string s)
{
    int n = s.size();
    p[0] = 1;
    for (int i = 1; i <= n; i++)
    {
        h[i] = h[i - 1] * BASE + s[i - 1];
        p[i] = p[i - 1] * BASE;
    }
}

ULL getHash(int l, int r)
{
    return h[r] - h[l] * p[r - l];
}
~~~

用 `unsigned long long` 自然溢出（自动对2^64取模），效率很高。

**练习题**：
- 洛谷 P3375 [【模板】KMP](https://www.luogu.com.cn/problem/P3375)
- 洛谷 P8306 [【模板】字典树](https://www.luogu.com.cn/problem/P8306)
- 洛谷 P3370 [【模板】字符串哈希](https://www.luogu.com.cn/problem/P3370)

## 第8章 数论基础

数论是数学中研究整数性质的分支，竞赛中经常用到。本章学习快速幂、GCD、扩展欧几里得和素数筛。

### 8.1 快速幂

快速幂用来快速计算 a^b mod m。暴力需要乘b次，快速幂只需要log(b)次。

思想：用指数的二进制表示。比如 3^13，13的二进制是1101，3^13 = 3^8 * 3^4 * 3^1。

~~~c++
#include <iostream>
using namespace std;

long long fastPow(long long a, long long b, long long m)
{
    long long res = 1;
    a %= m;

    while (b > 0)
    {
        if (b & 1)
            res = res * a % m;
        a = a * a % m;
        b >>= 1;
    }

    return res;
}

int main()
{
    long long a, b, m;
    cin >> a >> b >> m;
    cout << fastPow(a, b, m) << endl;
    return 0;
}
~~~

核心两句话：二进制位为1就乘上a，然后a平方、指数右移。超级常用！

### 8.2 最大公约数（辗转相除法封装）

辗转相除法（欧几里得算法）：

~~~c++
int gcd(int a, int b)
{
    return b == 0 ? a : gcd(b, a % b);
}
~~~

最小公倍数：
~~~c++
int lcm(int a, int b)
{
    return a / gcd(a, b) * b; // 先除后乘，防止溢出
}
~~~

C++17 标准库已内置：
~~~c++
#include <numeric>
int g = gcd(a, b);
int l = lcm(a, b);
~~~

### 8.3 扩展欧几里得

扩展欧几里得在求gcd的同时，找出方程 ax + by = gcd(a, b) 的一组整数解。

用于解**同余方程**和**模线性方程**。

~~~c++
#include <iostream>
using namespace std;

int exgcd(int a, int b, int &x, int &y)
{
    if (b == 0)
    {
        x = 1;
        y = 0;
        return a;
    }

    int g = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return g;
}

int main()
{
    int a, b, x, y;
    cin >> a >> b;
    int g = exgcd(a, b, x, y);
    cout << g << " " << x << " " << y << endl;
    return 0;
}
~~~

核心就一行 `exgcd(b, a % b, y, x)` 交换x和y，然后 `y -= a / b * x`。

### 8.4 素数筛

**埃氏筛**：每发现一个素数，标记它的所有倍数为合数。

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

bool isPrime[1000010];

void sieve(int n)
{
    memset(isPrime, true, sizeof(isPrime));
    isPrime[0] = isPrime[1] = false;

    for (int i = 2; i * i <= n; i++)
    {
        if (isPrime[i])
        {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
}
~~~

**欧拉筛（线性筛）**：每个合数只被最小质因子筛掉一次，O(n)。

~~~c++
#include <iostream>
#include <cstring>
#include <vector>
using namespace std;

bool isPrime[10000010];
vector<int> primes;

void linearSieve(int n)
{
    memset(isPrime, true, sizeof(isPrime));
    isPrime[0] = isPrime[1] = false;

    for (int i = 2; i <= n; i++)
    {
        if (isPrime[i])
            primes.push_back(i);

        for (int p : primes)
        {
            if (i * p > n) break;
            isPrime[i * p] = false;
            if (i % p == 0) break;
        }
    }
}
~~~

注意 `if (i % p == 0) break`——保证每个合数只被最小质因子筛一次。

**练习题**：
- 洛谷 P1226 [【模板】快速幂](https://www.luogu.com.cn/problem/P1226)
- 洛谷 P1082 [同余方程](https://www.luogu.com.cn/problem/P1082)
- 洛谷 P3383 [【模板】线性筛素数](https://www.luogu.com.cn/problem/P3383)

## 第9章 STL进阶

入门篇我们简单用过STL的一些容器。这一章系统学习几个最常用的STL容器和算法函数，让你写代码又快又稳。

### 9.1 map

map是**键值对**容器，内部用红黑树实现，按键自动排序。查找、插入、删除O(log n)。

~~~c++
#include <iostream>
#include <map>
using namespace std;

int main()
{
    map<string, int> mp;

    mp["apple"] = 5;
    mp["banana"] = 3;
    mp.insert({"orange", 2});

    if (mp.find("apple") != mp.end())
        cout << "apple: " << mp["apple"] << endl;

    for (auto &p : mp)
        cout << p.first << ": " << p.second << endl;

    mp.erase("banana");

    if (mp.count("orange") > 0)
        cout << "orange exists" << endl;

    return 0;
}
~~~

`unordered_map` 是哈希表实现，查找O(1)但无序：
~~~c++
#include <unordered_map>
unordered_map<string, int> ump;
~~~

### 9.2 set

set是集合容器，元素唯一且自动排序。

~~~c++
#include <iostream>
#include <set>
using namespace std;

int main()
{
    set<int> st;

    st.insert(3);
    st.insert(1);
    st.insert(4);
    st.insert(1);

    for (int x : st)
        cout << x << " "; // 1 3 4

    if (st.find(3) != st.end())
        cout << "找到了" << endl;

    st.erase(3);

    auto it = st.lower_bound(2);
    if (it != st.end())
        cout << *it << endl;

    return 0;
}
~~~

### 9.3 pair

pair把两个值捆绑在一起。

~~~c++
#include <iostream>
#include <utility>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
    pair<int, int> p1 = {3, 5};
    cout << p1.first << " " << p1.second << endl;

    vector<pair<int, int>> v = {{3, 1}, {1, 5}, {2, 3}};
    sort(v.begin(), v.end());

    for (auto &p : v)
        cout << p.first << "," << p.second << " ";

    return 0;
}
~~~

### 9.4 priority_queue

优先队列（堆），默认大顶堆。

~~~c++
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    priority_queue<int> pq;
    pq.push(3);
    pq.push(1);
    pq.push(5);
    cout << pq.top() << endl; // 5

    // 小顶堆
    priority_queue<int, vector<int>, greater<int>> minHeap;
    minHeap.push(3);
    minHeap.push(1);
    minHeap.push(5);
    cout << minHeap.top() << endl; // 1

    return 0;
}
~~~

### 9.5 vector

vector是动态数组，最常用的容器。

~~~c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
    vector<int> v;

    v.push_back(10);
    v.push_back(20);

    cout << v.size() << endl;
    cout << v[0] << endl;

    v.pop_back();

    for (int x : v) cout << x << " ";

    sort(v.begin(), v.end());

    v.clear();

    vector<int> v2(100, 0);

    return 0;
}
~~~

### 9.6 algorithm常用函数

~~~c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
    int a[] = {3, 1, 4, 1, 5, 9, 2, 6};
    int n = 8;

    sort(a, a + n);
    reverse(a, a + n);

    auto it = unique(a, a + n);
    int newLen = it - a;

    if (binary_search(a, a + n, 5))
        cout << "找到了" << endl;

    int *p = lower_bound(a, a + n, 3);
    int *q = upper_bound(a, a + n, 3);

    int mx = *max_element(a, a + n);
    int mn = *min_element(a, a + n);

    fill(a, a + n, 0);
    swap(a[0], a[1]);

    vector<int> v = {3, 1, 4, 1, 5};
    sort(v.begin(), v.end());

    return 0;
}
~~~

**练习题**：
- 洛谷 P1106 [删数问题](https://www.luogu.com.cn/problem/P1106)
- 洛谷 P1090 [合并果子](https://www.luogu.com.cn/problem/P1090)

## 第10章 常见优化技巧

竞赛中时间和空间复杂度同样重要。这一章学习滚动数组、状态压缩和尺取法。

### 10.1 滚动数组

滚动数组用来**压缩空间**。当DP的状态只依赖前几项时，可以只保留需要的项。

斐波那契数列只需要两个变量：

~~~c++
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    if (n <= 2) { cout << 1 << endl; return 0; }

    int a = 1, b = 1, c;
    for (int i = 3; i <= n; i++)
    {
        c = a + b;
        a = b;
        b = c;
    }

    cout << c << endl;
    return 0;
}
~~~

01背包的一维优化也是滚动数组：

~~~c++
int dp[1010];
for (int i = 1; i <= n; i++)
    for (int j = C; j >= w[i]; j--)
        dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
~~~

从二维到一维，空间从O(n*C)降到O(C)，效果显著。

### 10.2 状态压缩

状态压缩（状压DP）用**二进制位**表示状态。比如8个位置的开关状态，用一个8位二进制数表示。

典型应用：**旅行商问题（TSP）**。

~~~c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int dist[20][20];
int dp[1 << 16][16];
int n;

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> dist[i][j];

    memset(dp, 0x3f, sizeof(dp));
    dp[1][0] = 0;

    for (int mask = 1; mask < (1 << n); mask++)
    {
        for (int i = 0; i < n; i++)
        {
            if (!(mask & (1 << i))) continue;
            if (dp[mask][i] == 0x3f3f3f3f) continue;

            for (int j = 0; j < n; j++)
            {
                if (mask & (1 << j)) continue;
                int newMask = mask | (1 << j);
                dp[newMask][j] = min(dp[newMask][j], dp[mask][i] + dist[i][j]);
            }
        }
    }

    cout << dp[(1 << n) - 1][n - 1] << endl;
    return 0;
}
~~~

关键：用二进制数表示集合，位运算操作集合。n一般不超过20。

### 10.3 尺取法（双指针）

尺取法用两个指针维护一个区间，高效解决连续子区间问题。

**最短子数组和**——求数组中和 >= target 的最短连续子数组长度。

~~~c++
#include <iostream>
#include <algorithm>
using namespace std;

int a[100010];

int main()
{
    int n, target;
    cin >> n >> target;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    int left = 1, sum = 0;
    int ans = n + 1;

    for (int right = 1; right <= n; right++)
    {
        sum += a[right];

        while (sum >= target)
        {
            ans = min(ans, right - left + 1);
            sum -= a[left];
            left++;
        }
    }

    if (ans > n) cout << 0 << endl;
    else cout << ans << endl;

    return 0;
}
~~~

**最长无重复子串**：

~~~c++
#include <iostream>
#include <cstring>
using namespace std;

int cnt[256];

int main()
{
    string s;
    cin >> s;

    int left = 0, ans = 0;

    for (int right = 0; right < s.size(); right++)
    {
        cnt[s[right]]++;

        while (cnt[s[right]] > 1)
        {
            cnt[s[left]]--;
            left++;
        }

        ans = max(ans, right - left + 1);
    }

    cout << ans << endl;
    return 0;
}
~~~

尺取法核心：右指针前进，左指针在条件不满足时跟进。每个元素最多访问两次，复杂度O(n)。

**练习题**：
- 洛谷 P1631 [序列合并](https://www.luogu.com.cn/problem/P1631)
- 洛谷 P1886 [滑动窗口 / 【模板】单调队列](https://www.luogu.com.cn/problem/P1886)
- 洛谷 P1102 [A-B 数对](https://www.luogu.com.cn/problem/P1102)

---

恭喜你完成了进阶篇的学习！从高精度到排序、从二分到DP、从图论到字符串、从数论到STL、再到各种优化技巧——你现在已经掌握了竞赛中大部分常用算法。接下来你可以去刷更多的题目巩固，或者学习更高级的内容如线段树、树链剖分、网络流等。加油！
