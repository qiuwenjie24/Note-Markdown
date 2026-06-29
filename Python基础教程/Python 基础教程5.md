# Python 基础教程5

# 程序组织与扩展

## 文件操作

### 打开文件

使用 `open()` 函数，可以选择不同的模式打开文件。

`open()` 可以选择的模式有：

| 模式   | 作用                           | 文件不存在时                  | 文件存在时               |
| :----- | :----------------------------- | :---------------------------- | :----------------------- |
| `'r'`  | **读取**（只读，默认）         | 报错 `FileNotFoundError`      | 正常读取                 |
| `'w'`  | **写入**（覆盖）               | **创建新文件**                | **清空旧内容**，重新写入 |
| `'a'`  | **追加**（在末尾添加）         | **创建新文件**                | 在文件末尾追加内容       |
| `'r+'` | **读写**（可读可写）           | 报错                          | 从头覆盖或读取           |
| `'b'`  | **二进制模式**（如图片、视频） | 配合使用，如 `'rb'` 或 `'wb'` |                          |

**编码警告**：操作文本文件时，指定 `encoding='utf-8'` 是避免中文乱码的关键。

例子：

```python
# 以‘只读’的模式打开data.txt文件
with open('data.txt', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content)
```

其中 `f` 表示打开的文件，它是一个迭代器。

### 读取文件

读取文件的方式有三种：

| 方法            | 返回值                       | 适用场景                       |
| :-------------- | :--------------------------- | :----------------------------- |
| `f.read()`      | 整个文件内容（一个大字符串） | 小文件                         |
| `f.readline()`  | 一行内容（字符串）           | 逐行处理，内存友好             |
| `f.readlines()` | 所有行（字符串列表）         | 需要获取所有行，或配合索引操作 |

例子：

```python
# 假设文件 content.txt 内容为：
# 第一行
# 第二行
# 第三行

with open('content.txt', 'r', encoding='utf-8') as f:
    # 方式1：read() —— 一次性读取全部内容
    content = f.read()
    print(content)   # 输出整个文件内容（一个字符串）
    
    # 方式2：readline() —— 每次只读取一行（类似与迭代器）
    line1 = f.readline()   # "第一行\n"
    line2 = f.readline()   # "第二行\n"
    
    # 方式3：readlines() —— 一次性读取所有行，返回列表
    lines = f.readlines()  # ['第一行\n', '第二行\n', '第三行\n']
```

**其他读取方式：直接遍历文件对象**

```python
with open('data.txt', 'r', encoding='utf-8') as f:
    for line in f:          # 直接遍历文件对象
        print(line.strip())
```

它兼具了 `readline()` 的内存效率和更简洁的语法。

### 写入文件

例子1：写入（覆盖）

```python
# 如果文件不存在，会自动创建
with open('output.txt', 'w', encoding='utf-8') as f:
    f.write('第一行内容\n')   # 必须手动加换行符 \n
    f.write('第二行内容\n')
```

注意：`'w'` 模式会清空原文件。如果不希望清空，使用 `'a'` 追加模式。

例子2：追加（末尾添加）

```python
with open('log.txt', 'a', encoding='utf-8') as f:
    f.write('2026-06-23: 用户登录成功\n')
```

## 异常处理

**异常（Exception）** 是程序运行时发生的错误信号。当 Python 遇到无法继续执行的情况时，会"抛出"一个异常。

异常处理是 Python 中**让程序在出错时不会崩溃，而是优雅地处理错误**的机制。

### 异常处理的基本结构

```python
try:
    # 尝试执行的代码（可能会出错）
    可能出错的代码
except 异常类型1:
    # 发生异常类型1时的处理
    处理代码
except 异常类型2:
    # 发生异常类型2时的处理
    处理代码
except Exception as e:
    # 捕获所有其他异常
    print(f"未知错误：{e}")
else:
    # 没有发生任何异常时执行（可选）
    正常执行的代码
finally:
    # 无论是否发生异常都执行（可选）
    一定会执行的代码
```

例子：

```python
try:
    # 1. 尝试执行可能会出错的代码
    x = int(input("请输入数字："))
    result = 100 / x
    print(f"结果：{result}")

except ValueError:
    # 2. 捕获 ValueError 类型的异常
    print("❌ 输入的不是有效数字")

except ZeroDivisionError:
    # 3. 捕获 ZeroDivisionError 类型的异常
    print("❌ 不能除以零")

except Exception as e:
    # 4. 捕获所有其他异常（Exception 是所有异常的父类）
    print(f"❌ 发生了未知错误：{e}")

else:
    # 5. 没有异常时执行（可选）
    print("✅ 计算成功！")

finally:
    # 6. 无论是否有异常都执行（可选）
    print("程序结束")
```

### Python 中常见的异常类型

| 异常类型            | 触发场景      | 示例                  |
| :------------------ | :------------ | :-------------------- |
| `ZeroDivisionError` | 除以零        | `10 / 0`              |
| `ValueError`        | 值错误        | `int('abc')`          |
| `TypeError`         | 类型错误      | `'1' + 1`             |
| `IndexError`        | 索引越界      | `[1,2][5]`            |
| `KeyError`          | 键不存在      | `{'a':1}['b']`        |
| `FileNotFoundError` | 文件不存在    | `open('no.txt')`      |
| `PermissionError`   | 权限不足      | 读取系统保护文件      |
| `AttributeError`    | 属性不存在    | `'abc'.length`        |
| `NameError`         | 变量不存在    | `print(x)`（x未定义） |
| `KeyboardInterrupt` | 用户按 Ctrl+C | 用户中断程序          |

## 模块与包

模块和包是 Python 组织代码的方式。当你在一个 `.py` 文件中写代码时，你就在写一个**模块**；当你把多个模块放在一个文件夹里，就形成了一个**包**。

### 模块（Module）

模块就是一个 `.py` 文件，文件名就是模块名。

例子：

```python
# 文件：calculator.py（这是一个模块）
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

PI = 3.14159
```

**为什么需要模块？**

| 问题                 | 模块的解决方案               |
| :------------------- | :--------------------------- |
| 代码太长，难以维护   | 拆分到多个文件，按功能分类   |
| 多个程序需要共用代码 | 一个模块被多个程序导入使用   |
| 命名冲突             | 每个模块有独立命名空间       |
| 团队协作             | 不同人负责不同模块，互不干扰 |

#### 导入模块

Python 提供了多种导入方式：

（以前面建立的 `calculator.py` 模块为例）

```python
# 1. 导入整个模块
import calculator
print(calculator.add(3, 5))      # 8
print(calculator.PI)             # 3.14159

# 2. 导入特定内容
from calculator import add, PI
print(add(3, 5))                 # 8
print(PI)                        # 3.14159
# print(subtract)                # ❌ 未导入，报错

# 3. 导入全部内容（不推荐）
from calculator import *
print(add(3, 5))                 # 8
# 问题：不知道哪些名字被导入了，容易覆盖同名变量

# 4. 起别名
import calculator as calc
print(calc.add(3, 5))            # 8

import calculator.add as a
print(a(3, 5))                   # 8
```

#### 模块的搜索路径

当你执行 `import calculator` 时，Python 做三件事：

1. **查找**：在搜索路径中寻找 `calculator.py`
2. **执行**：执行该文件中的所有代码（创建函数、变量等）
3. **创建命名空间**：将模块中的名字放入 `calculator` 命名空间

**搜索路径的查找顺序：**

1. 当前工作目录
2. `PYTHONPATH` 环境变量中的目录
3. Python 安装目录中的标准库

**以相对路径搜索：**

```text
项目目录/
├── main.py
└── model/
    ├── __init__.py
    ├── calculator.py
    └── utils.py      ← 在这里写 from .model import calculator
```

绝对导入（基于当前工作目录或 `sys.path`）：

```
# main.py
# 在 main.py 文件所在的档期那目录寻找 model 模块/包
from model import calculator 
```

相对导入（基于当前文件的位置）：

```
# 在 utils.py 中
from . import calculator          # 同级目录下的 calculator
from .calculator import add       # 直接导入模块中的函数
```

**相对导入的规则**

| 写法  | 含义     | 适用场景                      |
| :---- | :------- | :---------------------------- |
| `.`   | 当前包   | `from . import calculator`    |
| `..`  | 上一级包 | `from .. import config`       |
| `...` | 上两级包 | `from ...utils import helper` |

**注意：主程序（直接运行的），如 `main.py`，只能用绝对导入，其他被调用的模块可以用相对导入（作为包的一部分运行）。**

**相对导入只在“作为包被调用”时有效，不能直接运行。**

### 包（Package）

包是**包含多个模块的文件夹**。它让模块有层次结构，避免不同模块同名冲突。

**包的标志**：文件夹中必须有一个 `__init__.py` 文件（可以是空文件）。

```reStructuredText
my_package/                # 包名
├── __init__.py            # 包的标志（可以是空文件）
├── math_utils.py          # 模块
│   ├── add()
│   └── multiply()
├── string_utils.py        # 模块
│   ├── capitalize()
│   └── reverse()
└── validators/            # 子包
    ├── __init__.py
    ├── email.py
    └── phone.py
```

**`__init__.py` 有两个作用：**

1. **标记这个文件夹是一个 Python 包**
2. **当包被导入时，先执行 `__init__.py` 中的代码**

例如：

```python
# my_package/__init__.py
print("my_package 被导入！")

# 可以在 __init__.py 中暴露接口，简化导入
from .math_utils import add, multiply
from .string_utils import capitalize
# .math_utils表示在当前文件所在目录寻找math_utils
```

简化使用：

```python
# 使用：导入包即可直接使用
import my_package
my_package.add(3, 5)         # 不需要写 my_package.math_utils.add
```

#### 包的导入方式

```python
# 1. 导入整个包
import my_package
my_package.math_utils.add(3, 5)

# 2. 导入包中的特定模块
from my_package import math_utils
math_utils.add(3, 5)

# 3. 从模块中导入特定函数（前提是 __init__.py 暴露了）
from my_package import add, multiply
add(3, 5)

# 4. 相对导入（包内部使用）
# 在 validators/email.py 中导入同目录的 phone.py
from . import phone
# 或
from .phone import validate
```

### `__name__` 与 `if __name__ == "__main__"`

#### 文件的使用

一个 `.py` 文件（如 `calculator.py`）只有两种使用方式：

| 使用方式           | 命令                   | 场景                       |
| :----------------- | :--------------------- | :------------------------- |
| **作为主程序运行** | `python calculator.py` | 直接执行这个文件           |
| **作为模块被导入** | `import calculator`    | 其他文件引用这个文件的功能 |

#### `__name__` 变量

Python 在运行每个 `.py` 文件时，会自动给文件设置一个内置变量 `__name__`，它的值由文件的使用方式决定：

| 使用方式       | `__name__` 的值      |
| :------------- | :------------------- |
| 作为主程序运行 | `"__main__"`         |
| 作为模块被导入 | 文件名（不含 `.py`） |

例子：

```python
# 文件：calculator.py
print(f"__name__ 的值是：{__name__}")

# 直接运行 python calculator.py
# 输出：__name__ 的值是：__main__

# 在另一个文件中 import calculator，通过运行另一个文件调用该文件
# 输出：__name__ 的值是：calculator
```

`__name__` 就是 Python 给每个文件的“身份标识”——它告诉这个文件：“你现在是主角（`__main__`），还是配角（模块名）”

#### `if __name__ == "__main__"`

有时候我们希望让代码在不同的使用场景（以主程序运行或作为模块导入）下执行不同的逻辑，例如：

- **作为主程序运行**：执行测试代码、演示代码
- **作为模块被导入**：只提供函数/类定义，不执行测试代码

有了 `__name__` 这个标识，我们就可以用 `if` 语句来控制哪些代码在哪种场景下执行：

```python
# 文件：calculator.py

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

# 这段代码只在直接运行时才执行
if __name__ == "__main__":
    # 测试代码
    print("测试加法：", add(3, 5))
    print("测试减法：", subtract(10, 4))
```



### 常用模块导入模式

导入顺序约定

```python
# 1. 标准库（Python 自带的）
import os
import sys

# 2. 第三方库（pip 安装的）
import numpy as np
import requests

# 3. 自己写的模块
import my_utils
from my_project import config
```



## 迭代器与生成器

迭代器和生成器是 Python 中**处理数据流**的机制。它们的核心价值是：**惰性求值（Lazy Evaluation）**——需要时才计算，不提前把所有数据都放在内存里。

### 迭代器

迭代器是一个**可以逐个返回元素的对象**，它有两个特点：

1. 不关心数据总量有多大（只有当前迭代返回值占内存）
2. 每次只返回一个元素，用完就丢弃

### 迭代器协议

一个对象要成为迭代器，必须实现两个方法：

| 方法         | 作用                                           |
| :----------- | :--------------------------------------------- |
| `__iter__()` | 返回迭代器自身                                 |
| `__next__()` | 返回下一个元素，没有元素时抛出 `StopIteration` |

例子：手动实现一个倒计时到0的迭代器。

```python
# 手动实现一个迭代器
class CountDown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        # 终止条件
        if self.current < 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

# 使用
for num in CountDown(5):
    print(num, end=" ")   # 5 4 3 2 1 0
```

#### 常见的迭代器



#### 判断一个对象是否是迭代器

通过 `isinstance()` 函数和 `Iterator` ：

```python
from collections.abc import Iterator

print(isinstance([1, 2, 3], Iterator))   # False（列表不是迭代器）
print(isinstance(iter([1, 2, 3]), Iterator))  # True（iter() 将其转为迭代器）
print(isinstance(open('file.txt'), Iterator)) # True
```

#### 可迭代对象（Iterable）vs 迭代器（Iterator）

| 概念                       | 定义                                 | 举例                         |
| :------------------------- | :----------------------------------- | :--------------------------- |
| **可迭代对象（Iterable）** | 实现了 `__iter__()`，能被 `for` 遍历 | 列表、元组、字典、字符串     |
| **迭代器（Iterator）**     | 实现了 `__iter__()` + `__next__()`   | 文件对象、生成器、`map` 对象 |

**关系**：迭代器一定是可迭代对象，可迭代对象不一定是迭代器。

```python
# 用 iter() 将可迭代对象转为迭代器
numbers = [1, 2, 3]        # 可迭代对象，但不是迭代器
it = iter(numbers)         # 转为迭代器

print(next(it))   # 1
print(next(it))   # 2
print(next(it))   # 3
print(next(it))   # StopIteration（耗尽）
```

#### 迭代器与 `for` 循环的工作原理

 `for` 循环的工作原理是：通过 `iter()` 拿到一个独立的迭代器对象，由这个迭代器来记录遍历位置。

```python
# for 循环的本质
for x in iterable:
    print(x)

# 等价于
it = iter(iterable)   # 获取迭代器（将可迭代对象变为迭代器）
while True:
    try:
        x = next(it)   # 不断获取下一个元素
        print(x)
    except StopIteration:
        break          # 耗尽则退出
```

### 生成器

生成器是**用 `yield` 关键字定义的特殊函数**。它本质上是迭代器，但写法更简洁，是迭代器的"免模板"版本。

```python
# 用迭代器写倒计时（之前写的）
class CountDown:
    def __init__(self, start):
        self.current = start
    def __iter__(self):
        return self
    def __next__(self):
        if self.current < 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value
# =====================================================
# 用生成器写倒计时（等价迭代器写法）
def count_down(start):
    while start >= 0:
    # yield 让一个函数变成了生成器，让它拥有“暂停”的能力
        yield start 
        start -= 1

# 使用方式和迭代器完全一样
for num in count_down(5):
    print(num, end=" ")   # 5 4 3 2 1 0
```

#### `yield` 的执行机制

**`yield` 不会像 `return` 那样立即结束函数，而是“暂停”并“记住当前”。**

例子：逐步追踪生成器的执行过程

```python
# 生成器样例
def demo():
    print("→ 开始执行")
    yield 1
    print("→ 继续执行")
    yield 2
    print("→ 即将结束")
    yield 3
    print("→ 彻底结束")

gen = demo()
```

每调用一次 `next(gen)`，函数就执行到下一个 `yield` 并暂停：

```python
next(gen)   # 执行到第1个 yield
# 输出：→ 开始执行
# 返回：1

next(gen)   # 从第1个 yield 之后继续执行到第2个 yield
# 输出：→ 继续执行
# 返回：2

next(gen)   # 从第2个 yield 之后继续执行到第3个 yield
# 输出：→ 即将结束
# 返回：3

next(gen)   # 从第3个 yield 之后继续执行到函数结束
# 输出：→ 彻底结束
# 抛出：StopIteration
```

**注意：**

1. **生成器函数只是“蓝图”**：调用生成器函数，如 `demo()` ，不会执行函数体，只返回一个生成器对象（如 `gen`）。真正的执行发生在调用 `next(gen)` 的时候。
2. **恢复现场是“自动的”**：函数每次暂停时，会保存当前所有局部变量的状态、当前执行到哪一行。下次 `next(gen)` 调用时，Python 自动从上次暂停的位置精确恢复，仿佛时间从未中断。

#### 生成器的两种写法

**方式一：生成器函数（`yield`）**

```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

for num in fibonacci(10):
    print(num, end=" ")   # 0 1 1 2 3 5 8 13 21 34
```

**方式二：生成器表达式（类似列表推导式，但用圆括号）**

```python
# 列表推导式 → 立即生成所有数据
squares_list = [x**2 for x in range(10)]        # 立即计算，占用内存

# 生成器表达式 → 惰性生成
squares_gen = (x**2 for x in range(10))         # 不立即计算，返回生成器对象

# 使用时才逐个计算
for num in squares_gen:
    print(num, end=" ")   # 0 1 4 9 16 25 36 49 64 81
```

### 迭代器 vs 生成器 vs 可迭代对象

| 概念                       | 关键特征                           | 举例                            |
| :------------------------- | :--------------------------------- | :------------------------------ |
| **可迭代对象（Iterable）** | 实现了 `__iter__()`                | 列表、元组、字典、字符串        |
| **迭代器（Iterator）**     | 实现了 `__iter__()` + `__next__()` | 文件对象、`iter(list)` 的返回值 |
| **生成器（Generator）**    | 用 `yield` 定义的函数              | `def gen(): yield 1`            |

**关系图：**

```reStructuredText
可迭代对象（Iterable）
    │
    ├── 列表、元组、字典、字符串...
    │
    └── 迭代器（Iterator）
            │
            ├── 文件对象
            ├── map、filter、zip 对象
            ├── 生成器（Generator）
            │       ├── 生成器函数（yield）
            │       └── 生成器表达式（(x for x in range(10))）
            └── 手动实现的迭代器类
```



## 装饰器

装饰器是 Python 中**在不修改原函数代码的前提下，给函数增加额外功能**的工具。它是 Python 最优雅的特性之一。

简单例子：



### 装饰器的本质

装饰器本质上是**一个接收函数、返回新函数的函数**。

基本语法：

```python
def timer(func):           # 1. 接收一个函数作为参数
    def wrapper(*args, **kwargs):   # 2. 定义一个包装函数
        start = time.time()
        result = func(*args, **kwargs)   # 3. 调用原函数
        end = time.time()
        print(f"耗时：{end - start:.2f}秒")
        return result
    return wrapper         # 4. 返回包装函数

# @timer 的作用等价于：
# slow_function = timer(slow_function)
```



