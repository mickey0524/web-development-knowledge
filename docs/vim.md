## Basics
命令|功能
---|---
Esc/Ctrl-[|进入命令模式或撤销先前命令
:q|退出
:wq|保存并退出
:q!|不保存退出

## Move
命令|功能
---|---
h|左一列
l|右一列
0|行首
\$|行尾
f"|当前行下一个 "
F"|当前行上一个 "
t"|当前行下一个 " 之前
T"|当前行上一个 " 之后
;|重复上一个 f/F/t/T
,|反向重复上一个 f/F/t/T
j|下一行
9j|向下第 9 行
k|上一行
9k|向上第 9 行
H|屏幕第一行
M|屏幕中间一行
L|屏幕最后一行
gg|文件第一行
G|文件最后一行
nG|第 n 行
Ctrl-f|向下滚动一页
Ctrl-b|向上滚动一页
Ctrl-d|向下滚动半页
Ctrl-u|向上滚动半页
w|下一单词开头
b|上一单词开头
e|单词末尾
\*|下一个相同单词
\#|上一个相同单词
{|段落开头
}|段落末尾
%|匹配的括号
g;|上一个修改点
g,|下一个修改点
zz|使当前行居中
ma|标记当前位置为 a
'a|跳转到标记 a
''|回到跳转前的位置

## Edit
命令|功能
---|---
i|从光标前插入
a|从光标后插入
I|从行首插入
A|从行尾插入
x|删除当前字符
r|替换当前字符
s|删除当前字符并插入
o|从下方另起一行插入
O|从上方另起一行插入
d?|删除 dw db dd D d0 d{ d}
c?|删除并插入 cw cb cc C c0 c{ c}
y?|复制 yw ye yb yy y\$ y0 y{ y}
p|在光标后粘贴
P|在光标前粘贴
u|撤销
Ctrl-r|重做
.|重复上次操作
J|合并行
~|大小写转换
v|字符可视模式
V|行可视模式
Ctrl-v|块可视模式
\>|向右缩进（可视模式）
<|向左缩进（可视模式）
=|自动格式化（可视模式）
\>>|向右缩进当前行
<<|向左缩进当前行
==|自动格式化当前行
?i(|d/c/y/v () 以内的内容
?a(|d/c/y/v 包括 () 在内的内容
Ctrl-a|加 1
Ctrl-x|减 1
qa?q|录制宏并记录为 a
@a|执行宏 a
@@|执行最近录制的宏

## Search
命令|功能
---|---
/|向后搜索 / 后正则表达式
?|向前搜索 ? 后正则表达式
n|跳转到下一个匹配
N|跳转到上一个匹配
:s/a/b/g|将当前行的 a 替换为 b
:%s/a/b/g|将所有的 a 替换为 b

## Configuration
```vim
set nocompatible
filetype off

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" Plugins
Plugin 'VundleVim/Vundle.vim'
Plugin 'fatih/vim-go'
Plugin 'cohlin/vim-colorschemes'

" All of your Plugins must be added before the following line
call vundle#end()
filetype plugin indent on

" Put your non-Plugin stuff after this line
syntax on
set encoding=utf-8
set fileencodings=ucs-bom,utf-8,utf-16,gbk,gb18030,big5,latin1
set tabstop=4 shiftwidth=4 expandtab
set number
set hlsearch
color py-darcula
vnoremap <C-c> "*y
```

