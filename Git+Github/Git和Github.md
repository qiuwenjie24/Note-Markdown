## Git

### 介绍

Git一个版本控制软件，Git可以在本地托管项目代码，并且通过Git可以管理Github上的项目代码，实现本地云端同步(备份)，也可以共享他人。



### 下载安装

官网下载地址：https://git-scm.com/download/win

安装时候，除了修改安装路径，其余的默认点下一步就好。

安装完成后，鼠标右键桌面可以看到多了，Git GUI 和 Git Bash。Git GUI是通过图形界面实现Git的操作，Git Bash是通过命令行实现操作，我们一般是用Git Bash操作Git。



### 在Git上设置用户名和邮箱

一般用户名和邮箱使用的是Github上的用户名和邮箱。因为当通过Git把本地仓库同步到Github的远程仓库中时，它会显示是谁(哪个Github用户)提交的。

(只需要设置一次，之前设置过则不用设置)

1. 设置用户名为YourName：`git config --global user.name 'YourName'`

2. 设置邮箱为youremail@yourdomain.com：`git config --global user.email 'youremail@yourdomain.com'`

3. 查看设置信息：`git config --list`，检查你的用户名user.name和邮箱user.email是否已经设置对了。

注意：`git config -global`表示对Git中都使用这个配置信息，当然也可以对单个仓库指定不同的用户名和邮箱。



### 一般工作流程

工作区(磁盘)---暂存区(stage/index)---Git仓库(head)

1. **初始化本地Git仓库**

   进入一个已经创建好的文件夹，右键选择`Git Bash`，在命令行中输入`git init`，则创建一个初始化的仓库。在隐藏项目中可以看到一个.git文件夹，这就是那个仓库。



2. **添加/删除文件**

   `git add *`，将当前所有文件放入暂存区，(只有与仓库中上一次提交的文件有不同的才会被提交)，并记录操作，包括删除操作。（==一般只记住这个命令就行==）

   `git add *.txt`，将当前所有为`txt`类型的文件放入暂存区，并记录操作。

   `git add .`，将当前目录下的文件都放入暂存区，并记录。

   `git add 1.txt`，只添加`1.txt`文件进缓存区/暂存区。暂存区也在.git文件夹中，所以暂存区算是Git仓库的一部分。

   `git rm 1.txt`，删除磁盘中`1.txt`文件并记录在暂存区，之后从暂存区提交时候会把删除文件这个操作记录提交上去。如果手动删除，或者`rm 1.txt`删除，则git会检测到文件已被删除，但需要`git add`把删除操作添加到暂存区并提交，才会被记录到版本历史中。

   `git restore --staged 1.txt`，在暂存区中移除`1.txt`文件。不影响磁盘文件。

   `git restore --staged .`，在暂存区中移除当前目录下的所有文件。

   ==注： * 和 . 的用法可以推广到其他命令。==



3. **查看文件状态**

   使用`git status`可以查看当前(文件夹中)所有文件的状态。

   红色文件名则表示是磁盘中未提交的文件，绿色文件名表示提交进暂存区的文件。

   如果显示`working directory clean`，则表明磁盘中的文件都提交进仓库，暂存区也没有文件。



4. **提交文件到Git仓库(head)**

   `git commit -m '这次版本提交的注释'`，将暂存区的所有文件提交到仓库，并为这次提交附上注释，注释不能为空。一次提交为一个版本，仓库记录会记录每次版本的变化。



5. **将本地仓库同步到Github**

   **基本流程**：将Github仓库克隆(下载)到本地---修改文件并上传(覆盖)Git仓库---提交到远程仓库

   **克隆远程仓库(HTTPS版本)**：`git clone https://github.com/username/repository.git(仓库HTTPS URL)`，将远程仓库克隆到本地的当前目录下，相应仓库地址(HTTPS)可以在Github中找到。也可以在Github中直接手动下载到本地。

   **连接远程仓库**：`git remote add origin https://github.com/username/repository.git`，通过HTTPS方式将git与远程仓库建立连接，其中`origin`是默认远程仓库名称，你可以随便取，只是为了有多个远程仓库时本地能够识别是哪个远程仓库，`https://...`远程仓库地址(HTTPS)，将 `username/repository.git` 替换为你的 GitHub 用户名和仓库名称。

   **推送至远程仓库**：`git push -u origin main`，首次将本地仓库提交到远程仓库，`origin`就是刚才你给远程仓库起的名字；`main`就是要推送的本地分支，一般可以在路径看到当前的分支，如`~/Desktop/git_new (main)`中的`main`；`-u`表示上传`main`分支并与远程仓库中相应的分支进行合并，并把`origin main`记录为默认，后续再提交到远程仓库只需要`git push`即可。
   
   ==注意==：你在执行上述操作中，可能每次传输它会弹出窗口要你输入github账号密码并授权，这是因为我们采用的是HTTPS方式传输。如果不想每次输入密码，或者和他人合作你需要给别人上传远程的权限，这时可以采用SSH加密传输，具体操作看下面。



6. **建立本地仓库与远程仓库之间SSH传输**

   **生成 SSH 密钥**：在终端打开`Git Bash`，(即点击桌面下栏的Windows图标，找到并打开`Git Bash`，这时的路径就是终端）。并输入命令并回车，`ssh-keygen`，之后会显示创建的 ssh 存放的位置。继续回车，如果之前创建过该文件则会问你是否(yes/no)覆盖(overwrite)。之后还会要你创建一个二级密码(passphrase)，一般不创建直接回车就行。 在上面给的路径中找到.ssh文件夹，里面 ssh keys(密钥对)，即 id_rsa(私钥) 和 id_rsa.pub(公钥) 文件。

   **配置 SSH 公钥**：在自己github账号主页的右上角，点击自己用户头像，选择`Setting-->SSH and GPG keys-->New SSH key`，填写 title 是便于你自己知道这个 ssh 是哪个本地的，在 key 处填写的是 id_rsa.pub 里的内容(以文本方式该文件查看内容)。点击`Add SSH key`，就完成了 SSH 的配置。
   
   **测试SSH连接**：输入`ssh -T git@github.com`，它可能会提示你是否继续连接连接，输入 `yes`，然后它会把GitHub公钥加入 `~/.ssh/known_hosts`，并继续连接，最终显示认证成功。
   
   **克隆远程仓库(SSH版本)**：`git clone git@github.com:username/repository.git(仓库SSH URL)`，将远程仓库克隆到本地的当前目录下，`username/repository`改成相应的Github用户名和仓库名。
   
   **连接远程仓库**：`git remote add origin git@github.com:username/repository.git`，将 `username/repository.git` 替换为你的 GitHub 用户名和仓库名称。如果本地仓库尚未关联任何远程仓库时，可以使用该命令添加第一个远程仓库。
   
   **将远程仓库 URL 切换为 SSH**：`git remote set-url origin git@github.com:username/repository.git`。如果本地仓库已经关联了一个远程仓库，但需要更改其 从 HTTPS 切换到 SSH，可以使用该命令。



7. **获取远程仓库更新**

   远程仓库的代码有更新，你希望同步到本地，可以使用以下命令：

   **(1).**`git fetch` + `git merge`：使用`git fetch origin`获取远程仓库最新更改，`git merge origin/main`将远程仓库的 `main` 分支合并到**当前所在**的本地分支。

   **(2).**`git pull origin main`：将远程仓库(origin)的`main`分支同步到本地。两种方式等价，`git pull` = `git fetch` + `git merge`。

   **(3).**`git pull origin main -m "Merge remote changes into local branch"`，可以使用`-m`选项提交这次合并的目的。如果不使用`-m`，Git 会弹出一个`vim`编辑器界面需要你解释为什么这次合并是必要的，按 `Esc` 键回到命令行模式，输入 `:q!` 然后按 `Enter` 键，直接退出编辑器。



### 其他

1. 修改本地分支名称。`git branch -m master main`，将当前分支`master`重命名为`main`。
2. 将本地指定分支推送到远程指定分支。`git push origin master:main`，将本地 `master` 推送到远程 `main`

## Github

### 介绍

github可以在云端托管项目代码，可以随时下载上传。文件的每次修改，删除或上传均会被记录，并可以查看每次操作的变化。



### 一些常用的功能

Repository：创建仓库(相当于文件夹)，用于存放项目代码

Star：收藏别人仓库

Watch：关注其他用户

Fork：可以克隆别人的仓库到自己仓库中

Issue：可以通过issue对该仓库提一些建议

Pull require：请求别人是否同意把自己的仓库合并或更新到对方的原来仓库中



#### 搭建用户个人网站(博客)

1. 创建个人站点：新建一个仓库，且仓库名为 `用户名.github.io `   

2. 在该仓库中新建一个html类型的文件，该文件内容即为网站内容。

3. 访问该个人网站的地址：https://用户名.github.io

注意：该仓库中只能有html类型的文件，否则网页显示会报错。



### 搭建项目/仓库网站

- 创建项目站点步骤：进入项目/仓库主页---点击`settings`---点击`Launch automatic page generator`---添加/修改页面内容---点击`Continue to layouts`---设置网站的主题/布局---点击`Publish page`生成网站

- 访问该项目/仓库网站的地址：https://用户名.github.io/仓库名

- 修改网页内容：

  1.可以重走以上步骤修改网页内容。

  2.也可以在该仓库主页的另一个分支中找到该网页的html文件，进行修改

  可以在网站上很好地向别人展示该仓库的的信息，并提供一键下载等。







----

### 一些教程

[git入门教程bilibili](https://www.bilibili.com/video/BV1dV411G77N/?spm_id_from=333.1365.top_right_bar_window_history.content.click&vd_source=c534af34cf9e1dfc22a3ffcfa050847e) (==看过，感觉讲得很好很详细==)

[新手入门 玩转Github](https://www.bilibili.com/video/BV14X4y1u7co/?share_source=copy_web&vd_source=ef181c8f58da73206e53092297016991) (==看过，比较简单，能够知道大概的流程==)

==下面的都没怎么认真看过==

官网教程： [Git - 安装 Git (git-scm.com)](https://git-scm.com/book/zh/v2/起步-安装-Git)

菜鸟教程： [Git 安装配置 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-install-setup.html)

廖雪峰教程： [简介 - Git教程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://liaoxuefeng.com/books/git/introduction/)

《GitHub入门与实践》：网盘  [文件 (lanzouo.com)](https://wwc.lanzouo.com/i4BWko0gfje)，密码：7aik (已下载)

一些常用的 Git 命令和小技巧： [521xueweihan/git-tips: :trollface:Git的奇技淫巧 (github.com)](https://github.com/521xueweihan/git-tips)

教程： [图解Git (marklodato.github.io)](https://marklodato.github.io/visual-git-guide/index-zh-cn.html)

教程： [git - the simple guide - no deep shit! (rogerdudler.github.io)](https://rogerdudler.github.io/git-guide/index.zh.html)

