---
title: 第一课 认识C++01
tags: 题解
categories:
  - 题解
  - magooj
  - 一阶 语法基础
password: 123456
abstract: 已被加密，请输入密码查看
message: 需要密码查看
wrong_pass_message: 密码错误， 重试
abbrlink: 41573
date: 2026-01-01 21:32:20
---

# 第一阶段 第一课 认识C++01

第一课 基础输入输出

仅作参考不多解释



## 6001：A+B Problem

{% hideToggle 提示%}
~~~cpp
基础输入输出
按照格式书写
简单输出可以不使用变量存储
如果后续需要多次使用再使用变量
如： 
int c = a + b;
cout << c;
~~~
{% endhideToggle %}

{% hideToggle 题解%}

~~~cpp
#include <iostream>

using namespace std;

int main()
{
    int a, b;
    cin >> a >> b;
    
    cout << a + b;
    
    return 0;
}
~~~

{% endhideToggle %}



## 6002：电影票

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



## 6003：字符三角形

{% hideToggle 提示%}

~~~cpp
字符类型输入输出
行换默认用 '\n'，除非你明确知道需要立即刷新缓冲区才用 endl。
记住：endl 不仅仅是换行，它是一个 换行 + 刷新 操作，这个“刷新”代价高昂，不要滥用
~~~

{% endhideToggle %}

{% hideToggle 题解%}

~~~cpp
#include <iostream>

using namespace std;

int main() 
{
    char c;
    cin >> c;

    cout << ' ' << ' ' << c << '\n';
    cout << ' ' << c << c << c << '\n';
    cout << c << c << c << c << c << '\n';

    return 0;
}
~~~

{% endhideToggle %}



## 6004：Hello,World!

{% hideToggle 提示%}

```cpp
基础输出
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main()
{
    cout << "Hello,World!" << '\n';

    return 0;
}
```

{% endhideToggle %}

{% hideToggle 题解二%}

```cpp
#include <cstdio>

using namespace std;

int main()
{
    printf("Hello,World!\n");

    return 0;
}
```

{% endhideToggle %}



## 6005：输出第二个整数

{% hideToggle 提示%}

```cpp
基础输入输出
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

    cout << b << '\n';

    return 0;
}
```

{% endhideToggle %}



## 6006：字符菱形

{% hideToggle 提示%}

```cpp
字符类型输入输出
```

{% endhideToggle %}

{% hideToggle 题解%}

```cpp
#include <iostream>

using namespace std;

int main() 
{
    char c;
    cin >> c;

    cout << ' ' << ' ' << c << '\n';
    cout << ' ' << c << c << c << '\n';
    cout << c << c << c << c << c << '\n';
    cout << ' ' << ' ' << c << '\n';
    cout << ' ' << c << c << c << '\n';

    return 0;
}
```

{% endhideToggle %}
