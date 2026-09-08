---
title: 第三课 算术运算符01
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 15974
date: 2026-01-01 19:57:51
---

# 第一阶段 第三课 算术运算符01

## 7001：整数的和

{% hideToggle 提示%}

```cpp
 基础运算以及输入输出
 + - * / %
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
    
    cout << a + b + c;
    
    return 0;
}
```

{% endhideToggle %}



## 7002：整型与布尔型的转换

{% hideToggle 提示%}

```cpp
数据类型转换
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>
 
using namespace std;
 
int main()
{
    int a;
    bool b;
    cin >> a;
    
    b = a;
    a = b;
    
    cout << a;
    
    return 0;
}
```

{% endhideToggle %}



## 7003：整型数据类型存储空间大小

{% hideToggle 提示%}

```cpp
sizeof()的使用
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>
 
using namespace std;
 
int main()
{
    int a;
    short int b;
    cin >> a >> b;
    
    cout << sizeof(a) << " " << sizeof(b);
    
    return 0;
}
```

{% endhideToggle %}



## 7004：AxB问题

{% hideToggle 提示%}

```cpp
这个问题中，给出了两个数的取值范围 1 <= A, B <= 50000
int的取值范围 [-2147483648, 2147483647]
两个数都可能取到 50000， 两个50000相乘，就超过int的上限了
就要换成long long，但不是都用long long，该用的时候用
```

{% endhideToggle %}

{% hideToggle 题解一%}

```cpp
#include <iostream>
 
using namespace std;
 
int main()
{
    long long a, b;
    cin >> a >> b;
    
    cout << a * b;
    
    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
// 由于 a , b 没有超出 int 的上限
// 所以定义的时候可以使用 int  计算的时候使用 long long
#include <iostream>
 
using namespace std;
 
int main()
{
    int a, b;
    cin >> a >> b;
    
    cout << (long long)a * b;
    
    return 0;
}
```

{% endhideToggle %}



## 7005： 计算(a+b)×c的值

{% hideToggle 提示%}

```cpp
基础算术运算
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int a, b, c;
	cin >> a >> b >> c;
	
	cout << (a + b) * c; 

	return 0;
}
```

{% endhideToggle %}



## 7006:  计算(a+b)/c的值

{% hideToggle 提示%}

```cpp
基础算术运算
在计算机当中除法 / 是要特殊注意的
如果两边都是整数，得到一个向下取整的整数结果
如果两遍有一个是小数，就会得到一个小数的结果 隐式数据类型转换
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int a, b, c;
	cin >> a >> b >> c;
	
	cout << (a + b) / c; 

	return 0;
}
```

{% endhideToggle %}



## 7008: 反向输出一个三位数

{% hideToggle 提示%}

```cpp
数字翻转，是一个经典问题
有的题目要求输出前导零，有的题目不要输出前导零
这道题需要前导零
```

{% endhideToggle %}

{% hideToggle 题解一%}

```cpp
#include <iostream>

using namespace std;

int main() 
{
    int x;
    cin >> x;

    cout << x % 10 << x / 10 % 10 << x / 100 << '\n';

    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
#include <algorithm>
#include <iostream>
#include <string>

using namespace std;

int main()
{
    string s;
    cin >> s;

    reverse(s.begin(), s.end());

    cout << s << endl;

    return 0;
}
```

{% endhideToggle %}



## 7009：尼克与强盗

{% hideToggle 提示%}

```cpp
不需要前导零
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include<iostream>

using namespace std;

int main()
{
	int n, gw, sw, ans;
	
	cin >> n;
	
	gw = n % 10;
	sw = n / 10;
	
	ans = gw * 10 + sw;
	cout << ans;

	return 0;
}

```

{% endhideToggle %}

