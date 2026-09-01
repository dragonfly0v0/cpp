# 《C++ Primer》学习路线图（C 程序员特供版）

### 第一部分：C++ 基础（第1-4章）

> **目标**：适应 C++ 语法糖，建立“类型安全”和“引用”思维。

| **章节**   | **内容**      | **你的学习策略**                                                                           |
| -------- | ----------- | ------------------------------------------------------------------------------------ |
| **第1章**​ | 开始：IO、注释、循环 | **快速浏览**。只看 `iostream` 的用法（`cin`, `cout`），其他和 C 一样。                                  |
| **第2章**​ | 变量和基本类型     | **重点看**：引用（`&`）、`const`、类型别名、`auto`、`decltype`。指针部分你已经懂了，但注意 C++ 的 `const` 比 C 严格。   |
| **第3章**​ | 字符串、向量和数组   | **核心章节**。`std::string` 替代 `char*`，`std::vector` 替代动态数组，**迭代器**初探。C 风格的数组和指针可以跳过或快速过。 |
| **第4章**​ | 表达式         | **快速浏览**。大部分和 C 一样，注意 `sizeof` 对 class 的行为差异，以及列表初始化。                                |

### 第二部分：C++ 标准库（第5-7章）

**目标**：掌握函数新特性和面向对象入门。

| **章节**   | **内容** | **你的学习策略**                                               |
| -------- | ------ | -------------------------------------------------------- |
| **第5章**​ | 语句     | **跳过或极速过**。和 C 几乎一样。                                     |
| **第6章**​ | 函数     | **重点看**：引用传参（避免拷贝）、函数重载、默认参数、`constexpr` 函数、内联。          |
| **第7章**​ | 类      | **分水岭章节**。认真学！定义类、`this` 指针、访问控制、构造函数（尤其是初始化列表）、友元、静态成员。 |

### 第三部分：类设计者的工具（第8-12章）

**目标**：掌握 STL 和现代 C++ 内存管理。

| **章节**    | **内容** | **你的学习策略**                                                               |
| --------- | ------ | ------------------------------------------------------------------------ |
| **第8章**​  | IO 库   | 细看。文件流 `ifstream`/`ofstream`，字符串流，格式化输入输出。                               |
| **第9章**​  | 顺序容器   | **核心**。重点掌握 `vector`、`string`、`deque`、`list` 的底层差异和使用场景。                 |
| **第10章**​ | 泛型算法   | **核心**。`<algorithm>` 头文件，`find`, `sort`, `lambda` 表达式初探。                 |
| **第11章**​ | 关联容器   | **核心**。`map`, `set`, `unordered_map`。理解红黑树 vs 哈希表。                       |
| **第12章**​ | 动态内存   | **核心中的核心**。`shared_ptr`, `unique_ptr`, `weak_ptr`。**彻底告别 `new/delete`**。 |

### 第四部分：高级主题（第13-16章）

> **目标**：深入理解拷贝控制、多态和模板。

| **章节**    | **内容**  | **你的学习策略**                     |
| --------- | ------- | ------------------------------ |
| **第13章**​ | 拷贝控制    | 深拷贝 vs 浅拷贝，移动语义（C++11 重点）。     |
| **第14章**​ | 重载与类型转换 | 运算符重载（如 `string` 的 `+` 是怎么来的）。 |
| **第15章**​ | 面向对象编程  | 继承、多态、`virtual`、抽象基类。          |
| **第16章**​ | 模板与泛型编程 | 函数模板、类模板，了解原理即可。               |

### 第五部分：专家工具（第17-19章）

> **目标**：按需查阅，不急于一时。



# 第2章 变量和基本类型

举例

```cpp
#include <iostream>
#include <typeinfo> // 用于 typeid
using namespace std;

int main() {
    // 1. 引用：必须是变量别名，必须初始化
    int val = 10;
    int &ref = val;  // ref 是 val 的别名
    ref = 20;        // val 也变成 20

    // 2. const 引用（非常重要！）
    const int &r1 = val;  // 常量引用可以绑定到普通变量
    // r1 = 30;           // 错误！不能通过 const 引用修改

    // 3. auto 类型推导（C++11）
    auto x = 5;        // x 是 int
    auto y = 3.14;     // y 是 double
    auto &z = val;     // z 是 int&（引用需要显式声明）
    
    // 4. decltype：推断表达式的类型但不执行
    decltype(val) a = 0;    // a 的类型和 val 一样，是 int
    decltype((val)) b = a;  // 双层括号，b 是 int&

    cout << "val=" << val << ", ref=" << ref << endl;
    cout << "x type: " << typeid(x).name() << endl;

    return 0;
}
```

## 2.1 `using namespace std;` 是什么意思？

### 1. 先理解 `namespace`（命名空间）

C 语言没有命名空间的概念，所有函数、变量都在全局作用域。如果你写了一个函数叫 `max()`，同时又 `#include <algorithm>`（里面也有 `std::max()`），就会冲突。

C++ 引入了 **`namespace`**​ 来解决这个问题。标准库的所有东西都被放在了一个叫 **`std`**（standard）的命名空间里：

```
std::cout
std::string
std::vector
```

### 2. `using namespace std;` 的作用

这行代码的意思是：**"把 `std` 命名空间里的所有名字，全部引入到当前作用域。"**

引入之后，你就可以**省略 `std::` 前缀**：

```
// 不用 using namespace std;
std::cout << "Hello" << std::endl;

// 用了 using namespace std;
cout << "Hello" << endl;  // 编译器会自动去 std 里找
```

### 3.隐患：为什么项目里不推荐这么写？

#### 隐患 1：命名冲突（Name Collision）

这是最大的问题。`std` 里有**成千上万个**名字。如果你自己定义的变量/函数/类名，恰好和 `std` 里的某个名字一样，就会冲突。

示例：

```
#include <iostream>
#include <vector>

using namespace std;

// 你自己写了一个 count 函数
int count(const vector<int>& v) {
    return v.size();
}

int main() {
    vector<int> v = {1, 2, 3};
    cout << count(v) << endl;  // 你想调用自己的 count
    // 但编译器可能调用 std::count（算法库里的）
    // 或者报歧义错误：call of overloaded 'count' is ambiguous
    return 0;
}
```

如果加上 `#include <algorithm>`（里面有 `std::count`），编译器直接报错，因为两个 `count` 都匹配。

#### 隐患 2：头文件里用 = 灾难

如果你在 **`.h` 头文件**里写了 `using namespace std;`，那么**所有包含这个头文件的 `.cpp` 文件都会被污染**。这个污染会层层传递，最终可能导致整个项目出现莫名其妙的命名冲突，而且你很难定位到底是谁引入的。

```
// mylib.h  <-- 千万别这么干！
using namespace std;

void foo(string s);  // 实际上是 std::string
```

```
// main.cpp
#include "mylib.h"
// 现在 main.cpp 里也自动引入了整个 std 命名空间
// 如果 main.cpp 里也有命名冲突，编译器不会提示是 mylib.h 导致的
```

#### 隐患 3：代码可读性降低

写 `std::cout` 虽然多打了几个字，但它**明确告诉读代码的人**：这是标准库的东西，不是你项目里自定义的。在大型项目中，这种显式声明非常重要。



### 最佳实践（推荐写法）

#### ✅ 写法 1：显式写 `std::`（最推荐，工业级标准）

```
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3};
    std::cout << v.size() << std::endl;
    return 0;
}
```

- **优点**：零污染，零歧义，代码意图清晰。
- **缺点**：多打几个字（但 IDE 有自动补全，其实很快）。

#### ✅ 写法 2：只引入你需要的名字（头文件中也安全）

```
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main() {
    vector<int> v = {1, 2, 3};
    cout << v.size() << endl;
    return 0;
}
```

- **优点**：减少打字量，同时只引入需要的名字，污染范围可控。
- **适用场景**：`.cpp` 文件里用得最多。

#### ⚠️ 写法 3：`using namespace std;` 仅在 `.cpp` 文件里小范围使用

```
#include <iostream>
#include <vector>

// 只在 .cpp 文件里用，且不在全局作用域，而在函数内部
int main() {
    using namespace std;  // 作用域仅限于 main 函数
    vector<int> v = {1, 2, 3};
    cout << v.size() << endl;
    return 0;
}
```

- **优点**：把污染限制在函数内部，不影响其他函数。
- **适用场景**：小程序、算法竞赛、练习代码。

#### 总结对照表

| 写法                          | 适合场景     | 风险          |
| --------------------------- | -------- | ----------- |
| `using namespace std;` 在全局  | 练习/小程序   | ❌ 高（命名污染）   |
| `using namespace std;` 在函数内 | 小程序      | ⚠️ 低（仅限函数内） |
| `using std::cout;` 等局部引入    | 项目开发     | ✅ 低         |
| 显式 `std::xxx`               | 项目开发/库开发 | ✅ 零         |

## 2.2 decltype

decltype 是 C++11 引入的一个编译期类型推导关键字。它的名字来源于 declare type（声明类型）。

一句话概括：decltype(表达式) 返回的是“这个表达式本身的类型”，但编译器不会去执行这个表达式。

#### 1、和 auto 的区别（先搞清楚这个）

你已经有 auto 的基础了，它们俩很容易混：




| **关键字**    | **作用**                     | **是否需要初始化**       |
| ---------- | -------------------------- | ----------------- |
| `auto`     | 根据**初始化值**推导变量类型           | ✅ 必须初始化           |
| `decltype` | 根据**给定的表达式**推导类型，可以只声明不初始化 | ❌ 不需要（但定义变量时通常需要） |

```cpp
int x = 10;

auto a = x;        // a 的类型是 int（因为 x 的值是 int）
decltype(x) b;     // b 的类型是 int（因为 x 的类型是 int），b 没有初始化
decltype(x) c = 5; // c 的类型是 int，初始化为 5
```

**核心区别**：`auto` 看的是"值"，`decltype` 看的是"表达式本身的类型声明"。

#### 2、decltype 的三条核心规则

这是理解 decltype 的关键，也是 C++ Primer 第2章的重点。

##### 规则 1：如果 decltype 后面跟的是一个变量名 → 推导出的类型就是该变量的声明类型（包括 const 和引用）。

```cpp
const int ci = 0;
const int& cr = ci;

decltype(ci) x = 0;   // x 的类型是 const int
decltype(cr) y = x;   // y 的类型是 const int&（保留引用！）
// y = 10;            // 错误，y 是 const 引用，不能修改
```

⚠️ **和 `auto` 的重大区别**：`auto` 会**忽略引用和顶层 const**，但 `decltype` **保留一切**。

```cpp
auto a1 = ci;     // a1 是 int（const 被忽略）
decltype(ci) d1;  // d1 是 const int（const 被保留）
```

##### 规则 2：如果 `decltype` 后面跟的是一个**表达式**（非单纯变量名）

→ 推导出的类型是**该表达式返回值的类型**。

```cpp
int x = 10;
int* px = &x;

decltype(x + 1) a;  // a 的类型是 int（x+1 返回 int）
decltype(*px) b;    // b 的类型是 int&（解引用返回左值引用！）
decltype(px) p;     // p 的类型是 int*（px 是变量，走规则1）
```

**特别注意解引用**：`*px` 是一个表达式，解引用操作返回的是"指向对象的左值引用"，所以 `decltype(*px)` 是 `int&`，不是 `int`。



##### 规则 3：双层括号 `decltype((variable))`

→ **永远是引用类型**



# 第3章 字符串、向量和数组

## 3.1 `std::string` —— 再也不用碰 `char*` 了

#### 初始化方式（C++ 比 C 灵活很多）

```cpp
#include <iostream>
#include <string>

int main() {
    // 1. 默认初始化（空字符串）
    std::string s1;

    // 2. 用字面量初始化
    std::string s2 = "Hello";
    std::string s3("World");

    // 3. 重复字符初始化
    std::string s4(5, 'A');  // "AAAAA"

    // 4. 拷贝初始化
    std::string s5 = s2;     // s5 是 "Hello"

    std::cout << s1 << "|" << s2 << "|" << s3 << "|" << s4 << "|" << s5 << std::endl;
    return 0;
}
```

**对比 C 语言**：你不需要关心缓冲区大小，不需要 `strcpy`，不用担心溢出。

#### 常用操作速查

```cpp

```
