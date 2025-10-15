## Python和VSCode

#### 介绍

python是一种编程语言。Vscode是一个编译器。

#### 下载安装

一般运行python需要搭建python环境和编辑器（编译器/解释器/IDE），这里我们选择的编辑器为VScode。

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



#### Vscode中选择解释器

当你创建新的虚拟环境，想使用新的环境下的python解释器，可以通过以下步骤：

1. 使用快捷键`ctrl+shift+P`打开命令面板。

2. 在命令面板中输入`Python: Select Interpreter`，然后选择该选项。

3. 在弹出的列表中，会显示系统和所有环境中可用的python解释器，选择你想要环境的python解释器。

4. 选择后，Vscode会自动切换到新的环境，并在编辑器底部的状态栏(右下角)显示当前激活的环境。

   状态栏中`{} python`表示当前使用的python语言，`3.10.16('pytorch_cpu')`表示当前处在虚拟环境`pytorch_cpu`中，并且使用的是3.10.16版本的python。

   如果后续想要切换成其他环境中的python解释器，可以点击`3.10.16('pytorch_cpu')`，在弹出的列表中选择想要切换的python解释器。

5. 如果在弹出的列表中没有找到你想要的环境，那么你可以点击点击列表中的`Enter interpretet path`-->`Find`-->在你想要的环境的路径下选择python解释器`python.exe`。



#### 一些注意事项

1. 变量名区分大小写。

2. 对缩进有强制性要求，使用缩进来表示代码块。

3. 不需要分号`;` 。

4. 循环语句，if语句需要带双引号`:`。

5. 终止程序的快捷键： `ctrl+c`。

6. 一般习惯上，值会发生变动的变量名用全小写，值恒定不变的变量名用全大写。