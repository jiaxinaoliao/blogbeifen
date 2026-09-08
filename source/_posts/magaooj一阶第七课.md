---
title: 第七课 循环 累加累乘与最值
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 58417
date: 2026-01-01 15:29:06
---

# 第一阶段 第七课 循环 累加累乘与最值

## 6029：最大跨度值

{% hideToggle 提示%}

```cpp
典型例题
利用打擂台的方法
后期学习数组之后可以用排序
```

{% endhideToggle %}

{% hideToggle 题解1%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int ma, mi, n, x;
	cin >> n >> x;
    
	ma = mi = x
	for(int i = 2; i <= n; i++)
    {
		cin >> x;
		if(x < mi)
        {
			mi = x;
		}
		if(x > ma)
        {
			ma = x;
		}
	}
    
	cout << ma - mi;
    
	return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解2%}

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int a[1010];

int main()
{
    int n;
    cin >> n;

    for (int i = 1; i <= n; i++) 
    {
        cin >> a[i]; 
    }

    sort(a + 1, a + n + 1);
    int ans = a[n] - a[1];

    cout << ans;

    return 0;
}
```

{% endhideToggle %}



## 6259：银行利息

{% hideToggle 提示%}

```cpp
数学计算
注意题目要求 要整数部分
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    double r, m, y;
    cin >> r >> m >> y;

    for (int i = 1; i <= y; i++)
    {
        m = m * (1 + r / 100);
    }

    cout << (int)m << endl;

    return 0;
}
```

{% endhideToggle %}



## 6258：分苹果

{% hideToggle 提示%}

```cpp
累加求和
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

    int sum = 0;
    for (int i = 1; i <= n; i++)
    {
                sum += i;
    }
    
    cout << sum << endl;

    return 0;
}			
```

{% endhideToggle %}



## 6257：3721数

{% hideToggle 提示%}

```cpp
分支循环嵌套
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    for (int i = 1; i <= 200; i++)
    {
        if (i % 3 == 2 && i % 7 == 1)
        {
            cout << i << " ";
        }
    }

    return 0;
}
```

{% endhideToggle %}



## 6256：输出 1 到 n 之间的所有奇数

{% hideToggle 提示%}

```cpp
可以使用嵌套
也可以改变循环条件 推荐
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

    for (int i = 1; i <= n; i += 2)
    {
        cout << i << " ";
    }

    return 0;
}
```

{% endhideToggle %}



## 6255：输出 n 到 1 之间的所有数

{% hideToggle 提示%}

```cpp
逆序
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

    for (int i = n; i >= 1; i--)
    {
        cout << i << " ";
    }

    return 0;
}
```

{% endhideToggle %}



## 6254：输出 1 到 n 之间的所有数

{% hideToggle 提示%}

```cpp
正序 遍历
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

    for (int i = 1; i <= n; i++)
    {
        cout << i << " ";
    }

    return 0;
}
```

{% endhideToggle %}



## 6253：输出 1 到 100 之间的所有数

{% hideToggle 提示%}

```cpp
基础 for
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    n = 100;

    for (int i = 1; i <= n; i++)
    {
        cout << i << " ";
    }

    return 0;
}
```

{% endhideToggle %}



## 6252：津津的储蓄计划

{% hideToggle 提示%}

```cpp
逻辑梳理：
每月先收入300
减去当月开销
把手中整百的钱存入银行（利息20%）
如果某个月钱不够花 → 输出负数月份并结束
年底：存款×1.2 + 手里剩下的钱
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int s = 0, ans = 0;
    
    for (int i = 1; i <= 12; i++)
    {
        int a;
        cin >> a;
        s += 300;
        if (s >= a)
        {
            s -= a;
            ans += s / 100 * 100;
            s -= s / 100 * 100;
        }
        else
        {
            cout << -i << endl;
            return 0;
        }
    }

    ans = ans * 1.2 + s;
    cout << ans;

    return 0;
}
```

{% endhideToggle %}



## 6032：计算分数加减表达式的值

{% hideToggle 提示%}

```cpp

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
    cin >> n;

    double sum = 0;
    for (int i = 1; i <= n; i++)
    {
        if (i % 2)
        {
            sum += 1.0 / i;
        }
        else
        {
            sum += -1.0 / i;
        }
    }
    
    cout << fixed << setprecision(4) << sum;

    return 0;
}
```

{% endhideToggle %}



## 6031：打分

{% hideToggle 提示%}

```cpp

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
    cin >> n;

    double sum = 0;
    int ma = 0, mi = 999;
    for (int i = 1; i <= n; i++)
    {
        int a;
        cin >> a;
        if (a > ma)
        {
            ma = a;
        }
        if (a < mi)
        {
            mi = a;
        }
        sum += a;
    }
    sum = sum - ma - mi;
    sum = sum / (n - 2);
    
    cout << fixed << setprecision(2) << sum;

    return 0;
}
```

{% endhideToggle %}



## 6030：找数组的最大数

{% hideToggle 提示%}

```cpp
经典打擂台
注意 ma 的取值
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[110];

int main()
{
    int n, ma = 0;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
        if (a[i] > ma)
        {
            ma = a[i];
        }
    }
    cout << ma;

    return 0;
}
```

{% endhideToggle %}



## 6018：输出1到n的偶数

{% hideToggle 提示%}

```cpp
这是使用循环嵌套的写法
刚才是使用改变循环次数的写法
这种写法循环次数会多
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

    for (int i = 1; i <= n; i++)
    {
        if (i % 2 == 0)
        {
            cout << i << endl;
        }
    }

    return 0;
}
```

{% endhideToggle %}



## 6028：求最高分最低分

{% hideToggle 提示%}

```cpp
经典打擂台
最大值 ma 赋值为最小
最小值 mi 赋值为最大
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

    int ma = 0, mi = 9999;
    for (int i = 1; i <= n; i++)
    {
        int a;
        cin >> a;
        if (a > ma)
        {
            ma = a;
        }
        if (a < mi)
        {
            mi = a;
        }
    }

    cout << ma << " " << mi;

    return 0;
}
```

{% endhideToggle %}



## 6027：查找10的位置

{% hideToggle 提示%}

```cpp
嵌套 查找
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a;
    bool f = false;
    
    for (int i = 1; i <= 20; i++)
    {
        cin >> a;
        if (a == 10)
        {
            cout << i;
            f = true;
            break;
        }
    }
    
    if (!f)
    {
        cout << 0;
    }

    return 0;
}
```

{% endhideToggle %}



## 6026：人口增长问题

{% hideToggle 提示%}

```cpp
数学计算
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
    double s = 0;
    cin >> s >> n;
    
    for (int i = 1; i <= n; i++)
    {
        s *= 1.001;
    }

    cout << fixed << setprecision(4) << s;

    return 0;
}
```

{% endhideToggle %}



## 6025：乘方计算

{% hideToggle 提示%}

```cpp
注意使用pow需要类型转换
```

{% endhideToggle %}

{% hideToggle 题解1%}

```cpp
#include <iostream>

using namespace std;

int main() 
{
	int a, n;
    cin >> a >> n;
    
    int b = 1;
    for(int i = 1; i <= n; i++)
    {
        b = b * a;
    }
    
    cout << b;

	return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解2%}

```cpp
#include <iostream>
#include <cmath>

using namespace std;

int main()
{
    int a, n;
    cin >> a >> n;

    cout << (int)pow(a, n);

    return 0;
}
```

{% endhideToggle %}



## 6024：求阶乘的和

{% hideToggle 提示%}

```cpp
注意数据范围
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    long long n, sum = 0, a;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        a = 1;
        for (int j = 1; j <= i; j++)
        {
            a *= j;
        }
        sum += a;
    }

    cout << sum;

    return 0;
}
```

{% endhideToggle %}



## 6023：求平均年龄

{% hideToggle 提示%}

```cpp
累加
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    int n, a, sum = 0;
    double ava;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        cin >> a;
        sum += a;
    }
    ava = sum * 1.0 / n;

    cout << fixed << setprecision(2) << ava;

    return 0;
}
```

{% endhideToggle %}



## 6022：满足条件的数累加

{% hideToggle 提示%}

```cpp
条件 累加
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, m, sum = 0;
    cin >> n >> m;

    for (int i = n; i <= m; i++)
    {
        if (i % 17 == 0)
        {
            sum += i;
        }
    }

    cout << sum;

    return 0;
}
```

{% endhideToggle %}



## 6021：求整数的和与均值

{% hideToggle 提示%}

```cpp
累加
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    int n, a, sum = 0;
    double ava;
    cin >> n;
    
    for (int i = 1; i <= n; i++)
    {
        cin >> a;
        sum += a;
    }

    ava = sum * 1.0 / n;

    cout << sum << " " << fixed << setprecision(5) << ava;

    return 0;
}
```

{% endhideToggle %}



## 6020：奇数求和

{% hideToggle 提示%}

```cpp
条件累加
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, m, sum = 0;
    cin >> n >> m;

    for (int i = n; i <= m; i++)
    {
        if (i % 2)
        {
            sum += i;
        }
    }

    cout << sum;

    return 0;
}
```

{% endhideToggle %}



## 6019：求和

{% hideToggle 提示%}

```cpp
可以利用高斯公式
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, sum = 0;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        sum += i;
    }

    cout << sum;

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

    cout << (1 + n) * n / 2;

    return 0;
}
```

{% endhideToggle %}
