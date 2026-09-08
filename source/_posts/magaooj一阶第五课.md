---
title: 第五课 分支结构
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 17185
date: 2026-01-01 17:28:48
---

# 第一阶段 第五课 分支结构

## 6007：输出绝对值

{% hideToggle 提示%}

```cpp
可以使用计算
也可以使用 <cmath> 中的 fabs()
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
	double a;
	cin >> a;
	
	if (a < 0)
	{
		a = 0 - a; 
        // a = a * -1;
        // a = -a;
	}
	
	cout << fixed << setprecision(2) << a;

	return 0;
}
```

{% endhideToggle %}



## 6008：苹果和虫子

{% hideToggle 提示%}

```cpp
数据大超过int
考虑多吃的情况
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    long long n, x, y, cnt;
    cin >> n >> x >> y;

    cnt = y / x;
    if (y % x)
    {
        cnt++;
    }
    n -= cnt;

    if (n < 0)
    {
        cout << 0;
    }
    else
    {
        cout << n;
    }

    return 0;
}
```

{% endhideToggle %}



## 6009：整数大小比较

{% hideToggle 提示%}

```cpp
else-if 应用
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int x, y;
	cin >> x >> y;
	
	if (x > y)
    {
		cout << ">";
    }
	else if (x < y)
    {
		cout << "<";
    }
	else
    {
		cout << "=";
    }

	return 0;
}

```

{% endhideToggle %}



## 6010：判断数正负

{% hideToggle 提示%}

```cpp
else-if 应用
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    if (n > 0)
    {
        cout << "positive";
    }
    else if (n == 0)
    {
        cout << "zero";
    }
    else
    {
        cout << "negative";
    }

    return 0;
}
```

{% endhideToggle %}



## 6011：判断奇偶数

{% hideToggle 提示%}

```cpp
注意输出空格
```

{% endhideToggle %}

{% hideToggle 题解1%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    if (n % 2)
    {
        cout << "n o";
    }
    else
    {
        cout << "y e s";
    }

    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解2%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    cout << (n % 2 ? "n o" : "y e s");

    return 0;
}
```

{% endhideToggle %}

## 6012：适合晨练

{% hideToggle 提示%}

```cpp
if - else 或 三目运算符
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    cout << (n >= 25 && n <= 30 ? "ok!" : "no!");

    return 0;
}
```

{% endhideToggle %}



## 6013：骑车与走路

{% hideToggle 提示%}

```cpp
速度 * 时间 = 路程
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    double x, b, w;
    cin >> x;

    b = 23 + 27 + x / 1.2;
    w = x / 3;

    if (b > w)
    {
        cout << "Bike";
    }
    else if (b == w)
    {
        cout << "All";
    }
    else
    {
        cout << "Walk";
    }

    return 0;
}
```

{% endhideToggle %}



## 6014：收费

{% hideToggle 提示%}

```cpp
分支应用
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    int n;
    double sum;
    cin >> n;
    
    if (n <= 20)
    {
        sum = n * 1.68;
    }
    else
    {
        sum = n * 1.98;
    }

    cout << fixed << setprecision(2) << sum;

    return 0;
}
```

{% endhideToggle %}



## 6015：计算邮资

{% hideToggle 提示%}

```cpp
多种方式
先计算 邮费 再 考虑是否加急
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, cnt = 0, ans;
    char c;
    cin >> n >> c;

    n -= 1000;
    if (n > 0)
    {
        cnt = n / 500;
        if (n % 500)
        {
            cnt++;
        }
    }
    cnt *= 4;

    ans = 8 + cnt;
    if (c == 'y')
    {
        ans += 5;
    }

    cout << ans;

    return 0;
}
```

{% endhideToggle %}



## 6016：范围判断

{% hideToggle 提示%}

```cpp
c++中没有连续不等式 如 1 < n < 100
必须要用 逻辑运算符 链接 n > 1 && n < 100
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    if (n > 1 && n < 100)
    {
        cout << "yes";
    }

    return 0;
}
```

{% endhideToggle %}



## 6017：枚举结构

{% hideToggle 提示%}

```cpp
基础模拟题目
答案都在题目里面
注意数据范围
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <cctype>
#include <iostream>

using namespace std;

int main()
{
    long long y, w, ans = -1;
    char x, z;
    cin >> x >> y >> z >> w;

    if (islower(x) && islower(z) && x == z)
    {
        ans = (y <= w ? w - y + 1 : y - w + 1);
        cout << "valid" << '\n';
    }
    else
    {
        cout << "Invalid" << '\n';
    }

    cout << ans;

    return 0;
}
```

{% endhideToggle %}

