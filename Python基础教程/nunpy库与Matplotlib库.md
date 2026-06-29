# nunpy库与Matplotlib库

## nunpy库





## Matplotlib库

Matplotlib 是 Python 中最常用的**数据可视化库**，可以生成高质量的图表。它的设计理念是：**一切皆对象**——图表的每个元素（图形、坐标轴、刻度、标签）都是可以独立控制和修改的对象。

### 历史与由来

MATLAB 的绘图系统非常成熟和易用。为了在 Python 里复刻 MATLAB 的绘图体验，建立了Matplotlib库。

Matplotlib 名字的来源：Mat（MATLAB）+ plot（绘图）+ lib（library）。

### 安装与导入

**两种安装方式**

```bash
# Anaconda 安装
conda install matplotlib

# 如果没有Anaconda，则 pip 安装
pip install matplotlib
```

**导入**

```python
# 标准导入方式
import matplotlib.pyplot as plt
import numpy as np

# 在 Jupyter 中内嵌显示（如果不用Jupter则无需关心）
%matplotlib inline
```

### 核心概念：Figure 和 Axes

| 概念       | 中文        | 说明                                       |
| :--------- | :---------- | :----------------------------------------- |
| **Figure** | 图形/画布   | 整个图表窗口，可以包含多个子图             |
| **Axes**   | 坐标轴/子图 | 一个独立的绘图区域，包含数据、刻度、标签等 |
| **Axis**   | 轴          | x轴、y轴，负责刻度线和刻度标签             |
| **Artist** | 图形元素    | 图表中所有可见的元素（线条、文字、图例等） |

**关系图**：

```reStructuredText
Figure (画布)
├── Axes (子图 1)
│   ├── Axis (x轴)
│   ├── Axis (y轴)
│   ├── Line (数据线)
│   ├── Title (标题)
│   └── Legend (图例)
└── Axes (子图 2)
    └── ...
```

**两种绘图方式**：

| 方式             | 说明                                                | 适用场景             | 来源                |
| :--------------- | :-------------------------------------------------- | :------------------- | ------------------- |
| **pyplot 风格**  | `plt.plot()` 直接绘图，自动创建 Figure 和 Axes      | 快速绘图、探索性分析 | MATLAB 用户迁移而来 |
| **面向对象风格** | 显式创建 `fig, ax = plt.subplots()`，通过 `ax` 操作 | 精细控制、复杂图表   | Python 原生设计     |

### 快速上手

例子：

```python
import matplotlib.pyplot as plt
import numpy as np

# 准备数据
x = np.linspace(0, 10, 100) # [0, 10] 区间中均匀取100个点
y = np.sin(x)

# 创建图表（类似与matlab）
plt.figure(figsize=(10, 6))   # 创建画布，设置宽高（英寸）
plt.plot(x, y, label='sin(x)', color='blue', linewidth=2)  # 绘制折线图
plt.xlabel('x')               # 设置轴标签
plt.ylabel('y')   
plt.title('正弦函数')          # 设置标题
plt.legend()                  # 显示图例
plt.grid(True)                # 显示网格
plt.show()                    # 显示图表
```

### 面向对象方式 vs pyplot 方式

**pyplot 方式（快速绘图）**：

```python
plt.plot(x, y)
plt.xlabel('x')
plt.title('标题')
plt.show()
```

**面向对象方式（精细控制）**：

```python
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y, label='sin(x)')
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_title('标题')
ax.legend()
ax.grid(True)
plt.show()
```

 **对比**：

| 对比     | pyplot 方式  | 面向对象方式     |
| :------- | :----------- | :--------------- |
| 代码量   | 少           | 略多             |
| 控制粒度 | 粗           | 精细             |
| 适合场景 | 简单快速绘图 | 复杂图表、多子图 |
| 推荐度   | ⭐⭐⭐ 入门     | ⭐⭐⭐⭐⭐ 进阶       |

> **建议**：先学会 pyplot，再过渡到面向对象方式。掌握面向对象方式是真正理解 Matplotlib 的标志。

### 常见图表类型

**1. 折线图（`plot`）**

**2. 散点图（`scatter`）**

**3. 柱状图（`bar`）**

**4. 直方图（`hist`）**

**5. 饼图（`pie`）**

需要时候再具体查找。

### 子图（`subplots`）

以 $2\times 2$ 子图布局为例：

```python
# 创建 2行2列 的子图
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# axes 是一个二维数组，按 [行][列] 索引
axes[0, 0].plot(x, np.sin(x))
axes[0, 0].set_title('sin(x)')

axes[0, 1].plot(x, np.cos(x))
axes[0, 1].set_title('cos(x)')

axes[1, 0].plot(x, np.tan(x))
axes[1, 0].set_title('tan(x)')
axes[1, 0].set_ylim(-5, 5)

axes[1, 1].plot(x, np.exp(x))
axes[1, 1].set_title('exp(x)')

plt.tight_layout()  # 自动调整间距（避免子图重叠）
plt.show()
```

### 保存图表

```python
plt.savefig('output.png', dpi=300)
```

**注意**：保存必须在 `plt.show()` 之前，否则会保存空白图片

**`plt.savefig` 支持的参数：**

| 参数                  | 说明                     |
| :-------------------- | :----------------------- |
| `'output.png'`        | 文件名（必填）           |
| `dpi`                 | 分辨率（可选，默认 100） |
| `bbox_inches='tight'` | 去除多余空白（可选）     |
| `transparent=True`    | 透明背景（可选）         |
| `facecolor='white'`   | 背景色（可选）           |
| `edgecolor='none'`    | 边框颜色（可选）         |

**文件名支持多种格式**：

```python
plt.savefig('output.png')   # PNG
plt.savefig('output.pdf')   # PDF（矢量）
plt.savefig('output.svg')   # SVG（矢量）
plt.savefig('output.jpg')   # JPG（不支持透明）
```

