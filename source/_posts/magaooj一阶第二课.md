---
title: 第二课 认识C++02
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 12831
date: 2026-01-01 20:57:46
---



# 第一阶段 第二课 认识C++02

## 8001：交换值

{% hideToggle 提示%}

```cpp
多种方式交换
取巧的办法
第一种 引入新变量
第二种 使用数学方式
第三种 使用swap函数
```

{% endhideToggle %}

{% hideToggle 题解取巧%}

```cpp
#include <iostream>

using namespace std;
 
int main()
{
    int a, b;
    cin >> a >> b;
     
    cout << b << " " << a << '\n';
     
    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解一 常用%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    int a, b;
    cin >> a >> b;

    int t = a;
    a = b;
    b = t;

    cout << a << ' ' << b << '\n';

    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
#include <iostream>

using namespace std;
 
int main()
{
    int a, b;
    cin >> a >> b;
     
    a += b;
    b = a - b;
    a -= b;
     
    cout << a << " " << b << '\n';
     
    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解三%}

```cpp
#include <iostream>
#include <utility> // swap的头文件 c++11 之前在 algorithm 里面

using namespace std;

int main()
{
    int a, b;
    cin >> a >> b;

    swap(a, b);

    cout << a << " " << b << '\n';

    return 0;
}
```

{% endhideToggle %}



## 8002：买图书

{% hideToggle 提示%}

```cpp
保留小数
法一
    1、double 类型
    2、添加 iomanip 头文件
    3、cout << fixed << setprecision(n)
    
法二：
    cstdio
    printf()
```

{% endhideToggle %}

{% hideToggle 题解一%}

```cpp
#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
    double n, m;
    cin >> n >> m;

    cout << fixed << setprecision(2) << n - (m * 0.8);

    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() 
{
    int n, m;
    cin >> n >> m;

    printf("%.2lf", n - m * 0.8);

    return 0;
}
```

{% endhideToggle %}



## 8003：对齐输出

{% hideToggle 提示%}

```cpp
输出格式控制
```

{% endhideToggle %}

{% hideToggle 题解一%}

```cpp
#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
	int a, b, c;
	cin >> a >> b >> c;

	cout << setw(8) << a << " ";
	cout << setw(8) << b << " ";
	cout << setw(8) << c;
	
	return 0;
} 

//	set -- 设置
//	width -- 宽度
//	
//	头文件 iomanip 输入输出操作 
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
#include <cstdio>

using namespace std;

int main()
{
	int a, b, c;
	scanf("%d %d %d", &a, &b, &c);
	
    printf("%8d %8d %8d", a, b, c)
	
	return 0;
} 
```

{% endhideToggle %}



## 8004：梯形面积

{% hideToggle 提示%}

```cpp
数学面积计算 三角形 与 梯形
```

{% endhideToggle %}

{% hideToggle 题解取巧%}

```cpp
#include <cstdio>

using namespace std;

int main() {
    int h = 150 * 2 / 15;
    double s = 1.0 * (15 + 25) * h / 2;

    printf("%.2lf\n", s);

    return 0;
}
```

{% endhideToggle %}



## 8005：电影票

{% hideToggle 提示%}

~~~cpp
基础运算以及输入输出
+ - * / %
~~~

{% endhideToggle %}

{% hideToggle 题解%}

~~~cpp
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;
    
    cout << n << " " << n * 10;
    
    return 0;
}
~~~

{% endhideToggle %}
