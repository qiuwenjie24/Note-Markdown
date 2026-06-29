# Python 基础教程4

# 系统设计

## 面向对象（OOP）

面向对象编程（Object-Oriented Programming，OOP）是一种**把数据（属性）和操作（方法）打包在一起**的编程方式。

### 类与对象的核心概念

| 概念                  | 解释                       | 类比                 |
| :-------------------- | :------------------------- | :------------------- |
| **类（Class）**       | 创建对象的**蓝图/模板**    | 房子的设计图纸       |
| **对象（Object）**    | 按照蓝图创建的**具体实例** | 按照图纸建出来的房子 |
| **属性（Attribute）** | 对象拥有的**数据**         | 房子的面积、颜色     |
| **方法（Method）**    | 对象能做的**操作**         | 开灯、关门           |

例子：

```python
class Car:           # 类（图纸）
    def __init__(self, brand, color):
        self.brand = brand   # 属性
        self.color = color   # 属性
    
    def drive(self):         # 方法
        print(f"{self.color}的{self.brand}正在行驶")

# 对象（实例）
my_car = Car("特斯拉", "红色")   # 实例化
my_car.drive()                   # 调用方法
```

注意：`self` 代表**当前实例对象本身** 。

### 类的完整结构

**结构**：类属性、实例属性、实例方法、类方法、静态方法

#### 类属性

类属性定义在类内部、方法外部，属于类本身。所有实例共享同一个值。

**修改类属性有两种方式：**

- 通过类名修改（影响这个类的所有实例）
- 通过实例修改（只影响这个当前实例）

```python
class Student:
    # 类属性：所有学生共享
    school = "北京大学"
    total_students = 0

# 方式1：通过类名修改（影响所有实例）
Student.school = "清华大学"
s1 = Student()
s1 = Student()
print(s1.school)  # 清华大学
print(s2.school)  # 清华大学

# 方式2：通过实例修改（注意：这不会修改类属性，而是给这个实例创建了一个同名的实例属性）
s1.school = "浙江大学"  # 只是给 s1 这个实例绑定了新的 school 属性
print(s1.school)  # 浙江大学（实例属性）
print(s2.school)  # 清华大学（类属性，没有被影响）
```

**注意**：通过实例修改类属性，实际上是在实例上创建了一个同名的实例属性，并没有真的修改类属性。

#### 实例属性

**实例属性**定义在 `__init__` 方法中，属于**每个实例自己**。每个实例有自己的值。

```python
class Student:
    def __init__(self, name, age):
        # 实例属性：每个学生有自己的名字和年龄
        self.name = name
        self.age = age

s1 = Student("张三", 18)
print(s1.name)  # 张三
```

#### 类属性vs实例属性对比

| 类属性           | 类属性                     | 实例属性                         |
| :--------------- | :------------------------- | -------------------------------- |
| **定义位置**     | 类内部，方法外部           | `__init__` 方法内，用 `self.xxx` |
| **属于谁**       | 属于类                     | 属于实例                         |
| **访问方式**     | `类名.属性` 或 `实例.属性` | `实例.属性`                      |
| **修改影响范围** | 影响所有实例               | 只影响当前实例                   |
| **适用场景**     | 共享数据、统计计数         | 每个对象独有的数据               |

#### 实例方法

**实例方法**是类中最常见的方法。它的第一个参数是 `self`，代表当前实例。

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    # 实例方法
    def introduce(self):
        # 能通过 self 访问实例属性
        return f"我叫{self.name}，今年{self.age}岁"
    
    def have_birthday(self):
        # 能修改实例属性
        self.age += 1

s = Student("张三", 18)
print(s.introduce())   # 我叫张三，今年18岁
s.have_birthday()
print(s.introduce())   # 我叫张三，今年19岁
```

#### 类方法

**类方法**的第一个参数是 `cls`，代表类本身。通过 `@classmethod` 装饰器来声明。

注意：**类方法是属于类的方法，调用时不需要实例存在。因为无法调用实例属性。**

```python
class Student:
    school = "北京大学"
    total = 0
    
    def __init__(self, name):
        self.name = name
        Student.total += 1   # 每创建一个学生，总数加1
        # self.total += 1 是在实例上创建新的实例属性，只影响当前实例
        # Student.total += 1 修改的是类属性，所有实例共享
        
    
    # 类方法，无法调用实例属性self.
    @classmethod
    def change_school(cls, new_school):
        # cls 代表 Student 这个类
        cls.school = new_school
    
    @classmethod
    def get_total(cls):
        return cls.total

# 调用类方法（可以直接用类名调用，不需要实例）
Student.change_school("清华大学")
print(Student.school)   # 清华大学
print(Student.get_total())  # 0（目前还没有创建任何实例）

s1 = Student("张三")
s2 = Student("李四")
print(Student.get_total())  # 2
```

**特点：**

- 第一个参数是 `cls`（代表类本身）
- 通过 `cls` 只能访问类属性，不能访问实例属性（因为还没有实例）
- 可以通过**类名**调用（`类名.方法()`），也可以通过实例调用
- 常用于：修改类属性、创建对象的替代方式

#### **类方法 vs 实例方法对比**

| 对比维度               | **实例方法**                            | **类方法**                             |
| :--------------------- | :-------------------------------------- | :------------------------------------- |
| **装饰器**             | 不需要                                  | `@classmethod`                         |
| **第一个参数**         | `self`（代表实例）                      | `cls`（代表类）                        |
| **调用方式**           | `实例.方法()`                           | `类名.方法()` 或 `实例.方法()`         |
| **能否访问实例属性？** | ✅ 能（通过 `self.属性`）                | ❌ 不能（没有 `self`）                  |
| **能否访问类属性？**   | ✅ 能（通过 `self.属性` 或 `类名.属性`） | ✅ 能（通过 `cls.属性` 或 `类名.属性`） |
| **能否修改类属性？**   | ✅ 能（但建议用类名）                    | ✅ 能（建议用 `cls`）                   |
| **能否被继承/重写？**  | ✅ 能                                    | ✅ 能                                   |
| **需要实例才能调用？** | ✅ **必须**有实例                        | ❌ 不需要，直接用类名                   |
| **典型用途**           | 操作实例数据（90%+ 的方法）             | 修改类属性、替代构造函数（工厂方法）   |

```python
class Demo:
    class_var = "类属性"
    
    def __init__(self, value):
        self.instance_var = value
    
    def instance_method(self):
        print(self.instance_var)   # ✅ 能访问实例属性
        print(self.class_var)      # ✅ 也能访问类属性
    
    @classmethod
    def class_method(cls):
        print(cls.class_var)       # ✅ 能访问类属性
        # print(cls.instance_var)  # ❌ 不能访问实例属性（没有实例）

# 调用方式不同
obj = Demo("实例属性")
obj.instance_method()    # 必须用实例调用
Demo.class_method()      # 直接用类名调用
obj.class_method()       # 也可以用实例调用（但不推荐）
```

#### 静态方法

**静态方法**没有特殊的第一个参数（不需要 `self` 或 `cls`）。它就是一个放在类里面的普通函数，通过 `@staticmethod` 装饰器来声明。

**特点：**

- 没有 `self` 或 `cls` 参数
- **不能**访问类属性或实例属性（除非作为参数传进来）
- 可以通过类名或实例调用
- 本质上就是把普通函数放进类里，只是为了**逻辑上的归类**

例子：

```python
class MathHelper:
    # 静态方法：纯粹的工具函数，放在类里只是因为逻辑上相关
    @staticmethod
    def add(a, b):
        return a + b
    
    @staticmethod
    def is_even(num):
        return num % 2 == 0

# 调用静态方法（直接用类名调用）
print(MathHelper.add(3, 5))     # 8
print(MathHelper.is_even(10))   # True

# 也可以通过实例调用（但不建议）
helper = MathHelper()
print(helper.add(1, 2))         # 3
```

### 三种方法对比总结

|                      | 实例方法      | 类方法                         | 静态方法                       |
| :------------------- | :------------ | :----------------------------- | :----------------------------- |
| **装饰器**           | 无            | `@classmethod`                 | `@staticmethod`                |
| **第一个参数**       | `self`        | `cls`                          | 无特殊参数                     |
| **能访问实例属性？** | ✅ 可以        | ❌ 不可以                       | ❌ 不可以                       |
| **能访问类属性？**   | ✅ 可以        | ✅ 可以                         | ❌ 不可以（除非传参）           |
| **调用方式**         | `实例.方法()` | `类名.方法()` 或 `实例.方法()` | `类名.方法()` 或 `实例.方法()` |
| **使用频率**         | ⭐⭐⭐⭐⭐ 最高    | ⭐⭐⭐ 中等                       | ⭐⭐ 较低                        |
| **典型用途**         | 操作对象数据  | 修改类属性、工厂方法           | 工具函数、验证函数             |

### 构造函数`__init__`

`__init__` 是 Python 中最重要的特殊方法，它在**创建对象时自动调用**，用于初始化对象。

```python
class Person:
    def __init__(self, name, age, city="未知"):
        self.name = name      # 必须提供的参数
        self.age = age        # 必须提供的参数
        self.city = city      # 有默认值
        self.created_at = datetime.now()  # 自动生成

# 创建对象时自动调用 __init__
p1 = Person("张三", 18)               # city使用默认值
p2 = Person("李四", 20, "北京")        # 指定city
```

注意：`self` 代表**当前实例对象本身** 。

### 属性与方法的访问控制

Python 通过**命名约定**来实现访问控制权限：

| 命名方式 | 含义         | 访问权限                                                |
| :------- | :----------- | :------------------------------------------------------ |
| `name`   | 普通属性     | **public公开**（约定：可被外部访问）                    |
| `_name`  | 单下划线开头 | **protected受保护**（约定：只能在类或子类访问）         |
| `__name` | 双下划线开头 | **private私有**（约定：只能在自己类中访问，子类也不行） |

注意：**Python 中所有属性本质上都是 public**，只是通过命名约定，告诉程序员哪些不能动，并为了与其他语言的访问控制概念保持一致。

### 继承

继承允许一个类**继承另一个类的属性和方法**，实现代码复用。

```python
# 父类（基类）
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def eat(self):
        print(f"{self.name} 正在吃东西")
    
    def sleep(self):
        print(f"{self.name} 正在睡觉")

# 子类（派生类），继承Animal类
class Dog(Animal):
    def bark(self):
        print(f"{self.name} 汪汪叫")
    
    # 重写父类方法（覆盖父类的同名方法）
    def eat(self):
        print(f"{self.name} 正在啃骨头")

class Cat(Animal):
    def meow(self):
        print(f"{self.name} 喵喵叫")
    
    def eat(self):
        print(f"{self.name} 正在吃鱼")

# 使用
dog = Dog("旺财", 3)
cat = Cat("咪咪", 2)

dog.eat()    # 旺财 正在啃骨头（子类重写的方法）
dog.bark()   # 旺财 汪汪叫
cat.eat()    # 咪咪 正在吃鱼
cat.meow()   # 咪咪 喵喵叫
```

如果需要调用父类的方法，使用 **`super()`** ：

```python
class Cat(Animal): # 继承 Animal
    def __init__(self, name, age, kind):
        super().__init__(name, age)  # 调用父类的构造函数 __init__
        self.kind = kind             # 子类新增属性
    
    def info(self):
        return f"{super().info()}，品种：{self.grade}"
```

### 多态

多态指**不同对象对（从父类继承的）同一个方法做出不同的响应**。

**多态 = 重写 + 继承 + 统一接口调用**

**重写（Override）**：子类中定义了与父类**同名**的方法，子类对象调用该方法时，**执行的是子类版本**，父类版本被覆盖。

特别地，Python 的多态甚至不需要继承关系，只要对象有同名方法就行。

**注意：**多态和重写的区别在于调用的时候，当你用同一个名字去调用不同的对象时，它们自动用自己的实现。重写是多态的基础，多态更强调统一接口调用。

例子：`shape` 对应不同对象时调用相应的方法。

```python
# 计算面积（未实现）
class Shape:
    def area(self):
        pass

# 计算圆的面积
class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    def area(self):
        return 3.14 * self.radius ** 2

# 计算矩阵面积
class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    def area(self):
        return self.width * self.height

# 多态：同样调用 area()，不同对象执行不同逻辑
shapes = [Circle(5), Rectangle(4, 6)]
for shape in shapes:
    print(shape.area())   # 78.5 和 24
```

