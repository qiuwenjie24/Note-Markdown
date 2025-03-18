## 一.安装 Sublime text 3

官网：

http://www.sublimetext.com/3

64位操作系统选择“Windows64bit”下载。

## 二.安装 Package Control

#### 1.Package Control

插件包管理器，可以帮助我们浏览、安装和卸载 Sublime Text 中的插件。

- `Preferences ---> Package Control`（快捷键`Ctrl+Shift+p`）

- 在弹出的输入框中选择`Package Control: Install Package` ，然后点击即可自动下载安装。
- 可通过右下角状态查看是否安装完成。

##### 2.LaTeXTools

用于编译转换latex语言

- 重复步骤：`Preferences ---> Package Control--->Package Control: Install Package`
- 在新的输入框中输入想要下载的插件，比如 `LaTeXTools`，点击即可自动下载安装

#### 3.LaTeX-cwl

LaTeX代码自动补全



## 三.使用sublime编译latex文件(window系统)：

1.需要下载有texlive，sumatraPDF，以及sublime中的latextools插件。

texlive下载及安装教程

https://zhuanlan.zhihu.com/p/138586028

sumatraPDF官网：

https://www.sumatrapdfreader.org/download-free-pdf-viewer

2.配置LaTeXTools，使之与texlive，sumatraPDF 连接一起使用：

1. 重置LaTeXTools配置：`Preference ---> Package Setting ---> LaTeXTools ---> Reset user settings to default`

2. 设置LaTeXTools配置：`Preference ---> Package Setting ---> LaTeXTools ---> Setting-User`

3. `ctrl+F`查找`texpath`，找到如下这段代码，然后进行下面的修改：

   1. 把`"texpath": ""`修改成自己的texlive的路径，`"texpath" : "D:\\SOFEWARE\\texlive2022\\2022\\bin\\win32;$PATH"`  ，记得路径中要用`\\`而不是`\`

   2. `"distro" : "miktex"`修改成`"distro" : "TeXlive"`

   3. `"sumatra": ""`修改成SumatraPDF的路径，`"sumatra": "D:\\SOFEWARE\\SumatraPDF\\SumatraPDF.exe"`

   4. 之后就可以编译tex文件了。

      `Ctrl+Shift+B`可以选择编译方式，推荐用pdfLatex，之后`Ctrl+B`就可以使用默认的pdfLatex了。

   ```json
   "windows": {
   		// Path used when invoking tex & friends; "" is fine for MiKTeX
   		// For TeXlive 2011 (or other years) use
   		// "texpath" : "C:\\texlive\\2011\\bin\\win32;$PATH",
   		"texpath" : "",
   		// TeX distro: "miktex" or "texlive"
   		"distro" : "miktex",
   		// Command to invoke Sumatra. If blank, "SumatraPDF.exe" is used (it has to be on your PATH)
   		"sumatra": "",
   		// Command to invoke Sublime Text. Used if the keep_focus toggle is true.
   		// If blank, "subl.exe" or "sublime_text.exe" will be used.
   		"sublime_executable": "",
   		// how long (in seconds) to wait after the jump_to_pdf command completes
   		// before switching focus back to Sublime Text. This may need to be
   		// adjusted depending on your machine and configuration.
   		"keep_focus_delay": 0.5
   	},
   ```

   3.设置sumatraPDF反选功能：

   sumatraPDF中选择 `菜单--->设置--->选项`，在最后一行`设置反向搜索命令行`中修改为sublime的路径 `"D:\SOFEWARE\Sublime Text 3\sublime_text.exe"%f":%l"`

## 设置sublime

`Preferences --->Setting`，在左边的文件时默认的设置，右边Preferences.sublime-settings-User文件中可以修改，添加设置

#### 1.调节字体

`Preferences --->Setting`，在右边`Preferences.sublime-settings-User`文件中输入 `"font_size": 10,` 即可，10为字体大小，可更改。

#### 2.调节字体样式

`"font_face": "Consolas",`

#### 3.sublime中文字体显示正常

`"font_options": ["gdi"],`

#### 4.拼写检查

`"spell_check": false,`



## 删除掉配置文件

`Preferences-->Browse Packages`，此处存着用户数据，删除文件，相当于恢复sublime初始设置。

一般卸载或者升级时配置文件时是不会删除，需要手动删除。

## 其他

1.编译中文则需要添加 `\usepackage{ctex} `  以及用xelatex 编译；若多添加UTF8 选项，`\documentclass{article}` ，则用pdflatex 编译也可以。原因是pdflatex 默然按照 GBK 编码处理 tex 文件，xelatex 的默认输入文件是 utf-8 编码。

2.单行注释 `ctrl+/` ，多行注释`ctrl+shift+/` 。

3.`Ctrl+Shift+B`可以选择编译方式.

4.编译器内正常显示中文简体字，在`preferences-settings`中添加

```json
"font_options": ["gdi"],  //正常显示中文必备
"font_face": "Consolas",  //字体样式
```

5.若语法无错误下编译eps图片，报错如下

`Package pdftex.def Error: File xxx-eps-converted-to.pdf' not found: using draft setting. `

则需要在pdflatex命令后面加一个参数 `-shell-escape`，sublime 添加的方法如下，在latextools用户设置里添加`"options": ["--shell-escape"]`,到`builder_settings`,

```json
"builder_settings" : {

    // General settings:
    // See README or third-party documentation

    // (built-ins): true shows the log of each command in the output panel
    "display_log" : false,
    "options": ["--shell-escape"],

    // Platform-specific settings:
    "osx" : {
        // See README or third-party documentation
    },

    "windows" : {
        // See README or third-party documentation
    },

    "linux" : {
        // See README or third-party documentation
    }
},
```

参考于[网站1](https://tex.stackexchange.com/questions/170723/custom-build-settings-in-sublime)，至于其他的编译器如何添加可以参考[网站2](https://tex.stackexchange.com/questions/598818/how-can-i-enable-shell-escape)或百度。注，此方法可能失效，若失效请换用texstudio，下载安装参考[网站3](https://zhuanlan.zhihu.com/p/138586028)。

6.`file --- new view into file`可以使当前文件再开一个视图。

7.文件名或文件的路径含有空格可能会影响sumatrapdf的反选。

8.界面的右下角可以选择文件的类型，比如latex，c++等。



## 快捷键

1. 临时显示菜单：`alt`
2. 输出控制台/输出框：`ctrl + ~`
3. 

