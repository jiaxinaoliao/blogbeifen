---
title: 第八课 for循环计数
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 4708
date: 2026-01-01 14:30:24
---

# 第一阶段 第八课 for循环计数

## 7018：整数的个数

{% hideToggle 提示%}

```cpp
for计数
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int k, x, cnt1, cnt5, cnt10;
	cnt1 = cnt5 = cnt10 = 0;
	
	cin >> k;
	
	for (int i = 1; i <= k; i++)
	{
		cin >> x;
		if (x == 1)
		{
			cnt1++;
		}
		else if (x == 5)
		{
			cnt5++;
		}
		else if (x == 10)
		{
			cnt10++;
		}
	} 
	
	cout << cnt1 << '\n';
	cout << cnt5 << '\n';
	cout << cnt10 << '\n';

	return 0;
}

```

{% endhideToggle %}



## 7019：与指定数字相同的数的个数

{% hideToggle 提示%}

```cpp
需要大量存储 则用数组
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[110];

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
    }
    int x;
    cin >> x;
    
    int count = 0;
    for (int i = 0; i < n; i++)
    {
        if (a[i] == x)
        {
            count++;
        }
    }

    cout << count;

    return 0;
}
```

{% endhideToggle %}



## 7020：正常血压

{% hideToggle 提示%}

```cpp
计数 打擂台
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

    int cnt = 0, ma = 0;
    for (int i = 1; i <= n; i++)
    {
        int a, b;
        cin >> a >> b;
        if (a >= 90 && a <= 140 && b >= 60 && b <= 90)
        {
            cnt++;
        }
        else
        {
            cnt = 0;
        }
        if (cnt > ma)
        {
            ma = cnt;
        }
    }

    cout << ma;

    return 0;
}
```

{% endhideToggle %}



## 7021：统计满足条件的4位数

{% hideToggle 提示%}

```cpp
计数
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

    int cnt = 0;
    for (int i = 1; i <= n; i++)
    {
        int a;
        cin >> a;
        if (a % 10 - a / 1000 - a % 1000 / 100 - a % 100 / 10 > 0)
        {
            cnt++;
        }
    }

    cout << cnt;

    return 0;
}
```

{% endhideToggle %}



## 7022：药房管理

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, m;
    cin >> m >> n;

    int cnt = 0;
    for (int i = 1; i <= n; i++)
    {
        int a;
        cin >> a;
        if (a > m)
        {
            cnt++;
        }
        else
        {
            m -= a;
        }
    }

    cout << cnt;

    return 0;
}
```

{% endhideToggle %}



## 7023：奥运奖牌计数

{% hideToggle 提示%}

```cpp
字符建议使用单引号
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

    int a, b, c;
    int ca, cb, cc, sc;
    ca = cb = cc = sc = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> a >> b >> c;
        ca += a, cb += b, cc += c;
        sc += a + b + c;
    }

    cout << ca << ' ';
    cout << cb << ' ';
    cout << cc << ' ';
    cout << sc << '\n';

    return 0;
}
```

{% endhideToggle %}



## 7024：春游

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[1010];

int main()
{
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= m; i++)
    {
        int x;
        cin >> x;
        a[x] = true;
    }

    bool f = true;
    for (int i = 0; i < n; i++)
    {
        if (a[i] == false)
        {
            cout << i << " ";
            f = false;
        }
    }
    if (f)
    {
        cout << n;
    }

    return 0;
}
```

{% endhideToggle %}



## 7025：小杨的储蓄

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[1010];

int main()
{
    int n, d;
    cin >> n >> d;
    for (int i = 1; i <= d; i++)
    {
        int x;
        cin >> x;
        a[x] += i;
    }

    for (int i = 0; i< n;i++)
    {
        cout << a[i] << " ";
    }

    return 0;
}
```

{% endhideToggle %}



## 7026：奇偶分家

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, odd, even;
    odd = even = 0;
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        int x;
        cin >> x;
        if (x%2)
        {
            odd++;
        }
        else
        {
            even++;
        }
    }

    cout << odd << " " << even << '\n';

    return 0;
}
```

{% endhideToggle %}



## 7027：数值统计

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int n, a, b, c;
    a = b = c = 0;
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        int x;
        cin >> x;
        if (x < 0)
        {
            a++;
        }
        else if (x == 0)
        {
            b++;
        }
        else
        {
            c++;
        }
    }

    cout << a << " " << b << " " << c << endl;

    return 0;
}
```

{% endhideToggle %}



## 7028：证书等级

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a, n, s;
    cin >> s >> n;

    int cnt = 1;
    for (int i = 1; i < n; i++)
    {
        cin >> a;
        if (a > s)
        {
            cnt++;
        }
    }

    if (cnt <= n * 0.1)
    {
        cout << 'A';
    }
    else if (cnt <= n * 0.3)
    {
        cout << 'B';
    }
    else if (cnt <= n * 0.6)
    {
        cout << 'C';
    }
    else if (cnt <= n * 0.8)
    {
        cout << 'D';
    }
    else
    {
        cout << 'E';
    }

    return 0;
}
```

{% endhideToggle %}



## 7029：短信计费

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

    double s = 0;
    int a, cnt = 0;
    for (int i = 1; i <= n; i++)
    {
        cin >> a;

        cnt += a / 70;
        if (a % 70)
        {
            cnt++;
        }
    }
    s = cnt * 0.1;

    cout << fixed << setprecision(1) << s;

    return 0;
}
```

{% endhideToggle %}



## 7030：石头剪子布

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
    int n, y, k;
    y = k = 0;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {
        char a, b;
        cin >> a >> b;
        if (a == 'S' && b == 'J' || a == 'J' && b == 'B' || a == 'B' && b == 'S')
        {
            y++;
        }
        else if (b == 'S' && a == 'J' || b == 'J' && a == 'B' || b == 'B' && a == 'S')
        {
            k++;
        }
    }
    
    if (y > k)
    {
        cout << "xiaoyan" << endl;
    }
    else if (y < k)
    {
        cout << "xiaoke" << endl;
    }
    else
    {
        cout << "QAQ" << endl;
    }

    return 0;
}
```

{% endhideToggle %}



## 7031：最长连号

{% hideToggle 提示%}

```cpp

```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int a[10010];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
    }

    int cnt = 1, ma = 1;
    for (int i = 2; i <= n; i++)
    {
        if (a[i] == a[i - 1] + 1)
        {
            cnt++;
            if (cnt > ma)
            {
                ma = cnt;
            }
        }
        else
        {
            cnt = 1;
        }
    }

    cout << ma;

    return 0;
}
```

{% endhideToggle %}

