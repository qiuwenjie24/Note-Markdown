

##### <font color='red'>用JabRef生成Latex参考文献</font>

https://blog.csdn.net/qq_21391921/article/details/78138139

https://www.jianshu.com/p/a727529e043f

https://blog.csdn.net/weixin_44191286/article/details/85698921







##### <font color='red'>关于JabRef文献PDF文件的相对路径设置</font>

请使用JabRef 2.5 以上版本，选择中文界面。

选项——》首选项——》外部程序——》文件主目录设置为引号内的“./”，使用正则表达式搜索设置为引号内的“**/.*[title].*//.[extension]”，然后点击“确定”。

将你的pdf文件放在bib文件相同目录下的子文件夹内，比如“files”，文档名称最好英文名。pdf文件名称就是paper的标题。

用jabref打开对应的bib数据库，点击单个条目，在“General”选项中，点击“自动”就能自动搜索对应条目的pdf文档了。

一般设置主目录就可以，建议将所有的pdf，word，等文档都放在主目录下的子文件夹内即可。

如果不在子文件下，则手动添加pdf的相对路径。

————————————————
版权声明：本文为CSDN博主「lrjnlp」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lrjnlp/article/details/5675135

##### <font color='red'>或者也可以这样设置</font>

 JabRef支持自动检索与bibtex相关的文献文件，支持常用的PDF与doc格式等等。它支持两种文件名检索，一种就是根据bibtexkey，一种就是根据title，默认的是根据bibtexkey，不过像我这种懒人，肯定是两个都想支持的，所以可以利用这个的正则表达式设置

**/.*([bibtexkey]|[title]).*\\.[extension]
以上含义就是在当前文件下递归检索符合bibtexkey或者title的文件名，而文件格式为JabRef支持的文件格式。

忘了说设置方法，到options->preferences->external programs->选中use regular expression search并输入以上的正则表达式
————————————————
版权声明：本文为CSDN博主「neofung」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/neofung/article/details/6921887





<font color='red'>JabRef 指南</font>

层级结构

JabRef 提出了**Library、Groups、SubGroups**、**Entries**的层级结构。

Library 对应于一个 .bib 文件，一般来说一篇 .tex 文件只会引用一个 .bib 文件。我目前的实践中，按照学科来管理 .bib 文件。所以我的文献库只有一个 `math.bib` . 因为不同库之间，记录除了复制粘贴外，没有办法交流或组织。

Groups 和 SubGroups 类似于子文件夹，可以自由删改，而不会影响文献本身的记录。只要你愿意，可以不停的建文件夹。值得注意的是，Groups只是一种组织形式，删除掉Groups并不会删除掉文献的记录。同一篇文献也可以出现在不同的组中。

Entries 就是文献的记录，一篇文献唯一对应于一个 Entries。

https://zhuanlan.zhihu.com/p/288131233