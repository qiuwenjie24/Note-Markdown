在python上搭建神经网络模型，除了需要python3，还需要anaconda，pytorch(CUDA版/CPU版)。CUDA其实就是GPU。

## Anaconda

#### 介绍

anaconda是一个流行的开源数据科学平台，它主要提供以下几个功能：

1. 一站式安装：anaconda中包含了许多常用的数据科学和机器学习库，省去以后使用再去安装的麻烦。
2. 环境管理：允许用户创建和管理多个独立的虚拟环境，这个功能很重要，可以为不同项目配置不同的环境。
3. 包管理：anaconda中自带有conda，conda是一个包管理器，类似于pip包管理器，可以安装、更新和管理python中的库。
4. Jupyter Notebook：anacon自带Jupyter Notebook，它是一个python的IDE，是一个基于网页端的编辑器，写一些比较短的代码时比较方便。

大家做机器学习，深度学习的时候，一般是冲着1，2功能才去下载anaconda。不用anaconda也能搞机器学习，但会很麻烦，一般都推荐下载使用。



#### 下载安装

在官网上下载，地址为 https://www.anaconda.com/download/success .

安装时候，除了修改安装路径，剩下的默认点击下一步就行了。如果之前没有安装python，那么在安装过程也可以勾选安装python的选项。

安装好后可以在window(开始)的所有应用中找到anaconda文件夹。

Anaconda Navigator ：通过图形化界操作anaconda，一般不推荐用。

Anaconda Prompt ：通过命令行操作anaconda，一般大家都使用这个。

Jupyter Notebook ：python的一种基于网页端的IDE。



#### 使用

`conda create -n pytorch_cpu`：创建一个虚拟环境，并命名为`pytorch_cpu`，命名可以自由设定。创建过程中会问你是否创建，需要回复 `y` 。特别地，如果你需要指定该环境中python的版本，比如3.11版本，那么可以使用`create -n pytorch_cpu python=3.11` 。

`conda info -e`：查看已经创建的虚拟环境以及所在的位置，其中`base`是初始自带的环境，其他的是另外创建的虚拟环境。带`*`号的表示当前所在的环境。

`conda activate pytorch_cpu` ：激活环境 `pytorch_cpu(你已创建有的虚拟环境的名称)` ，激活之后当前环境就变成了 `pytorch_cpu` 环境。

`conda deactivate`：退出当前环境，回到`base`环境。

`conda list`：查看当前环境中已安装的库和依赖。

`conda install pandas`：在当前环境安装`pandas`库，安装其他库也同理，改一下名称即可。

`python --version`：查看当前环境的python版本。

`conda env remove --name pytorch_cpu`：删除名称为`pytorch_cpu`的虚拟环境，注意删除的环境不能是当前所在的环境。删除环境之后可能在环境目录有一些残留的文件夹，可以手动检查并删除，因此可以在删除之前查看一下环境所在的位置，`conda info -e`。

`conda create --name pytorch_new --clone pytorch_old`：克隆环境，创建一个新的环境`pytorch_new`，并将旧环境`pytorch_old`中的所有包和设置复制到新环境`pytorch_new`中。



注：`conda`中并没有提供直接的命令来重命名环境，如果想重命名环境，可以通过克隆的方式将旧环境复制到新环境，然后再删除旧环境。另外，在你使用编译器的时候可能需要重新激活一次环境。



## PyTorch

#### 介绍

PyTorch是一个开源的深度学习框架，可以在python，c++和java上使用。目前的最新版本是2023.03发布的PyTorch2.0。

pytorch分为CPU版本和GPU版本，CPU版只能处理规模较小的模型，GPU版可以处理较大的模型，因为GPU的并行计算能力强。



#### 下载安装

官网：https://pytorch.org/

1. 官网主页下滑，可以看到`Install PyTorch`。依次选择：`stable`  --> `Windows` --> `conda(如果你前面安装了)` --> `python` --> `CPU` --> `复制命令` 。
2. 打开Anaconda Prompt --> 激活一个虚拟环境，即把当前环境变更到另一个虚拟环境，如pytorch_cpu，命令为 `conda activate pytorch_cpu` --> 在当前虚拟环境中安装pytorch，把第1步的命令复制到命令行中，如 `conda install pytorch torchvision torchaudio cpuonly -c pytorch`  --> 中途会问你要不要安装，回复 `y`。
3. 加入你下载的CUDA版本，那么你可以通过命令查看当前的CUDA的版本`python -c "import pytorch; print(torch.version.cuda)"`，如果正常显示版本号，则表示安装成功。





注：

1.只有Nvidia的显卡才能下载CUDA(GPU)版本。我的电脑的amd的，所以只能选择下载CPU版本。

2.下载的CUDA版本要根据电脑的显卡驱动版本决定，可以在Nvidia的控制面板查看驱动的版本号，然后去Nvidia官网查看可以适配的CUDA版本是多少。同时它可以向下兼容，如果你的驱动能适配高版本的CUDA，那低版本的CUDA也能适配。

3.安装pytorch时候，开vpn可能会好一些，因为它是在国外的服务器。





## Jupyter Notebook

#### 介绍

Jupyter Notebook是一个基于Web的交互式计算环境，可以实时返回当前代码的结果。



#### 使用

打开`Jupyter Notebook`，它会自动弹出一个网页 ----> 点击右上角的`New` ----> 点击`Python3` ----> 在网页写python代码，也支持库的导入。

注意：

1.虽然在网页上写代码，但是程序运行的内核是`Jupyter Notebook`，因此使用中不要关闭`Jupyter Notebook`。

2.导入的库必须是anaconda在当前的环境(默认base环境)中有这个库才可以。

3.自带的`Jupyter Notebook`是在base环境中的，如果你想在新建的环境中使用，那么需用conda进行包管理，下载到当前环境，`conda install jupyter notebook`。

4.直接打开的`Jupyter Notebook`是在base环境中的。在`anaconda`中激活新的环境，在新的环境的命令行中，输入`Jupyter Notebook`，则是在新环境中打开，可以调用当前环境中的库。





## d2l包

#### 介绍

d2l包是《动手学深度学习》中的一个python包，集成了书中的很多代码，方便实现很多功能。



#### 下载安装

1.激活已创建好的虚拟环境，如`conda deactivate pytorch_cpu`，这里`pytorch_cpu`是我的虚拟环境的名称。

2.查看当前环境的python版本号，`python --version`。目前`d2l` 只支持3.8~3.11，暂不兼容 Python 3.12版本。(很不巧，我的版本就是3.12)

2.1.解决方案一：你可以创建一个新的虚拟环境并指定python版本，`conda create -n d2l-env python=3.10`，其中`d2l-env`是虚拟环境的名称，`3.10`是python版本号。

2.2.解决方案二：在当前环境中降级python版本，`conda install python=3.11`，其中`3.11`表示降到3.11版本。之后更新当前环境的所有包，`conda update --all`，确保它们与当前版本兼容。

3.在当前虚拟环境下，我们可以使用`conda`安装或者`pip`安装。

3.1.使用`conda`安装，`conda install -c conda-forge d2l`。因为在`conda`默认频道上无法找到`d2l`，而通常在 `conda-forge` 频道中可以找到，因此需要多加上`-c conda-forge`。

3.2.因为`conda`中自带有`pip`，所以也可以用`pip`安装，`python -m pip install d2l`，其中`python -m pip` 可以确保是在当前的虚拟环境中进行。

4.确定安装成功。通过命令查看`d2l`版本号，`python -c "import d2l; print(d2l.__version__)"`，如果显示版本号，没有报出错误提示，则说明`d2l`安装成功。





