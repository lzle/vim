# vim 编辑器

`vi` 编辑器是 Unix 系统最初的编辑器。它使用控制台图形模式来模拟文本编辑窗口，允许查看文件中的行、在文件中移动、插入、编辑和替换文本。
尽管它可能是世界上最复杂的编辑器（至少讨厌它的人是这么认为的），但其拥有的大量特性使其成为 Unix 管理员多年来的支柱性工具。
在 GNU 项目将 vi 编辑器移植到开源世界时，他们决定对其作一些改进。由于它不再是以前 Unix 中的那个原始的 `vi` 编辑器了，
开发人员也就将它重命名为 vi improved 或 `vim`。本节将会带你逐步了解使用 `vim` 编辑器编辑文本 shell 脚本文件的基础知识。


## 目录

* [基础](#一基础)
    * [普通命令](#1普通命令)
    * [编辑数据](#2编辑数据)
    * [复制和粘贴](#3复制和粘贴)
    * [查找和替换](#4查找和替换)

* [配置](#二配置)
    * [基础知识](#1基础知识)
    * [基本配置](#2基本配置)
    * [缩进](#3缩进)
    * [外观](#4外观)
    * [搜索](#5搜索)
    * [编辑](#6编辑)

* [练习教程](#三vimtutor)

* [进阶](#四进阶)
    * [移动光标](#1移动光标)
    * [Undo/Redo](#2UndoRedo)
    * [组合命令](#3组合命令)
    * [区域选择](#4区域选择)
    * [单词补齐](#5单词补齐)
    * [可视化](#6可视化)
    * [分屏操作](#7分屏操作)
    * [多文件操作](#8多文件操作)

* [插件](#插件)
    * [管理工具](#管理工具)
    * [NERDTree](#NERDTree)



## 一、基础

### 1、普通命令

`vim` 编辑器有两种操作模式：

- [x] 普通模式
- [x] 插入模式

在大的文本文件中一行一行地来回移动会特别麻烦，幸而 `vim` 提供了一些能够提高移动速度的命令。

- [x] PageDown（或Ctrl+F）：下翻一屏。
- [x] PageUp（或Ctrl+B）：上翻一屏。
- [x] G：移到缓冲区的最后一行。
- [x] num G：移动到缓冲区中的第 num 行。
- [x] gg：移到缓冲区的第一行。

在普通模式下按下冒号键，会进入命令行模式，命令行模式提供了一个交互式命令行，可以输入额外的命令来控制 `vim` 的行为。

- [x] q：如果未修改缓冲区数据，退出。
- [x] q!：取消所有对缓冲区数据的修改并退出。
- [x] w filename：将文件保存到另一个文件中。
- [x] wq：将缓冲区数据保存到文件中并退出。


### 2、编辑数据

`vim` 编辑器提供了一些命令来编辑缓冲区中的数据。

|命 令|描 述|
|---|---
| x | 删除当前光标所在位置的字符
| dd | 删除当前光标所在行
| dw | 删除当前光标所在位置的单词
| d$ | 删除当前光标所在位置至行尾的内容
| J | 删除当前光标所在行行尾的换行符（拼接行）
| u | 撤销前一编辑命令
| a | 在当前光标后追加数据
| A | 在当前光标所在行行尾追加数据
| r char | 用char替换当前光标所在位置的单个字符
| R text | 用text覆盖当前光标所在位置的数据，直到按下ESC键


### 3、复制和粘贴

`vim` 中复制命令是 `y`，`yw` 表示复制一个单词，`y$` 表示复制到行尾，`yy` 表示复制当前的行，
在复制文本后，把光标移动到你想放置文本的地方，输入p命令。复制的文本就会出现在该位置。

按下 `v` 命令可进入可视模式，你会注意到光标所在位置的文本已经被高亮显示了。下一步，
移动光标来覆盖你想要复制的文本，在覆盖了要复制的文本后，按 `y` 键来激活复制命令，使用 `p` 命令来粘贴。

### 4、查找和替换

替换命令允许你快速用另一个单词来替换文本中的某个单词。必须进入命令行模式才能使用替换命令。替换命令的格式是：

- [x] :s/old/new/g：一行命令替换所有 old。
- [x] :n,ms/old/new/g：替换行号n和m之间所有 old。
- [x] :%s/old/new/g：替换整个文件中的所有 old。 
- [x] :%s/old/new/gc：替换整个文件中的所有 old，但在每次出现时提示。


## 二、配置

### 1、基础知识

`vim` 的全局配置一般在 /etc/vim/vimrc 或者 /etc/vimrc，对所有用户生效。用户个人的配置在 ~/.vimrc。

如果只对单次编辑启用某个配置项，可以在命令模式下，先输入一个冒号，再输入配置。举例来说，set number 这个配置可以写在 .vimrc 里面，也可以在命令模式输入。

```shell
:set number
```

配置项一般都有"打开"和"关闭"两个设置。"关闭"就是在"打开"前面加上前缀"no"。

```shell
" 打开
set number

" 关闭
set nonumber
```

上面代码中，双引号开始的行表示注释。

查询某个配置项是打开还是关闭，可以在命令模式下，输入该配置，并在后面加上问号。

```shell
:set number?
```

上面代码中，双引号开始的行表示注释。

查询某个配置项是打开还是关闭，可以在命令模式下，输入该配置，并在后面加上问号。

上面的命令会返回 number 或者 nonumber。

如果想查看帮助，可以使用help命令。

```shell
:help number
```

### 2、基本配置

``` shell
set nocompatible
```

不与 `vi` 兼容（采用 `vim` 自己的操作命令）。

``` shell
syntax on
```

打开语法高亮。自动识别代码，使用多种颜色显示。


```shell
set showmode
```

在底部显示，当前处于命令模式还是插入模式。

```shell
set showcmd
```

命令模式下，在底部显示，当前键入的指令。比如，键入的指令是 2y3d，那么底部就会显示 2y3，当键入 d 的时候，操作完成，显示消失。

```shell
set mouse=a
```

支持使用鼠标。

```shell
set encoding=utf-8 
```

使用 utf-8 编码。

```shell
set t_Co=256
```

启用256色。

```shell
filetype indent on
```

开启文件类型检查，并且载入与该类型对应的缩进规则。比如，如果编辑的是.py文件，`vim` 就是会找 Python 的缩进规则~/.vim/indent/python.vim。

### 3、缩进

```shell
set autoindent
```

按下回车键后，下一行的缩进会自动跟上一行的缩进保持一致。

```shell
set tabstop=2
```

按下 Tab 键时，`vim` 显示的空格数。

```shell
set shiftwidth=4
```

在文本上按下 >>（增加一级缩进）、<<（取消一级缩进）或者 ==（取消全部缩进）时，每一级的字符数。

```shell
set expandtab
```

由于 Tab 键在不同的编辑器缩进不一致，该设置自动将 Tab 转为空格。

```shell
set softtabstop=2
```

Tab 转为多少个空格。


### 4、外观

```shell
set number
```

显示行号

```shell
set relativenumber
```

显示光标所在的当前行的行号，其他行都为相对于该行的相对行号。

```shell
set cursorline
```

光标所在的当前行高亮。

```shell
set textwidth=80
```

设置行宽，即一行显示多少个字符。

```shell
set wrap
```

自动折行，即太长的行分成几行显示。

```shell
set nowrap
```

关闭自动折行

```shell
set linebreak
```

只有遇到指定的符号（比如空格、连词号和其他标点符号），才发生折行。也就是说，不会在单词内部折行。

```shell
set wrapmargin=2
```

指定折行处与编辑窗口的右边缘之间空出的字符数。

```shell
set scrolloff=5
```

垂直滚动时，光标距离顶部/底部的位置（单位：行）。

```shell
set sidescrolloff=15
```

水平滚动时，光标距离行首或行尾的位置（单位：字符）。该配置在不折行时比较有用。

```shell
set laststatus=2
```

是否显示状态栏。0 表示不显示，1 表示只在多窗口时显示，2 表示显示。

```shell
set  ruler
```

在状态栏显示光标的当前位置（位于哪一行哪一列）。

### 5、搜索

```shell
set showmatch
```

光标遇到圆括号、方括号、大括号时，自动高亮对应的另一个圆括号、方括号和大括号。

```shell
set hlsearch
```

搜索时，高亮显示匹配结果。

```shell
set incsearch
```

输入搜索模式时，每输入一个字符，就自动跳到第一个匹配的结果。

```shell
set ignorecase
```

搜索时忽略大小写。

```shell
set smartcase
```

如果同时打开了 ignorecase，那么对于只有一个大写字母的搜索词，将大小写敏感；其他情况都是大小写不敏感。比如，搜索 Test 时，将不匹配test；搜索 test 时，将匹配 Test。

### 6、编辑

```shell
set spell spelllang=en_us
```

打开英语单词的拼写检查。

```shell
set nobackup
```

不创建备份文件。默认情况下，文件保存时，会额外创建一个备份文件，它的文件名是在原文件名的末尾，再添加一个波浪号（〜）。

```shell
set noswapfile
```

不创建交换文件。交换文件主要用于系统崩溃时恢复文件，文件名的开头是.、结尾是.swp。

```shell
set undofile
```

保留撤销历史。

`vim` 会在编辑时保存操作历史，用来供用户撤消更改。默认情况下，操作记录只在本次编辑时有效，一旦编辑结束、文件关闭，操作历史就消失了。

打开这个设置，可以在文件关闭后，操作记录保留在一个文件里面，继续存在。这意味着，重新打开一个文件，可以撤销上一次编辑时的操作。撤消文件是跟原文件保存在一起的隐藏文件，文件名以 .un~ 开头。

```shell
set backupdir=~/.vim/.backup//  
set directory=~/.vim/.swp//
set undodir=~/.vim/.undo// 
```

设置备份文件、交换文件、操作历史文件的保存位置。

结尾的//表示生成的文件名带有绝对路径，路径中用%替换目录分隔符，这样可以防止文件重名。

```shell
set autochdir
```

自动切换工作目录。这主要用在一个 Vim 会话之中打开多个文件的情况，默认的工作目录是打开的第一个文件的目录。该配置可以将工作目录自动切换到，正在编辑的文件的目录。

```shell
set noerrorbells
```

出错时，不要发出响声。

```shell
set visualbell
```

出错时，发出视觉提示，通常是屏幕闪烁。

```shell
set history=1000
```

Vim 需要记住多少次历史操作。

```shell
set autoread
```

打开文件监视。如果在编辑过程中文件发生外部改变（比如被别的编辑器编辑了），就会发出提示。

```shell
set listchars=tab:»■,trail:■
set list
```

如果行尾有多余的空格（包括 Tab 键），该配置将让这些空格显示成可见的小方块。

```shell
set wildmenu
set wildmode=longest:list,full
```

命令模式下，底部操作指令按下 Tab 键自动补全。第一次按下 Tab，会显示所有匹配的操作指令的清单；第二次按下 Tab，会依次选择各个指令。



## 三、vimtutor

在 linux 系统中输入命令 `vimtutor`，可进入到 `vim` 教程学习中。

```shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                               第一讲小结


  1. 光标在屏幕文本中的移动既可以用箭头键，也可以使用 hjkl 字母键。
         h (左移)       j (下行)       k (上行)     l (右移)

  2. 欲进入 Vim 编辑器(从命令行提示符)，请输入：vim 文件名 <回车>

  3. 欲退出 Vim 编辑器，请输入 <ESC>   :q!   <回车> 放弃所有改动。
                      或者输入 <ESC>   :wq   <回车> 保存改动。

  4. 在正常模式下删除光标所在位置的字符，请按： x

  5. 欲插入或添加文本，请输入：

         i   输入欲插入文本   <ESC>             在光标前插入文本
         A   输入欲添加文本   <ESC>             在一行后添加文本

特别提示：按下 <ESC> 键会带您回到正常模式或者撤消一个不想输入或部分完整
的命令。

好了，第一讲到此结束。下面接下来继续第二讲的内容。

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                               第二讲小结


  1. 欲从当前光标删除至下一个单词，请输入：dw
  2. 欲从当前光标删除至当前行末尾，请输入：d$
  3. 欲删除整行，请输入：dd

  4. 欲重复一个动作，请在它前面加上一个数字：2w
  5. 在正常模式下修改命令的格式是：
               operator   [number]   motion
     其中：
       operator - 操作符，代表要做的事情，比如 d 代表删除
       [number] - 可以附加的数字，代表动作重复的次数
       motion   - 动作，代表在所操作的文本上的移动，例如 w 代表单词(word)，
                  $ 代表行末等等。

  6. 欲移动光标到行首，请按数字0键：0

  7. 欲撤消以前的操作，请输入：u (小写的u)
     欲撤消在一行中所做的改动，请输入：U (大写的U)
     欲撤消以前的撤消命令，恢复以前的操作结果，请输入：CTRL-R

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                  第三讲小结


  1. 要重新置入已经删除的文本内容，请按小写字母 p 键。该操作可以将已删除
     的文本内容置于光标之后。如果最后一次删除的是一个整行，那么该行将置
     于当前光标所在行的下一行。

  2. 要替换光标所在位置的字符，请输入小写的 r 和要替换掉原位置字符的新字
     符即可。

  3. 更改类命令允许您改变从当前光标所在位置直到动作指示的位置中间的文本。
     比如输入 ce 可以替换当前光标到单词的末尾的内容；输入 c$ 可以替换当
     前光标到行末的内容。

  4. 更改类命令的格式是：

         c   [number]   motion

现在我们继续学习下一讲。

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                               第四讲小结


  1. CTRL-G 用于显示当前光标所在位置和文件状态信息。
     G 用于将光标跳转至文件最后一行。
     先敲入一个行号然后输入大写 G 则是将光标移动至该行号代表的行。
     gg 用于将光标跳转至文件第一行。

  2. 输入 / 然后紧随一个字符串是在当前所编辑的文档中正向查找该字符串。
     输入 ? 然后紧随一个字符串则是在当前所编辑的文档中反向查找该字符串。
     完成一次查找之后按 n 键是重复上一次的命令，可在同一方向上查
     找下一个匹配字符串所在；或者按大写 N 向相反方向查找下一匹配字符串所在。
     CTRL-O 带您跳转回较旧的位置，CTRL-I 则带您到较新的位置。

  3. 如果光标当前位置是括号(、)、[、]、{、}，按 % 会将光标移动到配对的括号上。

  4. 在一行内替换头一个字符串 old 为新的字符串 new，请输入  :s/old/new
     在一行内替换所有的字符串 old 为新的字符串 new，请输入  :s/old/new/g
     在两行内替换所有的字符串 old 为新的字符串 new，请输入  :#,#s/old/new/g
     在文件内替换所有的字符串 old 为新的字符串 new，请输入  :%s/old/new/g
     进行全文替换时询问用户确认每个替换需添加 c 标志        :%s/old/new/gc
     

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                               第五讲小结


  1. :!command 用于执行一个外部命令 command。

     请看一些实际例子：
         (MS-DOS)         (Unix)
          :!dir            :!ls            -  用于显示当前目录的内容。
          :!del FILENAME   :!rm FILENAME   -  用于删除名为 FILENAME 的文件。

  2. :w FILENAME  可将当前 VIM 中正在编辑的文件保存到名为 FILENAME 的文
     件中。

  3. v motion :w FILENAME 可将当前编辑文件中可视模式下选中的内容保存到文件
     FILENAME 中。

  4. :r FILENAME 可提取磁盘文件 FILENAME 并将其插入到当前文件的光标位置
     后面。

  5. :r !dir 可以读取 dir 命令的输出并将其放置到当前文件的光标位置后面。
  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                               第六讲小结

  1. 输入小写的 o 可以在光标下方打开新的一行并进入插入模式。
     输入大写的 O 可以在光标上方打开新的一行。

  2. 输入小写的 a 可以在光标所在位置之后插入文本。
     输入大写的 A 可以在光标所在行的行末之后插入文本。

  3. e 命令可以使光标移动到单词末尾。

  4. 操作符 y 复制文本，p 粘贴先前复制的文本。

  5. 输入大写的 R 将进入替换模式，直至按 <ESC> 键回到正常模式。

  6. 输入 :set xxx 可以设置 xxx 选项。一些有用的选项如下：
        'ic' 'ignorecase'       查找时忽略字母大小写
        'is' 'incsearch'        查找短语时显示部分匹配
        'hls' 'hlsearch'        高亮显示所有的匹配短语
     选项名可以用完整版本，也可以用缩略版本。

  7. 在选项前加上 no 可以关闭选项：  :set noic

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                  第七讲小结


  1. 输入 :help 或者按 <F1> 键或 <Help> 键可以打开帮助窗口。

  2. 输入 :help cmd 可以找到关于 cmd 命令的帮助。

  3. 输入 CTRL-W CTRL-W  可以使您在窗口之间跳转。

  4. 输入 :q 以关闭帮助窗口

  5. 您可以创建一个 vimrc 启动脚本文件用来保存您偏好的设置。

  6. 当输入 : 命令时，按 CTRL-D 可以查看可能的补全结果。
     按 <TAB> 可以使用一个补全。
     
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```


## 四、进阶

### 1、移动光标

```
0   -> 数字零，到行头
^   -> 到本行第一个不是blank字符的位置（所谓blank字符就是空格，tab，换行，回车等）
$   -> 到本行行尾
w   -> 到下一个单词的开头，单词是由字母、数字、下划线组成的（不严格的说法）
e   -> 到下一个单词的结尾，单词是由字母、数字、下划线组成的（不严格的说法）
%   -> 匹配括号移动，包括 (, {, [. （你需要把光标先移到括号上）
ctrl + f -> 向下翻页
ctrl + b -> 向上翻页
( 和 ) 移到上一句和下一句
{ 和 } 移到上一段和下一段
```

### 2、修改类

```
x  -> 删除光标所在处的字符
r  -> 替换光标所在处的字符
dw -> 删除一个单词
de -> 删除到一个单词的末尾
d$ -> 删除到行尾
dt -> 删除到某个字符
cw ->  dw + i
ce ->  de + i
c$ ->  d$ + i
```

### 3、文本对象处理

文件内容如下，假设光标停在 “sesame” 的 “a” 上。

```
if (message == "sesame open")
```

基本附加键是 a 和 i。其中，a 可以简单理解为英文单词 a，表示选定后续动作要求的完整内容，
而 i 可理解为英文单词 inner，代表后续动作要求的内容的“内部”。

```
‘dw’（理解为 delete word）会删除“ame␣”，结果是“if (message == "sesopen")”
‘diw’（理解为 delete inside word）会删除“sesame”，结果是“if (message == " open")”
‘daw’（理解为 delete a word）会删除“sesame␣”，结果是“if (message == "open")”
‘di"’会删除“sesame open”，结果是“if (message == "")”
‘da"’会删除“"sesame open"”，结果是“if (message ==)”
‘di(’或‘di)’会删除“message == "sesame open"”，结果是“if ()”
‘da(’或‘da)’会删除“(message == "sesame open")”，结果是“if␣”
```


### 3、搜索类

```
/word   -> 向下搜索
?word   -> 向上搜索
n       -> 重复上一次的搜索
N       -> 反向重复上一次的搜索
```

### 3、Undo/Redo

```
u          -> 撤销最近的修改
U          -> 撤销对整行的修改
<ctrl+r>   -> 重做
```

### 4、组合命令

```
.   -> 重复上一个命令
3.  -> 重复 3 次
0y$ -> 先到行头，从这里开始拷贝，拷贝到本行最后一个字符
ye  -> 当前位置拷贝到本单词的最后一个字符
```

### 5、区域选择

```
在 visual 模式下，这些命令很强大，其命令格式为

<action>a<object> 和 <action>i<object>

action可以是任何的命令，如 d (删除), y (拷贝), v (可以视模式选择)。

object 可能是：w 一个单词，W 一个以空格为分隔的单词， s 一个句字， p 一个段落。也可以是一个特别的字符："、 '、 )、 }、 ]

假设你有一个字符串 (map (+) ("foo")).而光标键在第一个 o 的位置。

vi"  -> 会选择 foo
va"  -> 会选择 "foo"
vi)  -> 会选择 "foo"
va)  -> 会选择("foo")
v2i) -> 会选择 map (+) ("foo")
v2a) -> 会选择 (map (+) ("foo"))
```

### 6、单词补齐

在 Insert 模式下，你可以输入一个词的开头，然后按 `<ctrl-p>` 或是 `<ctrl-n>`，自动补齐功能就出现了


### 7、可视化

使用 v 和 V。一但被选好了，你可以做下面的事

```
J -> 把所有的行连接起来（变成一行，行之间有空白字符的话，可以用 gJ）
< 或 > -> 左右缩进
= -> 自动给缩进 （陈皓注：这个功能相当强大，我太喜欢了）
```

也可以使用 `<ctrl-v>` 加点东西

```
^           -> 到行头
<ctrl-v>    -> 开始块操作
<ctrl-d>    -> 向下移动 (你也可以使用hjkl来移动光标，或是使用%，或是别的)
I-- [ESC]   -> I是插入，插入“--”，按ESC键来为每一行生效。
```

### 8、分屏操作

```
:split        -> 创建分屏(或者使用缩写命令:sp)
:vsplit       -> 创建垂直分屏(或者使用缩写命令:vs)
<ctrl-w><dir> -> dir就是方向，可以是 hjkl 或是 ←↓↑→ 中的一个，其用来切换分屏。
<ctrl-w>=     -> 所有尺寸相等
<C-W>n 或 :new    -> 打开一个新窗口
<C-W>q 或 :quit   -> 退出当前窗口，当最后一个窗口退出时则退出 Vim
<C-W>o 或 :only   -> 只保留当前窗口，关闭其他所有窗口
```

### 9、多文件操作

```
:e <path/to/file> -> 打开一个文件
:w -> 存盘
:saveas <path/to/file> -> 另存为 <path/to/file>
:x， ZZ 或 :wq -> 保存并退出 (:x 表示仅在需要时保存，ZZ不需要输入冒号并回车)
:q! -> 退出不保存 :qa! 强行退出所有的正在编辑的文件，就算别的文件有更改
:bn 和 :bp -> 你可以同时打开很多文件，使用这两个命令来切换下一个或上一个文件
```

## 插件

### 管理工具

`vim-plug` 是一个轻量级的插件管理器，支持并行安装，可以在运行时异步加载插件。

#### 安装

Unix

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

#### 配置

Add a vim-plug section to your `~/.vimrc` (or `~/.config/nvim/init.vim` for Neovim)

```
call plug#begin()

" List your plugins here
Plug 'tpope/vim-sensible'

call plug#end()
```

Reload the file or restart Vim, then you can,

* :PlugInstall to install the plugins
* :PlugUpdate to install or update the plugins
* :PlugDiff to review the changes from the last update
* :PlugClean to remove plugins no longer in the list


### NERDTree

`NERDTree` 是一个文件资源管理器插件，可以让你在 Vim 中浏览文件系统，打开文件和目录。

#### 安装

```
call plug#begin()
  Plug 'preservim/nerdtree'
call plug#end()
```

Restart Vim, and run the `:PlugInstall` statement to install your plugins.

#### 快捷键

```
nnoremap <leader>n :NERDTreeFocus<CR>
nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
nnoremap <C-f> :NERDTreeFind<CR>
```

#### 配置

显示文件行数。

```
let g:NERDTreeFileLines = 1
```

执行 `:help NERDTree` 查看帮助。

```
o : open in prev window
go : preview
t : open in new tab
T : open in new tab silently
i : open split
gi : preview split
s : open vsplit
gs : preview vsplit
```


#### 相关插件

Adds file type icons to Vim plugins.

```
Plug 'ryanoasis/vim-devicons'
```

Use :help devicons for further configuration.



## 相关链接

[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)
[vim-plug](https://github.com/junegunn/vim-plug)
[nerdtree](https://github.com/preservim/nerdtree)
