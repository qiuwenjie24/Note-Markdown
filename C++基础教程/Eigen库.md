# C++ Eigen库

默认已经安装有C++以及VScode编辑器。

## Eigen库

### 简介

Eigen是一个用于线性代数的 C++ 模板库（开源），包含矩阵、向量、数值求解器和相关算法等内容，可以高效方便地进行矩阵计算。

与Matlab类似，但是C++更底层，效率更高，且可以直接控制硬件。

### 头文件

需要用到的头文件就三个。

```c++
// 推荐：一个头文件覆盖 90% 需求，包含QR分解等
#include <Eigen/Dense>

// 如果需要 3D 几何，再加上这个
#include <Eigen/Geometry>

// 如果需要稀疏矩阵，再加上这个
#include <Eigen/Sparse>
```

### 命名空间

Eigen库的命名空间是 `Eigen` 。

不使用命名空间：

```c++
#include <Eigen/Dense>

int main() {
    Eigen::Matrix3d m1;          // 使用Eigen库内的工具时需要加上 Eigen::
    return 0;
}
```

使用命名空间：

```c++
#include <Eigen/Dense>
using namespace Eigen;
int main() {
    Matrix3d m1;          // 无需加上 Eigen::
    return 0;
}
```



## 安装与环境配置

### 1. 下载 Eigen

从 Eigen 官网下载最新版本的压缩包：https://eigen.tuxfamily.org/

下载后解压到任意目录，比如 `D:\Cpp\Cpp_library` ，得到文件：

```reStructuredText
D:/Cpp/Cpp_library/eigen-5.0.0/
    					└── Eigen/
        						├── Dense
       					 		├── Core
        						└── ... (其他头文件)
```

注意，解压的时候可能会多创建一层`eigen-5.0.0`文件夹，那么此时的文件路径应为`D:/Cpp/Cpp_library/eigen-5.0.0/eigen-5.0.0` 。

### 2. 在 VS Code 中配置 Eigen

你需要告诉编译器去哪里找 Eigen 的头文件。Eigen 的核心头文件在解压后的 `Eigen` 文件夹里。

**2.1. 先找到 `tasks.json` 文件**

在 VS Code 中打开你的 C++ 项目文件夹，然后按以下步骤操作：

1. 按 `Ctrl + Shift + P`（打开命令面板）
2. 输入 `tasks`，选择 **Tasks: Configure Default Build Task**
3. 选择 **g++.exe build active file**
4. VS Code 会自动创建 `.vscode/tasks.json` 文件并打开。

**2.2. 修改 tasks.json**

打开后，找到文件内容：

```json
"args": [
    "-fdiagnostics-color=always",
    "-g",
    "${file}",
    "-o",
    "${fileDirname}\\${fileBasenameNoExtension}.exe"
],
```

修改成：

```json
"args": [
    "-fdiagnostics-color=always",
    "-g",
    "${file}",
    "-I", "D:/Cpp/Cpp_library/eigen-5.0.0",   // ← 添加这行（注意：路径改成你的）
    "-o",
    "${fileDirname}\\${fileBasenameNoExtension}.exe"
],
```

- 路径要指向你解压 Eigen 的文件夹 `eigen-5.0.0`（不是里面的 `Eigen` 子文件夹）

#### 2.4. 配置 `c_cpp_properties.json`

完成上面步骤之后，可以正常使用 Eigen 库，能编译，但是会有一个红色波浪线并提示错误。不影响使用，但是如果想消除它，需要进行以下步骤：

按 `Ctrl + Shift + P`，输入 `C/C++: Edit Configurations (JSON)`，打开后修改：

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/Cpp/Cpp_library/eigen-5.0.0"   // ← 添加这一行
            ],
            "compilerPath": "D:/Cpp/mingw64/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```

配完之后红色波浪线应该消失。

#### 2.5. 验证

写一个测试程序 `test.cpp`：

```c++
#include <iostream>
#include <Eigen/Dense>
using namespace Eigen;
using namespace std;

int main() {
    Matrix2d m;
    m << 1, 2, 3, 4;
    cout << m << endl;
    return 0;
}
```

按 `Ctrl + Shift + B`（F5） 编译，如果没有报错 `Eigen/Dense: No such file`，就说明配置成功了。



## 核心数据结构

Eigen 库的核心数据结构是**矩阵类Matrix**和**数组类Array**。

Matrix 主要用于线性代数运算（乘法是矩阵乘）

Array 用于逐元素操作（乘法是对应元素相乘）

两者通过 `.array()` 和 `.matrix()` 互相转换

### 矩阵Matrix

矩阵类 Matrix 是一个模板类，用于创建矩阵，计算是按矩阵整体进行操作。

**基本语法：**

- 固定行列数的矩阵：`Matrix<数据类型, 行数, 列数>` 。
- 动态行列的矩阵：`Matrix<数据类型, Dynamic, Dynamic>` ，用于运行时才知道具体的矩阵大小的情况。
- 动态行列的矩阵的简洁版：`MatrixX+数据类型首字母` ，如 `MatrixXd` 是双精度的动态矩阵。
- 半固定半动态矩阵：`Matrix<数据类型, 行数, Dynamic>` 或 `Matrix<数据类型, Dynamic, 列数>` 
- 方阵的简洁版：`Matrix+行数+数据类型首字母` ，如 `Matrix4d` 表示 $4\times 4$ 的双精度矩阵，等价于 `Matrix<double, 4, 4>` 。这种简写方式仅适用于大小不超过 $4\times 4$ 的矩阵。



**创建 Matrix 的四种方式**：

```c++
// 方式1：固定大小，未初始化（值不确定）
Matrix3d m1;

// 方式2：固定大小，直接初始化（静态方法）
Matrix3d m2 = Matrix3d::Zero();     // 全零
Matrix3d m3 = Matrix3d::Identity(); // 单位矩阵
Matrix3d m4 = Matrix3d::Random();   // 随机数
Matrix3d m5 = Matrix3d::Ones();     // 全一
Matrix3d m6 = Matrix3d::Constant(2.5); // 全为 2.5c

// 方式3：动态大小，指定尺寸（不管是否指定，实际大小都是根据运行的情况而发生改变）
MatrixXd m7;      // // 0x0 动态矩阵，用于还不知道多大，运行才确定的矩阵
MatrixXd m8(rows, cols); // rows、cols是运行时才决定的变量，创建rows行cols列矩阵，

// 方式4：<<初始化（最直观）
// 按行优先的顺序填充矩阵，<< 表示"开始填充"，换行只是为了让代码好看，Eigen 不关心换行
Matrix3d m9;
m7 << 1, 2, 3,   
      4, 5, 6,
      7, 8, 9;   
```

**访问和修改元素**

使用圆括号 `()` 访问矩阵元素：

```c++
Matrix3d m;           // 声明矩阵
m(0, 0) = 100;        // 矩阵的第0行第0列赋值100
double x = m(1, 2);   // 读取矩阵第1行第2列，并赋值给变量x
```



### 向量（Matrix 特例）

**列向量Vector**

列向量是行数为1的矩阵。

基本的语法：

- 固定维数的列向量：`Matrix<数据类型, 行数, 1>` 。
- 简洁版：`Vector+向量维数+数据类型首字母` ，如 `Vector2d` 表示双精度的2维列向量，但是这种方式最大只能创建4维向量。
- 动态列向量：`VectorXd` ，用于运行时才知道具体的矩阵大小的情况。

**行向量RowVector**

行向量是列数为1的矩阵。

基本的语法：

- 固定维数的行向量：`Matrix<数据类型, 1, 列数>` 。
- 简洁版：`RowVector+向量维数+数据类型首字母` ，如 `RowVector2d` 表示双精度的2维行向量，但是这种方式最大只能创建4维向量。
- 动态列向量：`RowVectorXd` ，用于运行时才知道具体的矩阵大小的情况。

**向量的声明**

声明以及初始化的方式与矩阵是相同的。

```c++
Vector3d v1;          // 3维双精度列向量（其实是 3x1 矩阵）
VectorXd v2;         // 0维动态列向量
VectorXd v3(5);       // 5维动态列向量
RowVector3d rv1;      // 3维行向量（1x3 矩阵）
```
**向量的初始化**

以 `Vector3d` 为例：

```c++
// 方式1：直接传三个值（最直观）
Eigen::Vector3d a(1, 2, 3);   // 创建向量并初始化为 (1, 2, 3)

// 方式2：逗号初始化
Eigen::Vector3d b;
b << 1, 2, 3;

// 方式3：先声明，后逐个赋值
Eigen::Vector3d c;
c(0) = 1;
c(1) = 2;
c(2) = 3;

// 方式4：静态方法
Eigen::Vector3d d = Eigen::Vector3d::Zero();   // (0,0,0)
Eigen::Vector3d e = Eigen::Vector3d::Ones();   // (1,1,1)
Eigen::Vector3d f = Eigen::Vector3d::UnitX();  // (1,0,0) ← 专门创建 x 轴单位向量
Eigen::Vector3d g = Eigen::Vector3d::UnitY();  // (0,1,0)
Eigen::Vector3d h = Eigen::Vector3d::UnitZ();  // (0,0,1)
```

**访问和修改元素**

Matrix 只能圆括号 `()` 访问元素，而矢量使用圆括号 `()` 或方括号 `[]` 都可以。

```c++
// 向量有额外语法糖，以下3种访问元素的方式是等价的
Vector3d v(1, 2, 3);  // 声明
v(0) = 10;    // 标准写法，访问第0个元素
v[0] = 10;    // 也可以用方括号
v.x() = 10;   // .x() 表示取向量的第1个元素, .y() 表示取第2个元素, .z()取第3个元素 , w()取第4个元素
```



### 数组Array

数组Array也是一个模板类。

Matrix 的 `*` 是矩阵乘法，但有时候我们只是想做对应元素相乘（逐元素乘)，为此引入了数组Array。

**基本语法**

Array 和 Matrix 是完全对应的，模板语法是类似。故这里只给出它们之间的对应：

| 用途         | Matrix 写法            | Array 写法                    |
| :----------- | :--------------------- | :---------------------------- |
| 模板语法     | `Matrix<double, 3, 3>` | `Array<double, 3, 3>`         |
| 3x3 typedef  | `Matrix3d`             | `Array33d`（注意是33，不是3） |
| 动态 typedef | `MatrixXd`             | `ArrayXXd` （注意是XX）       |
| 一维动态     | `VectorXd`             | `ArrayXd`（注意是X）          |
| 一维列向量   | `Vector3d`             | `Array3d` （注意这里是3）     |

注意：

-  Array 本身就不区分行/列向量，可以认为是列向量。
- `Array33d`，`Array3d` 同样地不能超过4维。

### Matrix 与 Array 的转换

两者通过 `.array()` 和 `.matrix()` 互相转换

```c++
Matrix2d m;
m << 1, 2, 3, 4;

Array22d a;
a << 5, 6, 7, 8;

// Matrix → Array
Array22d a_from_m = m.array();

// Array → Matrix
Matrix2d m_from_a = a.matrix();
```



## 基础运算：+ - * / 转置 点积 叉积

**运算总览**

| 运算 | 符号/方法      | 适用对象       | 说明         |
| :--- | :------------- | :------------- | :----------- |
| 加法 | `+`            | Matrix / Array | 对应元素相加 |
| 减法 | `-`            | Matrix / Array | 对应元素相减 |
| 乘法 | `*`            | Matrix         | 矩阵乘法     |
| 乘法 | `*`            | Array          | 逐元素乘法   |
| 除法 | `/`            | Array          | 逐元素除法   |
| 转置 | `.transpose()` | Matrix         | 行列互换     |
| 点积 | `.dot()`       | Vector         | 向量点积     |
| 叉积 | `.cross()`     | Vector (3D)    | 向量叉积     |

注意：

- 标量运算（如加减乘除）是矩阵/数组每个元素与标量运算。
- Matrix 没有直接的除法 `/` 。
- 原地转置不能使用 `A = A.transpose();` ，正确的做法是 `A = A.transpose();` 。
- 点积的用法：`a.dot(b)` 表示向量a、b做内积。
- 叉积的用法：`a.cross(b)` 表示向量a、b做外积，只用于 3D 向量。



## 块操作

本节所讲的操作对矩阵和数组都是通用，两者的区别只是得到的数据类型不同。故介绍时只以其中一个为例。

框架：

```reStructuredText
块操作
├── 前置：属性查询（rows, cols, size）
├── 核心：block, row, col, head, tail
└── 实战：修改子矩阵
```

### 属性查询

在操作块之前，先要知道矩阵/数组的属性信息。

**基本属性**：

- 行数：`.rows()` 
- 列数：`.cols()` 
- 总元素个数：`.size()` 

例子：

```c++
#include <iostream>
#include <Eigen/Dense>

int main() {
    Eigen::MatrixXd m(3, 4);
    
    // 行数
    std::cout << "行数: " << m.rows() << std::endl;     // 输出: 3
    
    // 列数
    std::cout << "列数: " << m.cols() << std::endl;     // 输出: 4
    
    // 总元素个数
    std::cout << "总元素: " << m.size() << std::endl;   // 输出: 12
    
    // 判断是否为空
    std::cout << "是否为空: " << m.size() == 0 << std::endl;
    
    return 0;
}
```



### 块与块操作

块（Block） 是矩阵/数组中的一个矩形子区域。块操作就是取块矩阵，并对块矩阵进行操作。

**重要**： 块操作返回的是引用，也就是说如果修改子矩阵，那么原矩阵也随着被修改！

 图示：

```reStructuredText
完整的 4x4 矩阵：
┌─────────────────┐
│ 1  2  3  4      │
│ 5  6  7  8      │
│ 9 10 11 12      │
│13 14 15 16      │
└─────────────────┘

块操作示例：
- 第 0 行    → [1 2 3 4]
- 第 2 列    → [3 7 11 15]ᵀ
- 2x2 子矩阵 → ┌────┐
               │6  7│
               │10 11│
               └────┘
```

### 块操作方法

**取任意矩形块**

基本语法：`.block(起始行, 起始列, 行数, 列数)` 

作用：获取矩阵/数组中的任意一个矩形子区域

例子：

```c++
Eigen::Matrix4d m;
m << 1, 2, 3, 4,
     5, 6, 7, 8,
     9,10,11,12,
     13,14,15,16;
Eigen::Matrix2d block = m.block(1, 1, 2, 2);
// 6  7
// 10 11
```

**取一行**

基本语法：`.row(行序号)` 

作用：获取矩阵/数组中的任意一个行

```c++
Eigen::Matrix4d m;
m << 1, 2, 3, 4,
     5, 6, 7, 8,
     9,10,11,12,
    13,14,15,16;
// 取第 1 行（从0开始计数）
Eigen::RowVector4d row1 = m.row(1); // 输出: 5 6 7 8
```

**取一列**

基本语法：`.col(列序号)` 

作用：获取矩阵/数组中的任意一个列

```c++
Eigen::Matrix4d m;
m << 1, 2, 3, 4,
     5, 6, 7, 8,
     9,10,11,12,
    13,14,15,16;
// 取第 2 列（从0开始计数）
Eigen::Vector4d col2 = m.col(2);
// 3
// 7
// 11
// 15
```

### 向量专用块操作

向量只有一维，有一些便捷的块操作方法。

**取前 n 个元素**

基本语法：`.head(n)` 

作用：获取向量的前 n 个元素。

**取后 n 个元素**

基本语法：`.tail(n)` 

作用：获取向量的后 n 个元素。

**取中间一段**

基本语法：`.segment(起始位置, 长度)` 

作用：获取向量的后 n 个元素。

**向量块操作图示**：

```text
向量 v = [1, 2, 3, 4, 5, 6]

.head(3)  → [1, 2, 3]
.tail(2)  → [5, 6]
.segment(2, 3) → [3, 4, 5]
           ↑    ↑
        起始   长度
```

**例子**：

```c++
Eigen::VectorXd v(6);
v << 1, 2, 3, 4, 5, 6;

// .head(n)
Eigen::VectorXd head = v.head(3);  // 取前 3 个元素
std::cout << "head(3): " << head.transpose() << std::endl;  // 输出: 1 2 3

// .tail(n)
Eigen::VectorXd tail = v.tail(2);  // 取后 2 个元素
std::cout << "tail(2): " << tail.transpose() << std::endl;  // 输出: 5 6

// .segment(n,m)
Eigen::VectorXd seg = v.segment(2, 3);  // 从索引 2 开始，取 3 个元素
std::cout << "segment(2,3): " << seg.transpose() << std::endl;  // 输出: 3 4 5
```

### 修改子矩阵

方式有：

- 修改单个元素
- 修改整行
- 修改子矩阵块

例子：

```c++
Eigen::Matrix3d m;
m << 1, 2, 3,
     4, 5, 6,
     7, 8, 9;

// 修改单个元素：
m(1, 1) = 100;  // 修改第 1 行第 1 列（中心元素）

// 修改整行
m.row(1).setZero();  // 将第 1 行全部赋值为 0

// 修改整列
m.col(0).setConstant(10);  // 将第 0 列全部赋值为 10

// 修改子矩阵块
m.block(0, 0, 2, 2) = Eigen::Matrix2d::Identity();  // 将左上角 2x2 子矩阵设为单位矩阵

// 修改子矩阵块
m.block(0, 0, 2, 2) = Eigen::Matrix2d::Identity();  // 将左上角 2x2 子矩阵设为单位矩阵
Eigen::Matrix2d small;
small << 99, 98,
         97, 96;
m.block(2, 2, 2, 2) = small;   // 用另一个矩阵赋值的方式修改
```

注意：块操作返回的是引用，如果需要独立副本，可以通过赋值的方式创建一个副本。

```c++
Eigen::MatrixXd m(3, 3);
m.setRandom();
// 如果需要独立副本
Eigen::MatrixXd copy = m.block(0, 0, 2, 2);
copy.setZero();  // 只修改 copy，不影响 m
```



## 元素级操作



框架：

```reStructuredText
元素级与归约操作
├── 逐元素：abs, sqrt, min, max, square
├── 归约：sum, prod, mean, norm
└── 布尔：all, any, count
```

### 操作分类

| 操作类别                                        | Matrix | Array | 说明       |
| :---------------------------------------------- | :----- | :---- | :--------- |
| 属性查询（`.rows()`, `.cols()`, `.size()`）     | ✅      | ✅     | 完全相同   |
| 逐元素数学函数（`.sqrt()`, `.exp()`, `.abs()`） | ❌      | ✅     | Array 专属 |
| 逐元素比较（`>`, `<`, `==`）                    | ❌      | ✅     | Array 专属 |
| 逐元素限幅（`.min()`, `.max()`）                | ❌      | ✅     | Array 专属 |
| 归约（`.sum()`, `.prod()`, `.mean()`）          | ❌      | ✅     | Array 专属 |
| 布尔归约（`.all()`, `.any()`, `.count()`）      | ❌      | ✅     | Array 专属 |

**核心结论：** 元素级操作是 Array 的专属领域，Matrix 需要先转换为 Array。

**注意**：Matrix 本身不支持元素级操作，但可以通过 `.array()` 转换，进行操作之后，再通过 `.matrix()` 转回 Matrix 。

### 属性查询

属性查询是 Matrix + Array 共有。在上一节介绍过，故这里略过。

### 逐元素操作

逐元素操作是 Array 特有的。

 **数学函数**

- `.sqrt()` ：对数组中的所有元素取平方根。
- `.square()` ：对数组中的所有元素取平方。
- `.abs()` ：对数组中的所有元素取绝对值。
- `.exp()` ：对数组中的所有元素取指数。
- `.log()` ：对数组中的所有元素取对数。
- `.sin()` ：对数组中的所有元素取正弦。
- `.cos()` ：对数组中的所有元素取余弦。

例子：

```c++
#include <iostream>
#include <Eigen/Dense>

int main() {
    Eigen::Array3d a(1, 4, 9);
    
    std::cout << "原数组: " << a.transpose() << std::endl;
    // 输出: 1 4 9
    
    // 基本数学函数
    std::cout << "sqrt:  " << a.sqrt().transpose() << std::endl;   // 1 2 3
    std::cout << "square:" << a.square().transpose() << std::endl; // 1 16 81
    std::cout << "abs:   " << a.abs().transpose() << std::endl;    // 1 4 9
    
    // 指数对数
    std::cout << "exp:   " << a.exp().transpose() << std::endl;    // 2.718 54.598 8103.08
    std::cout << "log:   " << a.log().transpose() << std::endl;    // 0 1.386 2.197
    
    // 三角函数
    Eigen::Array3d b(0, M_PI/2, M_PI);
    std::cout << "sin:   " << b.sin().transpose() << std::endl;    // 0 1 0
    std::cout << "cos:   " << b.cos().transpose() << std::endl;    // 1 0 -1
    
    return 0;
}
```

**逐元素比较**

对两个数组进行比较运算操作（`>, <, ==, <=` 等）时，是逐元素比较，最后返回一个数组。

数组的元素与每个元素中比较的结果有关，如果比较结果是 `false` 则该元素返回的值是1，反之是0。

例子：

```c++
Eigen::Array3d a(1, 5, 3);
Eigen::Array3d b(2, 4, 3);

// 比较运算返回布尔数组（0=false, 1=true）
std::cout << "a > b:  " << (a > b).transpose() << std::endl;   // 0 1 0
std::cout << "a >= b: " << (a >= b).transpose() << std::endl;  // 0 1 1
std::cout << "a < b:  " << (a < b).transpose() << std::endl;   // 1 0 0
std::cout << "a == b: " << (a == b).transpose() << std::endl;  // 0 0 1

// 与标量比较
std::cout << "a > 3:  " << (a > 3).transpose() << std::endl;   // 0 1 0
```

**逐元素限幅**

两个数组进行比较，并返回两个数组中对应元素的最大/最小值。

语法：

- `a.min(b)` ：返回数组 a 和 b 中的对应位置的最小元素组成的数组，b 可以为标量。
- `a.max(b)` ：返回数组 a 和 b 中的对应位置的最大元素组成的数组，b 可以为标量。
- `a.cwiseMin(b)` ：等价 `a.min(b)` ，但是 b 不能为标量。
- `a.cwiseMax(b)` ：等价 `a.max(b)` ，但是 b 不能为标量。

例子：

```c++
Eigen::Array3d a(1, 5, 3);

// 每个元素与标量比较，取最小/最大
std::cout << "min(a, 3): " << a.min(3).transpose() << std::endl;  // 1 3 3
std::cout << "max(a, 3): " << a.max(3).transpose() << std::endl;  // 3 5 3

// 两个数组逐元素比较
Eigen::Array3d b(2, 4, 6);
std::cout << "cwiseMin: " << a.cwiseMin(b).transpose() << std::endl;  // 1 4 3
std::cout << "cwiseMax: " << a.cwiseMax(b).transpose() << std::endl;  // 2 5 6

// 实用技巧：将 a 的元素数值限制在 [0, 10] 范围内
a = a.max(0).min(10);
```

### 归约操作

**基本归约**

- `a.sum()` ：对数组 a 的所有元素求和。
- `a.prod()` ：对数组 a 的所有元素求积。
- `a.mean()` ：对数组 a 的所有元素求平均值。
- `a.minCoeff()` ：返回数组 a 中最小值。
- `a.maxCoeff()` ：返回数组 a 中最大值。
- `int min_index; a.minCoeff(&min_index);` ：可以传变量地址进函数的方式，函数会修改变量为最小值所对应的索引。

例子：

```c++
Eigen::Array3d a(1, 2, 3);
Eigen::Array22d b;
b << 1, 2,
     3, 4;

// 求和
std::cout << "a.sum(): " << a.sum() << std::endl;    // 6
std::cout << "b.sum(): " << b.sum() << std::endl;    // 10

// 求积
std::cout << "a.prod(): " << a.prod() << std::endl;  // 6
std::cout << "b.prod(): " << b.prod() << std::endl;  // 24

// 平均值
std::cout << "a.mean(): " << a.mean() << std::endl;  // 2

// 最小值/最大值
std::cout << "a.minCoeff(): " << a.minCoeff() << std::endl;        // 1
std::cout << "a.maxCoeff(): " << a.maxCoeff() << std::endl;        // 3

// 获取最小值的位置
int min_index;
double min_value = a.minCoeff(&min_index);   // 传地址，函数会修改地址对应变量的数值、
std::cout << "最小值: " << min_value << " 位置: " << min_index << std::endl;  // 1 0
```

**矩阵的归约（按行或按列）**

需要结合基本归约操作。

- 按列归约 `.colwise()` 
- 按行归约 `.rowwise()`

例子：

```c++
Eigen::ArrayXXd m(2, 3);
m << 1, 2, 3,
     4, 5, 6;

// 按列归约（每一列独立运算）
std::cout << "按列求和: " << m.colwise().sum().transpose() << std::endl;
// 输出: 5 7 9  (第0列:1+4=5, 第1列:2+5=7, 第2列:3+6=9)

// 按行归约（每一行独立运算）
std::cout << "按行求和: " << m.rowwise().sum().transpose() << std::endl;
// 输出: 6 15  (第0行:1+2+3=6, 第1行:4+5+6=15)

// 按列求最大值
std::cout << "按列最大值: " << m.colwise().maxCoeff().transpose() << std::endl;
// 输出: 4 5 6

// 按行求平均值
std::cout << "按行平均值: " << m.rowwise().mean().transpose() << std::endl;
// 输出: 2 5
```



### 布尔操作

看例子吧，不知道怎么说。

```c++
Eigen::Array3d a(1, 5, 3);
Eigen::Array3d b(2, 4, 3);

// all()：是否所有元素都满足条件
bool all_greater = (a > b).all();      // false，因为 1>2 不成立
bool all_nonzero = (a != 0).all();     // true，所有元素非零

// any()：是否有任意元素满足条件
bool any_greater = (a > b).any();      // true，因为 5>4 成立

// count()：统计满足条件的元素个数
int count_greater = (a > b).count();   // 1

std::cout << "all(a>b): " << all_greater << std::endl;   // 0
std::cout << "any(a>b): " << any_greater << std::endl;   // 1
std::cout << "count(a>b): " << count_greater << std::endl; // 1
```

### 链式操作

可以进行链式操作，如：

```c++
// 链式操作
Eigen::Matrix3d result = m.array().square().sqrt().matrix(); 
```



## 线性方程组求解

### 数学形式

线性方程组的数学形式：
$$
Ax =b
$$
其中，系数矩阵 $A$ 和右侧向量 $b$ 是已知的，需要求解未知向量 $x$ 。

### 求解方法总览

注意：不要用求逆的方式求解，即 `.inverse()`：`x = A.inverse() * b` ，这种特别慢且数值不稳定。

Eigen 求解线性方程组的模式是：`x = A.分解器().solve(b);` 。需要先对系数矩阵做分解，然后再求解。任何分解器，都是这个模式。

注意

Eigen 提供了多种分解器，按使用频率排序：

| 分解器                  | 适用场景         | 速度 | 稳定性 | 数学原理                |
| :---------------------- | :--------------- | :--- | :----- | ----------------------- |
| `ldlt()`                | 对称正定矩阵     | 最快 | 高     | $A=LDL^T$               |
| `llt()`                 | 正定矩阵         | 快   | 高     | Cholesky 分解：$A=LL^T$ |
| `colPivHouseholderQr()` | 通用（推荐新手） | 中等 | 高     |                         |
| `fullPivLu()`           | 通用方阵         | 慢   | 最高   | LU 分解：$A=LU$         |
| `jacobiSvd()`           | 最小二乘、非方阵 | 最慢 | 最高   |                         |
| `householderQr()`       | 最小二乘         | 快   | 中     |                         |

例子：

```c++
#include <iostream>
#include <Eigen/Dense>

int main() {
    Eigen::Matrix3d A;
    A << 2, 1, 1,
         1, 2, 1,
         1, 1, 2;
    
    Eigen::Vector3d b(4, 4, 4);
    
    // 以下所有写法都正确，只是内部算法不同
    Eigen::Vector3d x1 = A.ldlt().solve(b);              // 对称正定专用
    Eigen::Vector3d x2 = A.llt().solve(b);               // 正定专用
    Eigen::Vector3d x3 = A.colPivHouseholderQr().solve(b); // 通用方阵
    Eigen::Vector3d x4 = A.fullPivLu().solve(b);         // 通用方阵（更稳定）
    Eigen::Vector3d x5 = A.householderQr().solve(b);     // 也可用于方阵
    
    std::cout << "ldlt: " << x1.transpose() << std::endl;
    std::cout << "llt:  " << x2.transpose() << std::endl;
    std::cout << "QR:   " << x3.transpose() << std::endl;
    std::cout << "LU:   " << x4.transpose() << std::endl;
    
    // 所有结果相同：1 1 1
    return 0;
}
```

**新手建议：**

- 方阵 → `colPivHouseholderQr()`
- 对称正定 → `ldlt()`
- 最小二乘 → `householderQr()`

### 检查求解是否成功

可以通过检查 `info()` 返回值的方式，知道求解器时候求解成功。

```c++
Eigen::Matrix3d A;
A << 1, 2, 3,
     4, 5, 6,
     7, 8, 9;  // 奇异矩阵（不可逆）

Eigen::Vector3d b(1, 2, 3);

auto solver = A.colPivHouseholderQr();
Eigen::Vector3d x = solver.solve(b);

if (solver.info() == Eigen::Success) {
    std::cout << "求解成功: " << x.transpose() << std::endl;
} else {
    std::cout << "求解失败（矩阵可能奇异）" << std::endl;
}
```



## 特征值与特征向量

### 本征问题

对于方阵 $A$ ，如果存在非零向量 $v$ 和标量 $\lambda$ 满足：
$$
A \cdot v = \lambda \cdot v
$$
则 $\lambda$ 称为特征值，$v$ 称为特征向量。

对于本征值问题，方阵 $A$ 是已知的，特征值 $\lambda$ 和特征向量 $v$ 是未知的，需要求解。

### 求解器

Eigen 中的本征值求解器有：

| 求解器                              | 适用矩阵    | 特征值类型        | 说明         |
| :---------------------------------- | :---------- | :---------------- | :----------- |
| `EigenSolver`                       | 任意方阵    | 复数              | 通用，最慢   |
| `SelfAdjointEigenSolver`            | 实对称/自伴 | 实数              | 常用，快     |
| `ComplexEigenSolver`                | 复数矩阵    | 复数              | 用于复数矩阵 |
| `RealSchur`                         | 实方阵      | 实数（Schur形式） | 中间结果     |
| `ComplexSchur`                      | 复数方阵    | 复数（Schur形式） | 中间结果     |
| `GeneralizedSelfAdjointEigenSolver` | 对称正定    | 实数              | 广义特征值   |

**新手建议：**

- 实对称矩阵 → `SelfAdjointEigenSolver`
- 其他情况 → `EigenSolver`

### 基本用法

步骤：

1. 创建求解器：`SomeSolver<MatrixType> solver(A);`
2. 获取特征值：`solver.eigenvalues(); `
3. 获取特征向量（可选）：`solver.eigenvectors();`

以通用求解器 `EigenSolver` 为例，其他求解器类似。

```c++
#include <iostream>
#include <Eigen/Dense>

int main() {
    Eigen::Matrix3d A;
    A << 2, 1, 0,
         1, 2, 1,
         0, 1, 2;
    
    // 创建求解器
    Eigen::EigenSolver<Eigen::Matrix3d> solver(A);
    
    // 获取特征值（复数类型）
    Eigen::Vector3cd eigenvalues = solver.eigenvalues();
    std::cout << "特征值:\n" << eigenvalues << std::endl;
    
    // 获取特征向量（复数类型）
    Eigen::Matrix3cd eigenvectors = solver.eigenvectors();
    std::cout << "特征向量:\n" << eigenvectors << std::endl;
    
    return 0;
}
```

### 广义特征值问题

**定义**：
$$
A v = \lambda Bv
$$
其中，$A$ 和 $B$ 是已知的。

**求解器**

广义特征值问题需要使用 `GeneralizedSelfAdjointEigenSolver` 求解器。

例子：

```c++
#include <iostream>
#include <Eigen/Dense>

int main() {
    Eigen::Matrix3d A, B;
    A << 2, 1, 0,
         1, 2, 1,
         0, 1, 2;
    
    B << 1, 0, 0,
         0, 1, 0,
         0, 0, 1;  // B 必须是正定对称
    
    Eigen::GeneralizedSelfAdjointEigenSolver<Eigen::Matrix3d> solver(A, B);
    
    Eigen::Vector3d eigenvalues = solver.eigenvalues();
    std::cout << "广义特征值: " << eigenvalues.transpose() << std::endl;
    
    return 0;
}
```



