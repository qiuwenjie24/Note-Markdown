## Linux

### 基本介绍

操作系统可以分为三类：linux、window、mac os，其中后两个是专有软件系统，第一个是开源操作系统。

linux是一款免费开源并且集可靠、安全、稳定于一身的多平台操作系统，兼具图形化界面操作和命令行操作。

基本结构：linux内核、GNU工具、图形化桌面、应用软件。

Linux发行版系统是对原始的Linux系统架构上进行一些改进加工的系统，使得对用户更友好更方便。原始的Linux系统相当于毛坯房，而Linux发行版系统相当于装修之后的房子。

Linux发行版系统：ubuntu、debian、Arch Linu、Kali Linux等。

(一般推荐使用ubantu，对新手更好友)



----

### 安装系统

在windows电脑上安装Linux操作系统的方式：WSL2，双系统，虚拟机等。

WSL 是 Windows 内嵌的 Linux 子系统，它相当运行着window系统，把Linux系统挂在后台，通过这种方式可以同时运行两个系统。其中 WSL2 采用 Hyper-V 虚拟化，性能接近原生 Linux。

(一般推荐新手使用WSL2安装，因为它更轻量、配置简单)

双系统安装使用的是原生的Linux系统，但电脑不能同时运行Linux和window两个系统，切换系统需要重启。

虚拟机安装是在 Windows 系统内创建一个完整的虚拟化 Linux 环境，不需要重启电脑即可同时运行 Windows 和 Linux，但是不支持 GPU 加速，不太适合用在深度学习。



**1.使用WSL2 安装 Linux**

系统要求：Windows 10 版本 2004 及以上或 Windows 11。WSL2 是 Windows 的内置功能，不需要下载。

启动WSL2：

1. 在“开始菜单”中搜索 “PowerShell”，右键选择 **“以管理员身份运行”**。
2. 输入命令`wsl --install`并回车。该命令会默认安装最新的 Ubuntu 发行版。
3. 重启系统，使得前面的更改操作能够生效。
4. 再次以管理员模式打开 PowerShell，执行`wsl --set-default-version 2`。该命令设置默认版本为 WSL2，确保你未来安装的 Linux 发行版都将以 WSL2 的模式运行。

**2.安装 Linux 发行版**

1. 通过 PowerShell（管理员模式）执行命令，`wsl --list --online`，查看所有可用的 Linux 发行版。

2. 安装指定的 Linux 发行版。比如你想安装最新版的 Ubuntu，可以使用命令：`wsl --install -d Ubuntu`。

3. 启动已安装的发行版。通过命令`wsl`启动，或者在“开始菜单”中点击 **Ubuntu** 图标启动。启动后会默认进入的 Linux 环境。

4. 装完成后，首次启动时会提示你创建一个 Linux 用户名和密码。

   用户名：`qwj`，密码`qiuwenjie`

5. 在 Ubuntu 终端中(而非PowerShell)，输入以下命令更新系统软件包，`sudo apt update && sudo apt upgrade -y`，获取更新信息，并将所有软件都更新到最新版本。

6. 可以通过命令`exit`，退出Linux 环境。

注意：WSL2 是默认安装在 Windows 系统盘中。

**3.迁移磁盘**

。。。。==待更新==。。。



-------



### 常用的基础命令：

### **1. 文件与目录管理命令**

- **`ls`**
  列出当前目录下的文件和子目录。常用参数：
  - `ls -l`：显示详细信息（权限、大小、时间等）。
  - `ls -a`：显示所有文件（包括隐藏文件）。
- **`pwd`**
  显示当前工作目录的完整路径。
- **`cd`**
  切换目录。
  - 示例：`cd /home/username` 切换到指定目录；`cd ..` 返回上级目录。
- **`mkdir`**
  创建新目录。
  - 示例：`mkdir myfolder` 创建一个名为 myfolder 的目录。
- **`rmdir`**
  删除空目录。
  - 示例：`rmdir myfolder`
- **`touch`**
  创建一个空文件或更新文件的时间属性。
  - 示例：`touch file.txt`，如果文件file.txt不存在，则会创建一个空内容的文本文件。
- **`cp`**
  复制文件或目录。
  - 示例：`cp source.txt destination.txt` 复制文件；加上 `-r` 参数可复制目录（例如 `cp -r sourcedir targetdir`）。
- **`mv`**
  移动或重命名文件和目录。
  - 示例：`mv oldname.txt newname.txt`（重命名）；`mv file.txt /path/to/destination/`（移动文件）。
- **`rm`**
  删除文件或目录。
  - 示例：`rm file.txt` 删除文件；`rm -r foldername` 递归删除目录及其中所有内容（使用时要小心）。

------

### **2. 查看文件内容的命令**

- **`cat`**
  直接显示文件内容，适合小文件。
  - 示例：`cat file.txt`
- **`less`** 和 **`more`**
  分页查看较长的文件内容，支持上下翻页。
  - 示例：`less file.txt`
- **`head`** 和 **`tail`**
  分别查看文件的开头和结尾部分内容。
  - 示例：`head -n 10 file.txt` 查看前 10 行；`tail -n 10 file.txt` 查看最后 10 行。

------

### **3. 查找与搜索相关命令**

- **`find`**
  在目录中搜索符合条件的文件或目录。
  - 示例：`find /home/username -name "*.txt"` 查找指定目录下所有后缀为 .txt 的文件。
- **`grep`**
  在文件或输出内容中搜索匹配的字符串。
  - 示例：`grep "hello" file.txt` 搜索文件中包含 “hello” 的行；可以结合管道符（`|`）使用，如 `ls -l | grep "^d"` 查找目录。

------

### **4. 权限与用户管理**

- **`chmod`**
  修改文件或目录的权限。
  - 示例：`chmod 755 script.sh` 设置权限为所有者可读写执行，组用户和其他用户可读执行。
- **`chown`**
  修改文件或目录的所有者。
  - 示例：`chown user:group file.txt` 将文件所有者修改为 user，同时修改所属组为 group。
- **`sudo`** (super user do的简称)
  以超级用户（root）权限执行命令（注意：使用时需谨慎）。
  - 示例：`sudo apt update` 以管理员权限更新软件包列表。

------

### **5. 系统管理与监控命令**

- **`apt`**（适用于 Debian/Ubuntu 系统）
  软件包管理工具，常用子命令：
  - `apt update`：更新软件仓库中的包信息。
  - `apt upgrade`：升级已安装的软件包。
  - `apt install packagename`：安装新的软件包。
- **`ps`**
  列出当前运行的进程。
  - 示例：`ps aux` 显示所有用户的进程详细信息。
- **`top`**
  实时显示系统资源使用情况和进程信息。
- **`kill`**
  终止指定的进程。
  - 示例：`kill 1234` 结束进程号为 1234 的进程；常配合 `ps` 或 `top` 使用。
- **`df`**
  显示文件系统的磁盘空间使用情况。
  - 示例：`df -h` 以人类可读的格式显示磁盘使用量。
- **`du`**
  显示目录或文件占用的磁盘空间。
  - 示例：`du -sh foldername` 查看目录的总大小。
- **`man`**
  查看命令的使用手册，非常有帮助。
  - 示例：`man ls` 查看 `ls` 命令的详细使用说明。

------

### **6. 其他常用命令**

- **`echo`**
  输出字符串到终端或写入文件。
  - 示例：`echo "Hello World"` 输出 “Hello World”。
- **`history`**
  显示已执行过的命令历史，便于重复使用或查找之前的命令。
- **`clear`**
  清屏，清除当前终端显示的内容。

