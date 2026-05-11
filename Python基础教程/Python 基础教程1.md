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

   从[Python官网](https://www.python.org/)中找到相应系统的下载地址。

2. Python环境安装

   安装时相应选项的解释可以参考[python安装教程-CSDN博客](https://blog.csdn.net/thefg/article/details/128601410)，一般自定义选择安装路径，其他按默认的来就好。

3. VS code下载

   下载地址[Visual Studio Code 官网](https://code.visualstudio.com/)

4. VS code安装

   直接安装，注意自定义安装路径即可。参考[VScode安装、配置和使用-CSDN博客](https://blog.csdn.net/toby54king/article/details/105453603)

5. 在VScode中安装python相关插件

   在VScode的拓展商店中搜索相关插件名字（如Python）并下载。常用的插件有：（1）python (必装，可以智能补全等)

6. 下载一些需要的python库

   python库的安装和卸载需要pip。在安装python过程时，如果勾选了`pip`则不需要额外下载。

   在终端输入`pip –-version`可以查看pip的版本信息。

   `pip list`查看已安装的python库。

   `python -m pip install --upgrade pip`更新pip版本到最新。

   `pip install <库的名称>`安装python库，如`pip install numpy`。

   `pip uninstall <库的名称>`卸载python库。

### Vscode中选择解释器

当你创建新的虚拟环境，想使用新的环境下的python解释器，可以通过以下步骤：

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



## 知识结构

```reStructuredText
┌─────────────────────────────────────────────────────────────────┐
│  第五层：专用领域库（按需学习）                                   │
│  • 数据分析：NumPy、Pandas、Matplotlib                          │
│  • 网络爬虫：requests、BeautifulSoup、Scrapy                    │
│  • Web开发：Django、Flask                                       │
│  • 人工智能：PyTorch、TensorFlow                                │
│  • 数据库：SQLAlchemy、sqlite3                                  │
├─────────────────────────────────────────────────────────────────┤
│  第四层：标准库（工具集，拿来就用）                               │
│  • 常用模块：math、random、datetime、json、re（正则）            │
│  • 系统交互：os、sys、subprocess                                │
│  • 数据存储：pickle、sqlite3                                    │
│  • 网络请求：urllib、socket                                     │
├─────────────────────────────────────────────────────────────────┤
│  第三层：高级特性（写出更 Pythonic 的代码）                        │
│  • 列表推导式、生成器表达式                                      │
│  • 装饰器、上下文管理器                                          │
│  • 迭代器、生成器（yield）                                       │
│  • 异常处理                                                      │
│  • 模块与包管理                                                  │
├─────────────────────────────────────────────────────────────────┤
│  第二层：核心编程能力（一定熟练掌握）                             │
│  • 函数（定义、参数、返回值、lambda）                            │
│  • 数据结构（列表、元组、字典、集合）                             │
│  • 控制流（if、for、while、break、continue）                    │
│  • 文件操作（open、read、write）                                │
│  • 面向对象（类、对象、继承、`__init__`、self）                  │
├─────────────────────────────────────────────────────────────────┤
│  第一层：语法基础（形成肌肉记忆）                                 │
│  • 变量与类型（int、float、bool、str、动态类型）                 │
│  • 基本输入输出（print、input）                                 │
│  • 运算符（算术、比较、逻辑、赋值、成员运算符 in/not in）         │
│  • 字符串操作（索引、切片、常用方法）                             │
│  • 缩进规则（代码块用缩进，不要用 `{}`）                         │
└─────────────────────────────────────────────────────────────────┘
```

主要聚焦于第一、第二层的知识。