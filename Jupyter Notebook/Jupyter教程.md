# **Jupyter Notebook**

## 简介

Jupyter Notebook 是一个开源的交互式计算环境，允许用户创建和共享包含实时代码、方程、可视化以及叙述性文本的文档。它最初是 IPython 项目的一部分，后来发展成为支持多种编程语言的工具。



## **主要特点**

1. **交互式计算环境**：可以逐行执行代码并立即查看结果
2. **多语言支持**：支持 Python、R、Julia、Scala 等 40 多种编程语言
3. **富文本支持**：结合 Markdown 和 HTML 编写文档说明
4. **数据可视化**：直接内联显示图表和图形
5. **易于分享**：可导出为 HTML、PDF、Markdown 等多种格式
6. **云端支持**：可在 Google Colab、Azure Notebooks 等平台使用



## 基本组件

1. **单元格**
   - 代码单元格：包含可执行代码。
   - Markdown单元格：包含格式化文本。
2. **内核**：执行代码的引擎，与前端界面分离。



## 安装与启动

==JupyterLab是jupyter notebook的下一代，推荐直接安装JupyterLab会更高效。两者是兼容的，下载哪个都可以。==



### 使用Anaconda安装

打开Anaconda Prompt，输入命令安装：`conda install jupyter notebook` 。

通常安装Anaconda的时候已经自动为你安装了Jupter Notebook。



### 使用pip命令安装

在终端中，使用命令安装`pip install jupyter notebook` 。

（终端打开：`window+R`，输入`cmd`，并回车）





## 基本操作流程

### 启动

- 在终端或Anaconda Prompt输入命令：`jupyter notebook` 。

- 在开始菜单中，找到`jupyter notebook` 。

启动之后，浏览器将会进入到Notebook的主页面，里面会显示当前所在的目录和目录下的文件。



### 创建新笔记本

1. 点击右上角 `New` → 选择语言 (如 `Python3(ipykernel)`)。
2. 也可以在菜单栏中选择 `File-->New——>Notebook`  → 选择内核（如`Python3(ipykernel)`）。
3. 在主页面所显示的目录下，自动创建 Untitled.ipynb 文件，并在浏览器中打开。
4. 界面组成：
   - 菜单栏
   - 工具栏
   - 单元格编辑区



### 单元格基础操作

- **添加单元格(cell)**：点击工具栏 "+" 按钮或按 `B`(下方)/`A`(上方)
- **删除单元格**：选中后快速双击 `D`
- **移动单元格**：选中后使用上下箭头按钮或拖拽
- **合并/拆分单元格**：菜单栏 Edit 中操作



### 代码与文档编写

**切换单元格类型**：选中单元格，按 `Y` 将当前单元块切换为代码单元格，按 `M` 将当前单元块切换为Markdown单元格。

**逐块执行代码**：使用 `Ctrl+Enter` 或 `shift + Enter` 运行当前单元块，并可随时修改并重新执行



## 注意

1. 虽然在网页上写代码，但是程序运行的内核是`Jupyter Notebook`，因此使用中不要关闭`Jupyter Notebook`。

2. 导入的库必须是anaconda在当前的环境（默认base环境）中有这个库才可以。

3. Anaconda自带的`Jupyter Notebook`是在base环境中的，如果你想在新建的环境中使用，那么需用conda进行包管理，下载到当前环境，`conda install jupyter notebook`。

4. 直接打开的`Jupyter Notebook`是在base环境中的。如果想要在新环境中打开，需要先在`anaconda`中激活新的环境，然后在新的环境的命令行中，输入 `Jupyter Notebook` 启动，在新环境中打开，可以调用当前新环境中的库。



## 实用功能

### 常用快捷键

- **运行当前单元格并跳转到下一单元格**：`Shift + Enter`
- **运行当前单元格但不跳转下一单元格**：`Ctrl + Enter` 
- **插入单元格**：`A` (上方) / `B` (下方)
- **删除单元格**：`D` + `D` （双击`D`）
- **切换单元格类型**：`Y` (代码) / `M` (Markdown)
- **保存笔记本**：Ctrl + S



### 效率技巧

1. **Tab补全**：输入部分命令后按Tab键
2. **查看帮助**：在函数/方法后加`?`，如 `pd.read_csv?`
3. **批量操作**：Shift+选择多个单元格



###  魔术命令（仅针对Jupyter）

- `%run script.py` - 运行外部Python文件
- `%load script.py` - 加载外部文件内容到当前单元格
- `%matplotlib inline` - 内嵌显示matplotlib图形
- `%%writefile file.txt` - 将单元格内容写入文件



## 其他

### 块间数据与执行问题

1. 所有成功执行的单元格产生的变量都会保存在内核内存中；中间单元格出错**不会清除**之前单元格创建的变量。

   示例：

   ```python
   # 单元格1
   a = 10  # 成功执行
   
   # 单元格2
   b = 20  # 成功执行
   
   # 单元格3
   c = d * 2  # 出错(d未定义)，但a和b依然存在
   ```

2. 出错单元格中**出错语句之前**的代码如果已执行，其产生的变量会被保存；出错语句及之后的代码不会执行。

   示例：

   ```python
   # 单元格内容：
   x = 5      # 会执行并保存
   y = 1/0     # 这里出错
   z = 10      # 不会执行
   ```

3. 可以通过点击状态栏中的 `Restart the kernel` ，以重置内核的方式清空所有数据。
4. Jupyter Notebook 的执行顺序完全由手动执行单元格的顺序决定，与单元格的物理排列顺序无关。



## JupyterLab

Jupyter Notebook 的下一代交互界面 JupyterLab 提供了更灵活的工作环境，支持多标签页、文件浏览器、终端等功能，逐渐成为推荐使用的版本。

安装命令：`pip install jupyterlab` 或 `conda install jupyterlab` 。

启动命令：`jupyter lab` 。

 **对比总结**：

| 特性               | Jupyter Notebook   | JupyterLab           |
| :----------------- | :----------------- | :------------------- |
| **核心功能**       | 基础 Notebook 编辑 | Notebook + 增强环境  |
| **多文件操作**     | 不支持             | 支持（分屏、多标签） |
| **扩展性**         | 有限               | 强大（支持插件）     |
| **文件管理**       | 简单文件列表       | 完整资源管理器       |
| **终端/文本编辑**  | 不支持，需外部工具 | 内置支持             |
| **启动命令**       | `jupyter notebook` | `jupyter lab`        |
| **Notebook兼容性** | 原生               | 完全兼容             |





