---
title: 第四课 算术运算符02
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 20647
date: 2026-01-01 18:57:55
---

# 第一阶段 第四课 算术运算符02

## 7010：当天的第几秒

{% hideToggle 提示%}

```cpp
基础运算 时间单位转换
统一单位
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int h, m, s, ans;
	char c;
	cin >> h >> m >> s >> c;
	
	ans = h * 3600 + m * 60 + s;
	
	if (c == 'P')
    {
		ans += 12 * 3600;
    }
		
	cout << ans;

	return 0;
}

```

{% endhideToggle %}



## 7012：时间规划

{% hideToggle 提示%}

```cpp
基础运算 时间单位转换
统一单位
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int sh, sm, eh, em, ah, am, ans;
    cin >> sh >> sm >> eh >> em;

    ah = sh * 60 + sm;
    am = eh * 60 + em;
    ans = am - ah;
    
    cout << ans;

    return 0;
}
```

{% endhideToggle %}



## 7013：休息时间

{% hideToggle 提示%}

```cpp
基础运算 时间单位转换
统一单位
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int h, m, s, k, eh, em, es, sum;
    cin >> h >> m >> s >> k;
    
    sum = 3600 * h + 60 * m + s + k;
    es = sum % 3600 % 60;
    em = sum % 3600 / 60;
    eh = sum / 3600;
    
    cout << eh << " " << em << " " << es;

    return 0;
}
```

{% endhideToggle %}



## 7014：甲流疫情死亡率

{% hideToggle 提示%}

```cpp
百分比的使用
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
    cin >> a >> b >> c;
    
    c = b / a * 100;

    cout << fixed << setprecision(3) << c << "%";

    return 0;
}
```

{% endhideToggle %}



## 7015：温度表达转化

{% hideToggle 提示%}

```cpp
按照题目要求书写公式
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    double f, c;
    cin >> f;
    
    c = 5 * (f - 32) / 9;

    cout << fixed << setprecision(5) << c;

    return 0;
}
```

{% endhideToggle %}



## 7016：大象喝水查

{% hideToggle 提示%}

```cpp
圆柱体积等于 底面积 x 高
考虑 数据类型
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    double pi = 3.14159, v;
    int h, r, l, cnt;
    cin >> h >> r;
    
    v = pi * r * r * h;
    cnt = 20000 / v;

    if (cnt * v < 20000)
    {
        cnt++;
    }

    cout << cnt;

    return 0;
}
```

{% endhideToggle %}



## 7017：三角形面积（海伦公式）

{% hideToggle 提示%}

```cpp
sqrt()根号   头文件cmath
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>

using namespace std;

int main()
{
    double a, b, c, p, s;
    cin >> a >> b >> c;
    
    p = (a + b + c) / 2;
    s = sqrt(p * (p - a) * (p - b) * (p - c));

    cout << fixed << setprecision(3) << s;

    return 0;
}
```

{% endhideToggle %}



