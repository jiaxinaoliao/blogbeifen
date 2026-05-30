---
title: C++指针详解
tags: OI相关
categories: C++ OI
abbrlink: 19359
date: 2026-05-10 20:23:52
---

# C++指针详解

## 第一部分：指针基础概念

### 1.1 什么是指针？

指针是一个变量，其值是另一个变量的内存地址。

```cpp
#include <iostream>
using namespace std;

int main() {
    int var = 10;      // 普通整型变量
    int *ptr = &var;   // 指针变量，存储var的地址
    
    cout << "变量var的值: " << var << endl;
    cout << "变量var的地址: " << &var << endl;
    cout << "指针ptr的值: " << ptr << endl;
    cout << "通过ptr访问的值: " << *ptr << endl;
    
    return 0;
}
```

### 1.2 指针的声明和初始化

```cpp
#include <iostream>
using namespace std;

int main() {
    // 指针声明语法
    int *intPtr;        // 指向整型的指针
    double *doublePtr;  // 指向双精度浮点型的指针
    char *charPtr;      // 指向字符的指针

    // 初始化指针
    int num = 5;
    int *ptr = &num;    // 正确：指向num的地址
    
    cout << "num的值: " << num << endl;
    cout << "ptr指向的值: " << *ptr << endl;
    
    return 0;
}
```

### 1.3 空指针和野指针

```cpp
#include <iostream>
using namespace std;

int main() {
    // 空指针 - 安全的做法
    int *ptr1 = nullptr;  // C++11推荐
    int *ptr2 = NULL;     // 传统C风格
    int *ptr3 = 0;        // 直接赋0
    
    cout << "空指针示例:" << endl;
    cout << "ptr1: " << ptr1 << endl;
    cout << "ptr2: " << ptr2 << endl;
    cout << "ptr3: " << ptr3 << endl;
    
    // 避免野指针：总是初始化指针
    int value = 100;
    int *safePtr = &value;
    cout << "安全指针指向的值: " << *safePtr << endl;
    
    return 0;
}
```

## 第二部分：指针运算和数组

### 2.1 指针算术运算

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *ptr = arr;  // 指向数组第一个元素
    
    cout << "初始指针位置: " << *ptr << endl;
    
    ptr++;  // 移动到下一个整数位置
    cout << "ptr++后: " << *ptr << endl;
    
    ptr += 2;  // 向后移动两个整数位置
    cout << "ptr+=2后: " << *ptr << endl;
    
    ptr--;  // 向前移动一个位置
    cout << "ptr--后: " << *ptr << endl;
    
    // 指针相减得到元素个数
    int *ptr2 = &arr[4];
    cout << "数组元素个数: " << (ptr2 - ptr) << endl;
    
    return 0;
}
```

### 2.2 指针与数组的关系

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    
    // 数组名本身是指向第一个元素的指针
    cout << "numbers: " << numbers << endl;
    cout << "&numbers[0]: " << &numbers[0] << endl;
    
    // 三种访问数组元素的方式
    cout << "\n访问数组元素:" << endl;
    for(int i = 0; i < 5; i++) {
        cout << "numbers[" << i << "] = " 
             << numbers[i]        // 数组下标法
             << " = " << *(numbers + i)  // 指针算术法
             << " = " << *(i + numbers) << endl; // 同样有效
    }
    
    return 0;
}
```

### 2.3 指针数组 vs 数组指针

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 1, b = 2, c = 3;
    
    // 指针数组 - 数组元素是指针
    int *ptrArray[3] = {&a, &b, &c};
    
    // 数组指针 - 指向整个数组的指针
    int arr[3] = {10, 20, 30};
    int (*arrayPtr)[3] = &arr;
    
    cout << "指针数组示例:" << endl;
    for(int i = 0; i < 3; i++) {
        cout << *ptrArray[i] << " ";
    }
    
    cout << "\n\n数组指针示例:" << endl;
    for(int i = 0; i < 3; i++) {
        cout << (*arrayPtr)[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 第三部分：动态内存管理

### 3.1 new和delete操作符

```cpp
#include <iostream>
using namespace std;

int main() {
    // 动态分配单个变量
    int *singlePtr = new int;
    *singlePtr = 42;
    cout << "动态整数: " << *singlePtr << endl;
    delete singlePtr;  // 释放内存
    singlePtr = nullptr;  // 避免悬空指针
    
    // 动态分配数组
    int size = 5;
    int *arrayPtr = new int[size];
    
    // 初始化数组
    for(int i = 0; i < size; i++) {
        arrayPtr[i] = i * 10;
    }
    
    // 使用数组
    cout << "\n动态数组内容:" << endl;
    for(int i = 0; i < size; i++) {
        cout << arrayPtr[i] << " ";
    }
    cout << endl;
    
    // 释放数组内存
    delete[] arrayPtr;
    arrayPtr = nullptr;
    
    return 0;
}
```

### 3.2 内存泄漏和预防

```cpp
#include <iostream>
using namespace std;

class MemoryManager {
private:
    int *data;
    int size;
    
public:
    // 构造函数
    MemoryManager(int s) : size(s) {
        data = new int[size];
        cout << "分配了 " << size << " 个整数的内存" << endl;
    }
    
    // 析构函数 - 防止内存泄漏
    ~MemoryManager() {
        delete[] data;
        cout << "释放了内存" << endl;
    }
    
    // 拷贝构造函数 - 防止浅拷贝问题
    MemoryManager(const MemoryManager& other) : size(other.size) {
        data = new int[size];
        for(int i = 0; i < size; i++) {
            data[i] = other.data[i];
        }
        cout << "深拷贝完成" << endl;
    }
    
    void display() const {
        for(int i = 0; i < size; i++) {
            cout << data[i] << " ";
        }
        cout << endl;
    }
};

int main() {
    MemoryManager manager1(5);
    
    // 使用拷贝构造函数避免内存问题
    MemoryManager manager2 = manager1;
    
    cout << "manager1: ";
    manager1.display();
    
    cout << "manager2: ";
    manager2.display();
    
    return 0;
}
```

## 第四部分：指针与函数

### 4.1 指针作为函数参数

```cpp
#include <iostream>
using namespace std;

// 值传递 - 不影响原始变量
void swapByValue(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}

// 指针传递 - 可以修改原始变量
void swapByPointer(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// 引用传递 - C++推荐方式
void swapByReference(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 10, y = 20;
    
    cout << "原始值: x=" << x << ", y=" << y << endl;
    
    swapByValue(x, y);
    cout << "值传递后: x=" << x << ", y=" << y << endl;
    
    swapByPointer(&x, &y);
    cout << "指针传递后: x=" << x << ", y=" << y << endl;
    
    swapByReference(x, y);
    cout << "引用传递后: x=" << x << ", y=" << y << endl;
    
    return 0;
}
```

### 4.2 返回指针的函数

```cpp
#include <iostream>
using namespace std;

// 安全：返回动态分配的内存
int* createArray(int size) {
    int *arr = new int[size];
    for(int i = 0; i < size; i++) {
        arr[i] = i * i;
    }
    return arr;
}

// 安全：返回静态变量的指针
int* getStaticValue() {
    static int staticVar = 999;
    return &staticVar;  // 安全：静态变量持续存在
}

int main() {
    int *dynamicArray = createArray(5);
    
    cout << "动态数组: ";
    for(int i = 0; i < 5; i++) {
        cout << dynamicArray[i] << " ";
    }
    cout << endl;
    
    delete[] dynamicArray;
    
    int *staticPtr = getStaticValue();
    cout << "静态变量: " << *staticPtr << endl;
    
    return 0;
}
```

### 4.3 函数指针

```cpp
#include <iostream>
using namespace std;

// 简单的数学函数
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

int multiply(int a, int b) {
    return a * b;
}

// 使用函数指针作为参数的函数
void calculate(int a, int b, int (*operation)(int, int)) {
    int result = operation(a, b);
    cout << "计算结果: " << result << endl;
}

int main() {
    // 声明函数指针
    int (*funcPtr)(int, int);
    
    // 指向不同的函数
    funcPtr = add;
    cout << "加法: " << funcPtr(10, 5) << endl;
    
    funcPtr = subtract;
    cout << "减法: " << funcPtr(10, 5) << endl;
    
    funcPtr = multiply;
    cout << "乘法: " << funcPtr(10, 5) << endl;
    
    // 使用回调函数
    cout << "\n使用回调函数:" << endl;
    calculate(20, 4, add);
    calculate(20, 4, subtract);
    calculate(20, 4, multiply);
    
    return 0;
}
```

## 第五部分：高级指针概念

### 5.1 指向指针的指针

```cpp
#include <iostream>
using namespace std;

int main() {
    int value = 100;
    int *ptr = &value;
    int **pptr = &ptr;  // 指向指针的指针
    
    cout << "变量value的值: " << value << endl;
    cout << "指针ptr指向的值: " << *ptr << endl;
    cout << "指针的指针pptr指向的值: " << **pptr << endl;
    
    cout << "\n地址信息:" << endl;
    cout << "value的地址: " << &value << endl;
    cout << "ptr的值(存储的地址): " << ptr << endl;
    cout << "ptr的地址: " << &ptr << endl;
    cout << "pptr的值(存储的地址): " << pptr << endl;
    
    // 修改多层指针的值
    **pptr = 200;
    cout << "\n修改后value的值: " << value << endl;
    
    return 0;
}
```

### 5.2 const与指针

```cpp
#include <iostream>
using namespace std;

int main() {
    int normalVar = 10;
    const int constVar = 20;
    
    // 指向常量的指针 - 不能通过指针修改值
    const int *ptrToConst = &normalVar;
    // *ptrToConst = 30;  // 错误！不能修改
    ptrToConst = &constVar;  // 正确：可以改变指针指向
    
    // 常量指针 - 指针本身不能指向其他地址
    int *const constPtr = &normalVar;
    *constPtr = 30;  // 正确：可以修改指向的值
    // constPtr = &constVar;  // 错误！不能改变指针指向
    
    // 指向常量的常量指针 - 都不能修改
    const int *const constPtrToConst = &constVar;
    // *constPtrToConst = 40;  // 错误！
    // constPtrToConst = &normalVar;  // 错误！
    
    cout << "指向常量的指针: " << *ptrToConst << endl;
    cout << "常量指针: " << *constPtr << endl;
    cout << "指向常量的常量指针: " << *constPtrToConst << endl;
    
    return 0;
}
```

### 5.3 void指针和类型转换

```cpp
#include <iostream>
using namespace std;

int main() {
    int intValue = 65;
    double doubleValue = 3.14;
    char charValue = 'A';
    
    // void指针可以指向任何类型
    void *voidPtr;
    
    voidPtr = &intValue;
    cout << "整数值: " << *(static_cast<int*>(voidPtr)) << endl;
    
    voidPtr = &doubleValue;
    cout << "双精度值: " << *(static_cast<double*>(voidPtr)) << endl;
    
    voidPtr = &charValue;
    cout << "字符值: " << *(static_cast<char*>(voidPtr)) << endl;
    
    // 指针类型转换示例
    int number = 0x41424344;  // ASCII码: A B C D
    char *charPtr = reinterpret_cast<char*>(&number);
    
    cout << "\n整数: " << number << endl;
    cout << "作为字符: ";
    for(int i = 0; i < 4; i++) {
        cout << charPtr[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 第六部分：综合练习和实际应用

### 6.1 字符串操作练习

```cpp
#include <iostream>
#include <cstring>
using namespace std;

// 自定义字符串长度函数
int myStrlen(const char *str) {
    int length = 0;
    while(*str != '\0') {
        length++;
        str++;
    }
    return length;
}

// 自定义字符串拷贝函数
void myStrcpy(char *dest, const char *src) {
    while(*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';
}

// 自定义字符串连接函数
// 注意：连续多次调用时，每次都会遍历到 dest 末尾，效率 O(n²)
// 实际使用时可记录当前末尾位置进行优化
void myStrcat(char *dest, const char *src) {
    // 移动到dest的末尾
    while(*dest != '\0') {
        dest++;
    }
    
    // 拷贝src到dest末尾
    while(*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';
}

int main() {
    char str1[100] = "Hello";
    char str2[] = "World";
    char str3[100];
    
    cout << "字符串操作演示:" << endl;
    cout << "str1: " << str1 << " (长度: " << myStrlen(str1) << ")" << endl;
    cout << "str2: " << str2 << " (长度: " << myStrlen(str2) << ")" << endl;
    
    myStrcpy(str3, str1);
    cout << "拷贝后str3: " << str3 << endl;
    
    myStrcat(str1, " ");
    myStrcat(str1, str2);
    cout << "连接后str1: " << str1 << endl;
    
    return 0;
}
```

### 6.2 动态二维数组

```cpp
#include <iostream>
using namespace std;

// 创建动态二维数组
int** create2DArray(int rows, int cols) {
    int **array = new int*[rows];  // 创建行指针数组
    for(int i = 0; i < rows; i++) {
        array[i] = new int[cols];  // 为每一行分配内存
    }
    return array;
}

// 初始化二维数组
void initialize2DArray(int **array, int rows, int cols) {
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            array[i][j] = i * cols + j + 1;
        }
    }
}

// 打印二维数组
void print2DArray(int **array, int rows, int cols) {
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            cout << array[i][j] << "\t";
        }
        cout << endl;
    }
}

// 释放二维数组内存
void delete2DArray(int **array, int rows) {
    for(int i = 0; i < rows; i++) {
        delete[] array[i];  // 释放每一行
    }
    delete[] array;  // 释放行指针数组
}

int main() {
    int rows = 3, cols = 4;
    
    // 创建动态二维数组
    int **matrix = create2DArray(rows, cols);
    
    // 初始化和打印
    initialize2DArray(matrix, rows, cols);
    cout << "动态二维数组:" << endl;
    print2DArray(matrix, rows, cols);
    
    // 释放内存
    delete2DArray(matrix, rows);
    
    return 0;
}
```

## 学习总结

### 重点回顾：

1. **指针基础**：理解指针是存储地址的变量
2. **指针运算**：掌握指针算术和数组关系
3. **动态内存**：熟练使用new/delete，避免内存泄漏
4. **函数指针**：理解回调函数机制
5. **高级概念**：掌握多级指针和const修饰符

### 最佳实践：

- 总是初始化指针
- 使用nullptr而不是NULL
- 及时释放动态分配的内存
- 避免返回局部变量的指针
- 使用智能指针（C++11+）自动管理内存



