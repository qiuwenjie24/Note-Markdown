# Python 基础教程1

# 绪论

## 简介

**Python** 是一种**解释型、高级、通用**的编程语言，由 Guido van Rossum 于 1991 年首次发布。

Python 3.0 版本进行了大更新，通常说的python都是指python3，即3.0之后的版本。

源文件扩展名： `.py`。

**核心特点：**

- **简洁易读**：语法清晰，使用缩进表示代码块，非常接近自然语言
- **动态类型**：变量无需声明类型，赋值时自动确定
- **解释型语言**：代码由解释器逐行读取并执行，无需预先编译成机器码。
- **自动内存管理**：有垃圾回收机制，不用手动管理内存
- **跨平台**：Windows、macOS、Linux 等系统均可运行
- **丰富的生态**：拥有庞大的标准库和第三方包，可以调用 C/C++ 编写的模块

**应用领域：**

- 数据分析与科学计算（Pandas、NumPy）
- 人工智能与机器学习（TensorFlow、PyTorch）
- Web开发（Django、Flask）
- 自动化脚本与爬虫
- 软件开发与测试



## 环境配置

一般运行python需要搭建python环境和编辑器（编译器/解释器/IDE），这里我们选择的编辑器为VScode。

### 下载安装

步骤：

1. Python环境下载

   - 进入 [Python官网](https://www.python.org/) 
   - 选择 `Downloads` --> `Windows` ，这里提供了看到各种版本的python下载
   - 在 `Stable Releases(稳定版)` 一栏中选择最新的安装程序，通常为 `Windows installer (64-bit)` 

2. Python环境安装

   - 双击下载好的安装程序 `Windows installer` ，并点击 `运行`。
   - 选择自定义安装路径 `Customize installation` ，并勾选选项（如`Add python.exe to PATH`）。
   - 之后一直选择 `Next` ，选项按默认就可以了，若其他需求在自行调整选项。
   - 安装时相应选项的解释可以参考 [python安装教程-CSDN博客](https://blog.csdn.net/thefg/article/details/128601410) 。

3. VS code下载

   - 进入 [Visual Studio Code 官网](https://code.visualstudio.com/) 
   - 直接点击 `Windows` 下载，或者选择 `System Installers x64` 或 `User Installers x64` 下载。

4. VS code安装

   - 双击下载好的安装程序。 
   - 安装过程中注意安装路径设置，以及环境变量默认自动添加到系统中，剩下的直接点击 `Next` 即可。
   - 选项的解释可以参考 [最新VS Code安装详细教程及vs code配置-CSDN博客](https://blog.csdn.net/thefg/article/details/131752996) 

5. 在VS code中安装插件

   - 中文界面插件：拓展商店中搜索 `Chinese`，选择 `Chinese (Simplified) (简体中文) Language` 扩展，安装后重新打开VS code即可实现中文界面。
   - python插件：VS code的拓展商店中搜索 `Python` 并点击下载安装，这时VS code才可以自动关联、调用python解释器，运行程序代码。

6. pip下载 python库

   - python库（如`numpy` 、`Eigen`库等）的安装和卸载需要pip。在安装python过程时，如果勾选了`pip` （默认勾选），则不需要额外下载。
   - 在终端输入`pip –-version`可以查看pip的版本信息。
   - `pip list`查看已安装的python库。
   - `python -m pip install --upgrade pip`更新pip版本到最新。
   - `pip install <库的名称>`安装python库，如`pip install numpy`。
   - `pip uninstall <库的名称>`卸载python库。

   注：如果电脑下载有 Anaconda，也可以使用它代替 pip 下载和管理python库。关于Anaconda在pytorch相关的笔记中有介绍。

### Vscode中切换不同环境

Anaconda可以处理管理python库，还可以在本地划分一个新的虚拟环境。假如你有一个新的虚拟环境，并且在新的环境下也下载了python解释器。可以通过以下步骤，切换到新的环境下的python解释器：

1. 使用快捷键`ctrl+shift+P`打开命令面板。

2. 在命令面板中输入`Python: Select Interpreter`，然后选择该选项。

3. 在弹出的列表中，会显示系统和所有环境中可用的python解释器，选择你想要环境的python解释器。

4. 选择后，Vscode会自动切换到新的环境，并在编辑器底部的状态栏(右下角)显示当前激活的环境。

   状态栏中`{} python`表示当前使用的python语言，`3.10.16('pytorch_cpu')`表示当前处在虚拟环境`pytorch_cpu`中，并且使用的是3.10.16版本的python。

   如果后续想要切换成其他环境中的python解释器，可以点击`3.10.16('pytorch_cpu')`，在弹出的列表中选择想要切换的python解释器。

5. 如果在弹出的列表中没有找到你想要的环境，那么你可以点击点击列表中的`Enter interpretet path`-->`Find`-->在你想要的环境的路径下选择python解释器`python.exe`。

### Vscode快捷键

Vscode 快捷键的设置可以通过菜单栏的 `Code > 首选项 > 键盘快捷方式` 查看或修改。

1. 中止/停止运行的快捷键： `ctrl+c` 
2. 调试运行快捷键：`F5` 
3. 注释行：`Ctrl + /` 
4. 运行编译任务：`Ctrl + Shift + B` 
5. 代码整体左右移：`Ctrl + [`（左移）， `Ctrl + [`（右移）



## 知识结构

```reStructuredText
┌─────────────────────────────────────────────────────────────────┐
│  第六层：专用领域库（按需学习）                                 │
│  • 数据分析：NumPy、Pandas、Matplotlib                         │
│  • Web开发：Django、Flask                                      │
│  • 人工智能：PyTorch、TensorFlow                               │
│  • 网络爬虫：requests、Scrapy                                  │
├─────────────────────────────────────────────────────────────────┤
│  第五层：标准库（工具集，拿来就用）                             │
│  • 常用模块：math、random、datetime、json、re                  │
│  • 系统交互：os、sys、subprocess                               │
│  • 数据存储：pickle、sqlite3                                   │
│  • 网络请求：urllib、socket                                    │
├─────────────────────────────────────────────────────────────────┤
│  第四层：程序组织与扩展（完善与扩展代码）                       │
│  • 文件操作（open、read、write、with）                        │
│  • 异常处理（try-except、raise、自定义异常）                   │
│  • 模块与包（import、__init__、__name__）                     │
│  • 迭代器与生成器（iter、next、yield）                         │
│  • 装饰器（闭包、@语法）                                       │
├─────────────────────────────────────────────────────────────────┤
│  第三层：系统设计（面向对象）                                   │
│  • 类与对象（class、__init__、self）                          │
│  • 类的结构（类/实例属性、三种方法）                            │
│  • 继承与多态（重写、super）                                   │
│  • 访问控制约定（public/_/__）                                 │
├─────────────────────────────────────────────────────────────────┤
│  第二层：核心编程能力（数据 + 函数）                            │
│  • 控制流（if、for、while、break、continue）                   │
│  • 数据结构（列表、元组、字典、集合、序列操作）                 │
│  • 函数（定义、参数、返回值、lambda、作用域）                  │
├─────────────────────────────────────────────────────────────────┤
│  第一层：语法基础（形成肌肉记忆）                               │
│  • 变量与类型（int、float、bool、str、动态类型）               │
│  • 基本输入输出（print、input）                                │
│  • 运算符（算术、比较、逻辑、赋值、成员运算符 in/not in）         │
│  • 字符串（索引、切片、常用方法、格式化）                       │
│  • 缩进规则与程序结构                                          │
└─────────────────────────────────────────────────────────────────┘
```

主要聚焦于前四层的知识。





