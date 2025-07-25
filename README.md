# OmniInt: 高精度整数计算库

![Language](https://img.shields.io/badge/language-C%2B%2B-blue.svg)
![Standard](https://img.shields.io/badge/C%2B%2B-11%2B-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

**OmniInt** 是一个轻量级、仅含头文件的 C++ 库，用于进行任意大小的整数（高精度）算术运算。它旨在克服 C++ 内置整数类型（如 `int`, `long long`）的范围限制，让大数计算变得像操作普通整数一样简单直观。

**重要提示：目前本项目仍旧有计算错误的问题，不能用于生产环境**

---

## 目录

- [主要特性](#主要特性)
- [快速开始](#快速开始)
- [用法示例](#用法示例)
- [构建与测试](#构建与测试)
- [未来计划](#未来计划)
- [版权与许可](#️-版权与许可)

---

## 主要特性

-   **任意精度**：支持超出 `long long` 范围的极大或极小整数。
-   **完整的运算符重载**：
    -   **算术运算**: `+`, `-`, `*`, `/`, `%`
    -   **关系运算**: `==`, `!=`, `<`, `>`, `<=`, `>=`
    -   **复合赋值**: `+=`, `-=`, `*=`, `/=`, `%=`
    -   **一元运算**: `-` (负号), `++`, `--` (前缀/后缀)
-   **易于使用的接口**：
    -   可通过 `long long` 和 `std::string` 进行构造和赋值。
    -   支持标准的输入/输出流操作 (`<<` 和 `>>`)。
-   **数学函数**：
    -   内置高效的整数平方根函数 `sqrt()`。
    -   内置基于二进制算法的高性能最大公约数函数 `gcd()`。
-   **异常安全**：在遇到除以零、类型转换溢出等错误时，会抛出标准异常。
-   **易于集成**：仅需一个头文件 (`OmniInt.h`) 即可集成到您的项目中。

## 快速开始

### 依赖要求

-   一个支持 C++11 或更高标准的 C++ 编译器 (例如 g++, Clang++)。

### 如何使用

1.  将 `OmniInt.h` 文件复制到您的项目目录中。
2.  在您的 C++ 源文件中包含该头文件：

    ```cpp
    #include "OmniInt.h"
    ```

3.  开始使用 `OmniInt` 对象进行计算！

    ```cpp
    #include <iostream>
    #include "OmniInt.h"

    int main() {
        // 从字符串创建非常大的整数
        OmniInt a("12345678901234567890");
        OmniInt b("98765432109876543210");

        // 直接进行算术运算
        OmniInt sum = a + b;
        OmniInt product = a * 5; // 可与普通整数混合运算

        std::cout << "Number a: " << a << std::endl;
        std::cout << "Number b: " << b << std::endl;
        std::cout << "Sum: " << sum << std::endl;
        std::cout << "Product (a * 5): " << product << std::endl;

        return 0;
    }
    ```

## 用法示例

### 初始化

您可以使用多种方式创建 `OmniInt` 对象。

```cpp
// 默认构造，值为 0
OmniInt a;

// 从 long long 构造
OmniInt b(12345LL);
OmniInt c(-98765);

// 从字符串构造（推荐的明确写法）
OmniInt d("123456789012345678901234567890");
OmniInt e("-98765432109876543210");
```

### 算术运算

所有基本算术运算符均已重载。

```cpp
OmniInt x("1000");
OmniInt y("33");

OmniInt quotient = x / y;   // 结果: 30
OmniInt remainder = x % y;  // 结果: 10
OmniInt product = x * y;    // 结果: 33000
```

### 关系比较

可以像比较内置整数一样比较 `OmniInt` 对象。

```cpp
OmniInt big("999999");
OmniInt small("99999");

if (big > small) {
    std::cout << "big is indeed bigger!" << std::endl;
}
```

### 数学函数

#### 整数平方根 (sqrt)

使用全局 `sqrt` 函数计算整数平方根（向下取整）。

```cpp
OmniInt n("98765432109876543210");
OmniInt root = sqrt(n);
// root 的值为 "9938079900"
std::cout << "The integer square root of " << n << " is " << root << std::endl;
```

#### 最大公约数 (gcd)

使用全局 `gcd` 函数计算两个数的最大公约数。

```cpp
OmniInt u("60");
OmniInt v("48");
OmniInt common_divisor = gcd(u, v);
// common_divisor 的值为 "12"
std::cout << "The GCD of " << u << " and " << v << " is " << common_divisor << std::endl;
```

## 构建与测试

项目附带一个全面的测试程序 `test_omniint.cpp`，用于验证库的所有功能是否正确。如果您在测试中发现任何失败 (`FAIL`)，欢迎提交 PR 或 Issues。

1.  **编译测试程序**:
    确保 `OmniInt.h` 和 `test_omniint.cpp` 在同一目录下。打开终端并执行以下命令：

    ```bash
    # 使用 g++
    g++ -std=c++11 -O2 -Wall -o test_runner test_omniint.cpp

    # 或者使用 Clang
    clang++ -std=c++11 -O2 -Wall -o test_runner test_omniint.cpp
    ```
    这会生成一个名为 `test_runner` 的可执行文件。

2.  **运行测试**:

    ```bash
    ./test_runner
    ```

    如果所有测试都通过，您将看到一个包含 `Passed: 86, Failed: 0` 的摘要。

## 未来计划

-   **性能优化**
    -   将内部存储从 `base 10` 升级为更大的基数 (如 `base 2^32`)，以大幅减少内存占用并提升大规模计算的速度。
-   **功能扩展**
    -   添加位运算符 (`&`, `|`, `^`, `<<`, `>>`)。
    -   实现更多数学函数，如 `pow()` (幂运算)、`lcm()` (最小公倍数) 等。

## ©️ 版权与许可

Copyright (c) 2025 SharkyMew

本项目在 [MIT许可证](https://opensource.org/licenses/MIT) 下发布。
