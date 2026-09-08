---
title: 第六课 分支结构嵌套与逻辑运算符
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 1177
date: 2026-01-01 16:28:57
---

# 第一阶段 第六课 分支结构嵌套与逻辑运算符

## 8006：判断闰年

{% hideToggle 提示%}

```cpp
分支结构中非常经典的问题
四年一润百米不润，四百年在润
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

    if (n % 4 == 0 && n % 100 != 0 || n % 400 == 0)
    {
        cout << "yes";
    }
    else
    {
        cout << "no";
    }

    return 0;
}
```

{% endhideToggle %}



## 8007：判断是否为两位数

{% hideToggle 提示%}

```cpp
基础判断
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

    if (n >= 10 && n <= 99)
    {
        cout << 1;
    }
    else
    {
        cout << 0;
    }

    return 0;
}
```

{% endhideToggle %}



## 8008：判断一个数能否同时被3和5整除

{% hideToggle 提示%}

```cpp
基础判断
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

    if (n % 3 == 0 && n % 5 == 0)
    {
        cout << "yes";
    }
    else
    {
        cout << "no";
    }

    return 0;
}
```

{% endhideToggle %}



## 8009：三个数

{% hideToggle 提示%}

```cpp
可以用 if 但是太麻烦
先求和 在求最大最小值
和 - 最大 - 最小 = 中间
也可以使用排序算法
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    double a, b, c;
    double ma, mi, md, sum;
    cin >> a >> b >> c;

    ma = max(max(a, b), c);
    mi = min(min(a, b), c);
    sum = a + b + c;
    md = sum - ma - mi;

    cout << fixed << setprecision(2);
    cout << mi << " ";
    cout << md << " ";
    cout << ma << " " << endl;

    return 0;
}
```

{% endhideToggle %}



## 8010：有一门课不及格的学生

{% hideToggle 提示%}

```cpp
注意题目 是仅有一门不及格
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int a, b;
	cin >> a >> b;
	
	cout << (a < 60 && b > 60 || a > 60 && b < 60 ? 1 : 0);

	return 0;
}

```

{% endhideToggle %}



## 8011：星期几

{% hideToggle 提示%}

```cpp
可以使用if 或者 switch 字符串也行
```

{% endhideToggle %}

{% hideToggle 题解1%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int a;
	cin >> a;
    
	if (a == 1)
	{
		cout << "Monday";
	}
	else if (a == 2)
	{
		cout << "Tuesday";
	}
	else if(a == 3)
	{
		cout << "Wednesday";
	}
	else if(a == 4)
	{
		cout << "Thursday";
	}
	else if (a == 5)
	{
		cout << "Friday";
	}
	else if(a == 6)
	{
		cout << "Saturday";
	}
	else if(a == 7)
	{
		cout << "Sunday";
	}
	
	return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解2%}

```cpp
#include <iostream>

using namespace std;

string s[10] = {"", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"};

int main()
{
    int n;
    cin >> n;
    
    cout << s[n];

    return 0;
}
```

{% endhideToggle %}



## 8012：肥胖问题

{% hideToggle 提示%}

```cpp
基础判断
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    double m, h, bmi;
    cin >> m >> h;
    
    bmi = m / (h * h);
    if (bmi < 18.5)
    {
        cout << "Underweight";
    }
    else if (bmi < 24)
    {
        cout << "Normal";
    }
    else
    {
        cout << bmi << endl;
        cout << "Overweight";
    }

    return 0;
}
```

{% endhideToggle %}



## 8013：最大的数

{% hideToggle 提示%}

```cpp
基础判断
或者巧用max()
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a, b, c;

    cin >> a >> b >> c;
    cout << max(max(a, b), c);

    return 0;
}
```

{% endhideToggle %}



## 8014：收集瓶盖赢大奖

{% hideToggle 提示%}

```cpp
基础判断
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a, b;
    cin >> a >> b;

    if (a >= 10 || b >= 20)
    {
        cout << 1;
    }
    else
    {
        cout << 0;
    }

    return 0;
}
```

{% endhideToggle %}



## 8015：晶晶赴约会

{% hideToggle 提示%}

```cpp
基础判断
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

    if (n == 1 || n == 3 || n == 5)
    {
        cout << "NO";
    }
    else
    {
        cout << "YES";
    }

    return 0;
}
```

{% endhideToggle %}



## 8016：买铅笔

{% hideToggle 提示%}

```cpp
基础判断
分类讨论
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <cmath>
#include <iostream>

using namespace std;

int main()
{
    int n, a, aj, b, bj, c, cj, as, bs, cs;
    cin >> n >> a >> aj >> b >> bj >> c >> cj;
    
    as = ceil(n * 1.0 / a) * aj;
    bs = ceil(n * 1.0 / b) * bj;
    cs = ceil(n * 1.0 / c) * cj;

    cout << min(min(as, bs), cs);

    return 0;
}
```

{% endhideToggle %}



## 8017：月份天数

{% hideToggle 提示%}

```cpp
正常应该用switch - case
注意 年份判断平年闰年 二月28， 29
也可以使用数组
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

int main()
{
    int y, m;
    cin >> y >> m;
    
    if (y % 4 == 0 && y % 100 != 0 || y % 400 == 0)
    {
        a[2] = 29;
    }

    cout << a[m];

    return 0;
}
```

{% endhideToggle %}



## 8018：简单计算器

{% hideToggle 提示%}

```cpp
适合固定值匹配（整数、字符），不适合范围判断
每个 case 要有 break，除非你有意让代码贯穿
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a, b, ans;
    char c;
    cin >> a >> b >> c;

    switch (c)
    {
    case '+':
        cout << a + b;
        break;
    case '-':
        cout << a - b;
        break;
    case '*':
        cout << a * b;
        break;
    case '/':
        if (b == 0)
        {
            cout << "Divided by zero!";
        }
        else
        {
            cout << a / b;
        }
        break;
    default:
        cout << "Invalid operator!";
        break;
    }

    return 0;
}
```

{% endhideToggle %}



## 8019：小玉家的电费

{% hideToggle 提示%}

```cpp
分段计费，注意题目要求是超出的部分
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

    if (n <= 150)
    {
        sum = 0.4463 * n;
    }
    else if (n <= 400)
    {
        sum = (0.4463 * 150) + (n - 150) * 0.4663;
    }
    else
    {
        sum = (0.4463 * 150) + 250 * 0.4663 + (n - 400) * 0.5663;
    }
    cout << fixed << setprecision(1) << sum;

    return 0;
}
```

{% endhideToggle %}

