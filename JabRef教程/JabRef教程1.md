## JabRef

#### 软件介绍

JabRef 是一个开源的参考文献管理软件，使用 Java 语言编写，具有跨平台特性。它可以很方便地管理下载到本机的文献，生成 BibTeX 文献数据库，供 LaTeX 或其它软件使用，可以与 Kile, Emacs, Vim, WinEdt 等多种软件结合使用。



#### 安装与下载

JabRef主页：http://jabref.sourceforge.net/

下载地址： [downloads.jabref.org](https://downloads.jabref.org/)

安装教程：https://docs.jabref.org/installation

windows直接点击*JabRef Windows Installer* 下载就完事了。

主界面(5.3版)

![image-20210920152450222](C:\Users\dell\Desktop\JabRef教程\教程1图片\1-1.png).

切换中文界面：
**options-> Preference-> General-> 语言Simplified Chinese, 默认编码UTF-8**

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-2.png" alt="image-20210920150914321" style="zoom:50%;" />.

保存后，重新打开软件才能生效。





#### 新建数据库

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-3" alt="image-20210920152736210" style="zoom:50%;" />.

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-4" alt="image-20210920153231516" style="zoom: 25%;" />.



#### 导入文献条目(entry)

1. 手动添加

 从菜单栏中选择**Library → New entry**

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-5" alt="image-20210920154231325" style="zoom:50%;" />.

选择条目的类型，这里我选择了*Article*。

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-6" alt="image-20210920154431352" style="zoom:33%;" />.

之后在*Required Fields(必填项)* 中填写相关信息。

![image-20210920154652640](C:\Users\dell\Desktop\JabRef教程\教程1图片\1-7).

其中*citationkey*是最重要的，你可以自己设定个命名规则给每个条目一个citationkey。“citationkey”的概念来自于BibTeX，在BibTeX中，每个条目都必须有且只有一个对应的标识符，即citationkey。

**注意：Bibtexkey是在LaTex中引用该文献的关键字，因此必须和文件中其他文献的key值不同，而且不能出现中文字符，否则会引用失败。**

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-8" alt="image-20210920155714679" style="zoom:33%;" />.

点击旁边的*generate*也可以自动生成citationkey。默认的格式是：*[auth] [year]*，如上面例子中的 Turing1950 。默认格式也可以在设置中修改(详情自己百度)。

也可以在其他tag上补充更多的信息。这个过程很麻烦，但JabRef可以通过公共数据库来补全其他信息：

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-9" alt="image-20210920160847667" style="zoom:50%;" />.

选择一个数据库查找条目的信息，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-10" alt="image-20210920161345076" style="zoom:50%;" />.

如果找到了其他相关的信息则会弹出对话框，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-11" alt="image-20210920162131150" style="zoom:50%;" />.

左侧是原先的信息，右边是搜索到的信息，你可以在相应的行中选择要左边的信息或有点的信息，或者都不要，即*None*，（中间选项）。之后点击 *Merge entries* 合并信息。



2.通过ID添加

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-12" alt="image-20210920163231755" style="zoom:50%;" />.

3.文本添加 （略）

4.使用在线数据库搜索参考资料并添加 （略）

5.通过pdf添加

方法一：把文件拖进来(可能需要拖到两个条目之间)，JabRef会自动解析文件生成信息(需要网络)。

方法二：**Lookup -> Search **

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-13" alt="image-20210920170634801" style="zoom: 33%;" />.

选择路径及相关要求，点击*Search*搜索相应路径下符合要求的文件 ，搜索的结果不包含已在库中的文件。

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-14" alt="image-20210920171810002" style="zoom:33%;" />.

*Export selected* 会生成一个文件列表的text文件，每行包含一个文件名及其路径，如下图，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-15" alt="image-20210920171902015" style="zoom:33%;" />.

点击 *Import* 则会生成生成对应的条目导入库中。

之后会显示导入结果(如下)，如果有导入失败的，可能是由于识别不了pdf某些信息等等，查看原因自行解决，不行就百度

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-16" alt="image-20210920172800035" style="zoom:50%;" />.

导入的条目可能需要进行一些编辑，因为从PDF文件中收集的所有信息可能不准确。



6.通过浏览器插件添加（略）

可以通过官方浏览器扩展([Firefox](https://addons.mozilla.org/en-US/firefox/addon/jabref/?src=external-github) - [Chrome](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh) - [Edge](https://microsoftedge.microsoft.com/addons/detail/pgkajmkfgbehiomipedjhoddkejohfna) - [Vivaldi](https://chrome.google.com/webstore/detail/jabref-browser-extension/bifehkofibaamoeaopjglfkddgkijdlh))自动识别和提取网站上的书目信息，点击可以将它们发送到JabRef。(据说这种方法不好，不推荐使用)。

7.通过导入窗口添加

**File->Import**（略）

8.通过BibTex代码添加

这里以Google学术为例，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-17" alt="image-20210922142210418" style="zoom:33%;" />.

![image-20210922142241312](C:\Users\dell\Desktop\JabRef教程\教程1图片\1-18).

![image-20210922142333986](C:\Users\dell\Desktop\JabRef教程\教程1图片\1-19).

同样地，有时候BibTex文件会有问题，得到的信息不对，这时需要手动修改。

#### 保存数据库

**File->Save library**

保存成功之后会生成两个文件(bib文件和bib.bak文件），例如

![image-20210922113656264](C:\Users\dell\Desktop\JabRef教程\教程1图片\1-20).（asd是我命名的文件名字)

用text打开bib.bak会显示数据库中文献的相应代码(标准BibTex代码)，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-21" alt="image-20210922114012680" style="zoom: 25%;" />.



#### 管理文献条目

可以通过Groups将你库中的文件分类整理。

group 主界面位于左侧的侧窗格，

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-22" alt="image-20210929110219749" style="zoom: 50%;" />.

通过**View → Groups interface**或者`Alt + 3`可以开启(关闭)Group的主界面.

1.新建Group并添加entry进去

可以通过放下方的**Add group**新增一个群。通过右键点击group可以编辑，添加子群等操作。

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-23" alt="image-20210929112053479" style="zoom:50%;" />.

也可以把已有的entry拖入新建的group，这样就分配给了新的group











#### 在LaTex中自动生成参考文献

**调用前需要将.bib文件放在.tex文件的同一目录的文件夹下**。

在LaTex中生成参考文献用到的语句不多，分别为：

```
 \cite{Bibtexkey}
 % 插入引用文献标记，Bibtexkey为所引用文献的key值；在正文需要引用的地方添加即可
 
 \nocite{Bibtexkey}
 \nocite{1} %只加入到参考文献列表中，不在文中引用（显示[id]）
 \nocite{*} %显示所有文献
 % 正文中未出现引用标记，但依然需要其出现在参考文献中时，使用此代码。被引用过的文献将自动出现在参考文献部分。
 
 \bibliographystyle{plain}
 % 设置文献参考样式，LaTex中参考文献标准样式有八种，具体信息参见下面

 \bibliography{bibfile}
 % 选择调用的BibTex文件，bibfile为文件名；该语句放在Latex文中的位置，相应数据库中的参考文献也将出现在pdf中的对应位置
```

注意，bib文件中只有引用的文献才会在参考文献列表中被列举出来。

*\cite* 用于正文中的引用，*\nocite*用于补充未在文中引用的参考文献，*\bibliography* 用于生成参考文献，*\bibliographystyle*用来调格式。

但似乎用\bibliography即可生成该数据库中的文献不需要用\notice。



**对于JabRef支持的一些编辑器可以更快速引用：**

1.要推送引文，首先在库中的条目表中选择你想推送的条目。然后点击**Tools ->Push entries to external application**，或者快捷键CTRL + L，或者点击图标。

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-24" alt="image-20210922145752151" style="zoom:33%;" />.

一般情况 下，默认的外部编辑器是TeXstudio。可以通过**Options ->Preferences -> External programs**修改成其他的编辑器。似乎这个功能只支持列表中有的编辑器(Emacs ,Lyx, Texmake, TeXstudio,Vim,WinEdt)。

<img src="C:\Users\dell\Desktop\JabRef教程\教程1图片\1-25" alt="image-20210922150043596" style="zoom:50%;" />.

**八种LaTex中参考文献标准样式：**

plain：按字母的顺序排列，比较次序为作者、年度和标题；
unsrt：样式同plain，只是按照引用的先后排序；
alpha：用作者名首字母+年份后两位作标号，以字母顺序排序；
abbrv：类似plain，将月份全拼改为缩写，更显紧凑；
ieeetr：国际电气电子工程师协会期刊样式；
acm：美国计算机学会期刊样式；
siam：美国工业和应用数学学会期刊样式；
palike:美国心理学学会期刊样式



**Tex文件写好后编译需要四步：**(现在似乎直接编译即可?)

用LaTeX编译：找到 .tex文件中引用的 .bib文件及风格，并生成一个 .aux 的文件；
用BibTeX编译：通过 .aux文件确定从哪个 .bib文件中引用文献，以及引用的格式和排序，并写入 .bbl文件；
用LaTeX编译：找到并读取 .bbl文件，将交叉引用数据写入.aux文件，这时会写入参考文献，但引用编号可能不正确；
用LaTeX编译：根据交叉数据确定文献编号，此时文献将正常显示。
