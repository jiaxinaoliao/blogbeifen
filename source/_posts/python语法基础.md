---
title: python语法基础
tags: python
categories: python 教程
abbrlink: 28571
date: 2026-05-15 06:15:49
---

# python语法基础

## 第1章 Python简介

Python是一种解释型、面向对象、动态数据类型的高级程序设计语言。由Guido van Rossum于1989年底发明，第一个公开发行版发行于1991年。



### Python的特点

- **简单易学**：语法清晰，接近自然语言，适合初学者。
- **免费开源**：可以自由使用、修改和分发。
- **可移植性**：几乎可以在所有操作系统上运行。
- **丰富的库**：拥有大量标准库和第三方库（如turtle、random、numpy等）。
- **解释型语言**：代码逐行执行，便于调试。



### 应用领域

- Web开发（Django、Flask）
- 数据分析与人工智能（Pandas、TensorFlow）
- 自动化脚本
- 科学计算
- 游戏开发
- 网络爬虫



### 安装与环境

建议安装Python 3.6以上版本，并配置好环境变量。可以使用IDLE、PyCharm、VS Code等编辑器编写代码。

------



## 第2章 输出语句

`print()`函数用于在屏幕上输出信息。



### 基本用法

```python
print("hello")
```

- `print` 是函数名（Python 3中必须加括号）。
- 双引号中的内容称为**字符串**，会原封不动输出。
- 也可以用单引号：`print('hello')`



### 输出多个内容

```python
print("你好", "世界", 2024)
# 输出：你好 世界 2024
```

默认用空格分隔，可通过`sep`参数修改分隔符：

```python
print("a", "b", "c", sep="-")  # 输出 a-b-c
```



### 输出后不换行

```python
print("hello", end="")
print("world")  # 输出 helloworld
```

`end`参数指定结尾字符，默认为换行符`\n`。



### 练习

1. 输出你的姓名和年龄。
2. 输出一首诗，每句占一行。

------



## 第3章 变量



### 3.1 定义

变量是存储数据的一个“盒子”，有一个名字，可以存放不同类型的数据（数字、字符串、布尔值等）。Python中变量不需要提前声明类型，直接赋值即可。



### 3.2 赋值

使用等号`=`给变量赋值。

```python
a = 0       # 整数
b = 3.14    # 浮点数
c = "你好"  # 字符串
d = True    # 布尔值
```

变量可以重新赋值，改变其值：

```python
a = 2
print(a)    # 输出 2
```

**命名规则**：

- 只能包含字母、数字、下划线，且不能以数字开头。
- 区分大小写（`age`和`Age`不同）。
- 不要使用Python关键字（如`print`、`if`、`while`等）。
- 建议使用有意义的名称，例如`student_name`而非`sn`。



### 多个变量同时赋值

```python
x, y, z = 1, 2, 3
a = b = c = 10
```



### 练习

1. 定义变量`price = 25.5`，`quantity = 3`，计算总价并输出。
2. 交换两个变量`x = 5`，`y = 10`的值。

------



## 第4章 输入

`input()`函数用于从键盘获取用户输入。输入的内容默认以**字符串**类型返回。



### 基本用法

```python
name = input("请输入您的名字：")
print("你好，" + name)
```

括号中的字符串是提示语，会在等待输入前显示。



### 类型转换

因为`input()`返回字符串，如果需要数字，必须用`int()`或`float()`转换。

```python
age = int(input("请输入年龄："))   # 将字符串转为整数
height = float(input("请输入身高（米）："))
```

如果不转换直接做数学运算会出错（字符串不能和数字相加）。



### 完整示例

```python
a = int(input("请输入第一个整数："))
b = int(input("请输入第二个整数："))
print("两数之和为：", a + b)
```



### 注意

- 如果用户输入的内容无法转换成数字（如输入字母），程序会报错（ValueError）。
- 后续学习异常处理可以解决这个问题。



### 练习

1. 输入半径，计算圆的面积（π取3.14）。
2. 输入两个数字，输出它们的乘积。

------



## 第5章 分支语句

分支语句根据条件是否成立，决定执行哪一段代码。



### 5.1 单分支语句（if）

格式：

```python
if 条件:
    语句块
```

当条件为`True`时执行语句块，否则跳过。

```python
score = 85
if score >= 60:
    print("及格了！")
```

注意：冒号和缩进（通常4个空格）是Python语法的重要组成部分。



### 5.2 双分支语句（if-else）

```python
if 条件:
    语句块1
else:
    语句块2
```



```python
num = int(input("输入一个整数："))
if num % 2 == 0:
    print("偶数")
else:
    print("奇数")
```



### 5.3 多分支语句（if-elif-else）

用于多个互斥的条件判断。

```python
if 条件1:
    语句块1
elif 条件2:
    语句块2
elif 条件3:
    语句块3
else:
    语句块4
```

示例：成绩等级评定

```python
score = int(input("请输入成绩（0-100）："))
if score >= 90:
    print("优秀")
elif score >= 75:
    print("良好")
elif score >= 60:
    print("及格")
else:
    print("不及格")
```

- `elif`可以有多个。
- `else`可选。
- 条件依次判断，一旦满足就退出整个结构。



### 练习

1. 输入一个年份，判断是否为闰年（能被4整除但不能被100整除，或者能被400整除）。
2. 输入三个数，输出其中的最大值。

------



## 第6章 循环结构一 - while

`while`循环在条件为真时重复执行一段代码。

### 基本格式

```python
while 条件:
    循环体
```

示例：输出1到5

```python
i = 1
while i <= 5:
    print(i)
    i = i + 1   # 或 i += 1
```



### 无限循环

若条件永远为真，循环会一直执行，通常配合`break`退出。

```python
while True:
    s = input("输入exit退出：")
    if s == "exit":
        break
    print("你输入了：", s)
```



### 循环控制语句

- `break`：立即终止整个循环。
- `continue`：跳过本次循环剩余代码，进入下一次迭代。

```python
i = 0
while i < 10:
    i += 1
    if i % 2 == 0:
        continue
    print(i)   # 输出奇数 1,3,5,7,9
```



### 应用：累加求和

```python
n = int(input("请输入n："))
total = 0
i = 1
while i <= n:
    total += i
    i += 1
print("1到%d的和为：%d" % (n, total))
```



### 练习

1. 使用while循环输出10到1的倒序数字。
2. 计算1×2×3×...×n（阶乘）。

------



## 第7章 循环结构二 - for

`for`循环通常用于遍历序列（字符串、列表、范围等）。

### 基本格式

```python
for 变量 in 可迭代对象:
    循环体
```



### 使用`range()`函数

`range(start, stop, step)`生成整数序列。

- `range(5)` → 0,1,2,3,4
- `range(2, 8)` → 2,3,4,5,6,7
- `range(1, 10, 2)` → 1,3,5,7,9

示例：输出1到5

```python
for i in range(1, 6):
    print(i)
```



### 遍历字符串

```python
for ch in "Python":
    print(ch)
```



### 与else配合

`for`循环正常执行完（没有被break终止）后会执行`else`块。

```python
for i in range(3):
    print(i)
else:
    print("循环结束")
```

输出：
0
1
2
循环结束



### 应用：计算总和

```python
n = int(input("请输入n："))
total = 0
for i in range(1, n+1):
    total += i
print(total)
```



### 练习

1. 用for循环输出1到100之间所有3的倍数。
2. 输入一个字符串，统计其中字母`'a'`出现的次数。

------



## 第8章 循环嵌套

一个循环内部包含另一个循环，称为循环嵌套。常用在打印图形、处理二维数据等场景。

### 示例1：打印矩形

```python
rows = 5
cols = 10
for i in range(rows):
    for j in range(cols):
        print("*", end="")
    print()   # 换行
```



### 示例2：打印直角三角形

```python
n = 5
for i in range(1, n+1):
    for j in range(i):
        print("*", end="")
    print(
```

输出：
*
**

------

------

------



### 示例3：乘法口诀表

```python
for i in range(1, 10):
    for j in range(1, i+1):
        print(f"{j}×{i}={i*j}", end="\t")
    print()
```



### 循环嵌套中的break和continue

- `break`只跳出**最内层**的循环。
- 如果需要跳出外层循环，可以使用标志变量或者将循环封装成函数。



### 练习

1. 使用循环嵌套输出一个数字金字塔（自己设计样式）。
2. 找出100以内的所有质数（提示：需要两层循环判断因数）。

------



## 第9章 random

`random`模块用于生成随机数或进行随机操作。使用时需要先导入：`import random`

### 常用函数

| 函数                   | 说明                             | 示例                                             |
| :--------------------- | :------------------------------- | :----------------------------------------------- |
| `random.random()`      | 返回[0.0, 1.0)之间的随机浮点数   | `r = random.random()`                            |
| `random.randint(a, b)` | 返回[a, b]之间的整数（包含两端） | `num = random.randint(1, 100)`                   |
| `random.choice(seq)`   | 从序列中随机选择一个元素         | `lst = ["红","黄","蓝"]; c = random.choice(lst)` |
| `random.shuffle(lst)`  | 随机打乱列表的顺序（就地修改）   | `random.shuffle(cards)`                          |
| `random.uniform(a, b)` | 返回[a, b]之间的随机浮点数       | `temp = random.uniform(36.0, 37.5)`              |



### 示例：猜数字游戏

```python
import random

target = random.randint(1, 100)
guess = -1
times = 0

while guess != target:
    guess = int(input("猜一个1~100之间的数字："))
    times += 1
    if guess < target:
        print("太小了")
    elif guess > target:
        print("太大了")
print(f"恭喜！你用了{times}次猜中了数字{target}")
```



### 示例：随机抽奖

```python
names = ["张三", "李四", "王五", "赵六"]
winner = random.choice(names)
print("中奖者：", winner)
```



### 练习

1. 生成一个长度为10的随机整数列表（范围1~50），然后找出其中的最大值。
2. 模拟掷骰子（1~6），掷100次，统计每个点数出现的次数。

------



## 第10章 turtle

`turtle`是Python内置的一个绘图模块，模拟一只小海龟在屏幕上爬行，留下轨迹。非常适合初学者学习编程和几何绘图。

### 基本操作

| 命令                   | 说明                   |
| :--------------------- | :--------------------- |
| `import turtle`        | 导入turtle模块         |
| `t = turtle.Turtle()`  | 创建一只海龟对象       |
| `t.forward(distance)`  | 向前移动distance像素   |
| `t.backward(distance)` | 向后移动               |
| `t.right(angle)`       | 右转angle度            |
| `t.left(angle)`        | 左转angle度            |
| `t.penup()`            | 抬起笔，移动不画线     |
| `t.pendown()`          | 落下笔，开始画线       |
| `t.color("red")`       | 设置画笔颜色           |
| `t.pensize(width)`     | 设置笔的粗细           |
| `t.circle(radius)`     | 画半径为radius的圆     |
| `t.goto(x, y)`         | 移动到坐标(x, y)       |
| `t.clear()`            | 清空绘图但海龟位置不变 |
| `t.reset()`            | 清空并让海龟回到原点   |
| `turtle.done()`        | 保持窗口不关闭         |

### 示例1：画一个正方形

```python
import turtle
t = turtle.Turtle()
for _ in range(4):
    t.forward(100)
    t.right(90)
turtle.done()
```



### 示例2：画一个彩色五角星

```python
import turtle
t = turtle.Turtle()
t.pensize(2)
t.color("red")
for _ in range(5):
    t.forward(150)
    t.right(144)   # 五角星每个外角144度
turtle.done()
```



### 示例3：画一个螺旋线

```python
import turtle
t = turtle.Turtle()
t.speed(0)          # 最快速度
for i in range(100):
    t.forward(i * 2)
    t.right(90)
turtle.done()
```



### 常用设置

- `t.speed(0)` 最快，`1`最慢，`6`正常。
- `turtle.bgcolor("black")` 设置背景颜色。
- `t.fillcolor("yellow")` 设置填充颜色，配合`t.begin_fill()`和`t.end_fill()`使用。



### 练习

1. 画一个等边三角形（内角60度，注意转向）。
2. 画一个笑脸（两个眼睛用圆圈，嘴巴用弧线或半圆）。
3. 画一朵由多个花瓣组成的花（例如使用循环和画圆）。

