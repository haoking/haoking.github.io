---
layout:     post
title:      "MacVim and YouCompleteMe"
subtitle:   "Vim on MacOS and code-completion"
date:       2018-12-08 10:00:00
author:     "Haoking"
header-img: "post_Images/2018_12_08/Homeslider_05.jpg"
tags:
    - Vim
    - MacVim
    - YouCompleteMe
    - vimrc GitHub 
    - Code completion
---

> [Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



## **MacVim and YouCompleteMe**

**Why MacVim**

MacOS has obtained the Vim in the system, but why people use MacVim?

The reason is vim on Mac cannot use a lot of useful features like code-completion, or that is very complex to install it.

A good thing is MacVim can support almost all the features of full Vim version, also keep updating to newest Vim version. MacVim is the most recommend of Vim official website.

 Check it on:

```http
https://www.vim.org/download.php
```

**How to install MacVim**

Please believe, mostly the best way to install developing tools on MacOS is to use Homebrew.

```http
https://brew.sh/
```

To download Homebrew, you easily install within one line in Terminal:

```shell
# install Homebrew
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Then it is easy to install MacVim:

```shell
# install Vim
$ brew install vim
# install MacVim
$ brew install macvim
```

 Then you can try MacVim and find it is using  the newest Vim version.

```shell
# open MacVim
$ mvim
```

![1544152924122](_post_Images/2018_12_08/1544152924122.jpg)



**Link MacVim to Vim**

After linked, you can input "vi" or "vim" to open MacVim and set some Bundle.

```shell
# open MacVim
$ vim ~/.bash_profile

#Add these three lines to file:
 alias vi=vim
 alias vim=mvim
 alias mvim='/usr/local/bin/mvim -v'
```



**Configuration auto-completion and optimization**

"~/.vimrc" writes all configuration auto-completion and optimization. 

```shell
# if ~/.vimrc is not exit 
$ touch ~/.vimrc
```

There are two ways to figure out the setting.

The first way which I recommend is using the exit vimrc on GitHub. If you do not use vim before, you should try this. Cause this is the most star project which is focused on grab the best bundle in Vim:

https://github.com/amix/vimrc

The following is the way to finish:

```shell
$ git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
$ sh ~/.vim_runtime/install_awesome_vimrc.sh
```

That's all! The program makes it simple.



**Auto-Completion**

The world best auto-completion is YouCompleteMe:

https://github.com/Valloric/YouCompleteMe

This can support auto-completion on most of the languages, like Python,  Java, c-family language and so on.

Also, the speed of YouCompleteMe is breakneck.

Install YouCompleteMe on MacOS is different from another system.

```shell
//Install the newest Python use Homebrew
$ brew install python
//If you have the python
$ brew reinstall python

//link python
$ brew link python
```

Now, you have the newest python version which is 3.0.0+

***The next step is the most important one which I take two days to find it out.***

Because the MacVim and YouCompleteMe have some problems with the newest version.

To fix it, we can add a line to .vimrc

```shell
// open .vimrc
$ open ~/.vimrc
```

Add the following to the file at the top, and fix the details here, cause maybe you are not 3.7.1, change them.

```shell
set pythonthreehome=/usr/local/Cellar/python/3.7.1/Frameworks/Python.framework/Versions/3.7/
```



The following is my .vimrc

![1544212776677](/_post_Images/2018_12_08/1544212776677.jpg)



Then clone the YouCompleteMe to Vim.

```shell
//Install the YouCompleteMe to Vim
$ cd ~/.vim_runtime
$ git clone git://github.com/Valloric/YouCompleteMe.git my_plugins/YouCompleteMe

//Compile the project
$ cd ~/.vim_runtime/my_plugins/YouCompleteMe
//if just install Python
$ ./install.py
//if install Python and C-family language
$ ./install.py --clang-completer
//if install Python and C-family language and Rust and son on
$ ./install.py --all
```



Then That's it! Enjoy the auto-completion.

```shell
$ vim
```



The Screenshot following:

**Java:**

![1544213445804](/_post_Images/2018_12_08/1544213445804.jpg)



**Python:**

![1544231886487](/_post_Images/2018_12_08/1544231886487.jpg)



**C++:**

![1544232287849](/_post_Images/2018_12_08/1544232287849.jpg)





**Custom .Vimrc Configuration**

If you want to custom the configuration, you can follow the steps:

```shell
//open vimrc configuration file
$ open ~/.vimrc
```



And copy the following content:

```shell
set rtp+=~/.vim/bundle/vundle
"do not forget to fic this line to fit your own file
set pythonthreehome=/usr/local/Cellar/python/3.7.1/Frameworks/Python.framework/Versions/3.7/
call vundle#begin()


Bundle 'gmarik/vundle'
" Python补全强力插件
Bundle 'davidhalter/jedi'
" c-family 补全插件
Bundle 'Valloric/YouCompleteMe'
" 添加引号,括号配对补全
Bundle 'jiangmiao/auto-pairs'
" 添加/解除注释
Bundle 'scrooloose/nerdcommenter'

" 在这里添加你想安装的Vim插件

call vundle#end()            
" required

" 自动补全配置
set completeopt=longest,menu "让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
autocmd InsertLeave * if pumvisible() == 0|pclose|endif "离开插入模式后自动关闭预览窗口
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>" "回车即选中当前项
"上下左右键的行为 会显示其他信息
inoremap <expr> <Down> pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up> pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp> pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"

"youcompleteme 默认tab s-tab 和自动补全冲突
"let g:ycm_key_list_select_completion=['<c-n>']
let g:ycm_key_list_select_completion = ['<Down>']
"let g:ycm_key_list_previous_completion=['<c-p>']
let g:ycm_key_list_previous_completion = ['<Up>']
let g:ycm_confirm_extra_conf=0 "关闭加载.ycm_extra_conf.py提示

let g:ycm_collect_identifiers_from_tags_files=1 " 开启 YCM 基于标签引擎
let g:ycm_min_num_of_chars_for_completion=2 " 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0 " 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_seed_identifiers_with_syntax=1 " 语法关键字补全
nnoremap <F5> :YcmForceCompileAndDiagnostics<CR> "force recomile with syntastic
"nnoremap <leader>lo :lopen<CR> "open locationlist
"nnoremap <leader>lc :lclose<CR> "close locationlist
inoremap <leader><leader> <C-x><C-o>
"在注释输入中也能补全
let g:ycm_complete_in_comments = 1
"在字符串输入中也能补全
let g:ycm_complete_in_strings = 1
"注释和字符串中的文字也会被收入补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0

nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 一般设定
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 设定默认解码
set fenc=utf-8
set fencs=utf-8,usc-bom,euc-jp,gb18030,gbk,gb2312,cp936

"去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限
set nocompatible

" 显示中文帮助
if version >= 603
    set helplang=cn
    set encoding=utf-8
endif

" 语法高亮
syntax on

" 设置配色
"colorscheme solarized  
"可能需要提前下载

" 设置字体
set guifont=Monaco:h12

" 设置gvim启动窗口的位置，以及大小
" winpos 300 105
" set lines=30 columns=100

" 开启行号显示
set number

"下面两行在进行编写代码时，在格式对起上很有用；
"第一行，vim使用自动对起，也就是把当前行的对起格式应用到下一行；
"第二行，依据上面的对起格式，智能的选择对起方式，对于类似C语言编
"写上很有用
set autoindent
set smartindent

"查询时非常方便，如要查找book单词，当输入到/b时，会自动找到第一
"个b开头的单词，当输入到/bo时，会自动找到第一个bo开头的单词，依
"次类推，进行查找时，使用此设置会快速找到答案，当你找要匹配的单词
"时，别忘记回车
set incsearch

" 高亮当前行
set cursorline

" 启动的时候不显示那个援助索马里儿童的提示
set shortmess=atI

" 我的状态行显示的内容（包括文件类型和解码）
set statusline=%F%m%r%h%w\[POS=%l,%v][%p%%]\%{strftime(\"%d/%m/%y\ -\ %H:%M\")}

" 总是显示状态行
set laststatus=2

" 制表符为4
set tabstop=4

" 统一缩进为4
set softtabstop=4
set shiftwidth=4

" 在c,c++,python文件中用空格代替制表符
autocmd FileType c,cpp,python set shiftwidth=4 | set expandtab

" 侦测文件类型
filetype on

" 载入文件类型插件
filetype plugin on

" 为特定文件类型载入相关缩进文件
filetype indent on

    " 如果文件类型为.sh文件
    if &filetype == 'sh'
        call setline(1,
        "\#########################################################################")
        call append(line("."), "\# File Name: ".expand("%"))
        call append(line(".")+1, "\# Author: Hat_Cloud")
        call append(line(".")+2, "\# mail: jijianfeng529@gmail.com")
        call append(line(".")+3, "\# Created Time: ".strftime("%F %R"))
        call append(line(".")+4, "\#########################################################################")
        call append(line(".")+5, "\#!/bin/bash")
        call append(line(".")+6, "")

    else
        call setline(1, "/*************************************************************************")
        call append(line("."), "    > File Name: ".expand("%"))
        call append(line(".")+1, "    > Author: Hat_Cloud")
        call append(line(".")+2, "    > Mail: jijianfeng529@gmail.com ")
        call append(line(".")+3, "    > Created Time: ".strftime("%F %R"))
        call append(line(".")+4, " ************************************************************************/")
        call append(line(".")+5, "")
    endif
    if &filetype == 'python'
        call append(line(".")+5, "\#!/usr/bin/env python")
        call append(line(".")+6, "\#coding: utf-8")
        call append(line(".")+7, "")
    endif
    if &filetype == 'cpp'
        call append(line(".")+6, "#include<iostream>")
        call append(line(".")+7, "using namespace std;")
        call append(line(".")+8, "")
    endif
    if &filetype == 'c'
        call append(line(".")+6, "#include<stdio.h>")
        call append(line(".")+7, "")
    endif
    " 新建文件后，自动定位到文件末尾
    autocmd BufNewFile * normal G

```



Then you need to compile the YouCompleteMe like the steps I have shown previously.

Then that's it.

Enjoy your amazing MacVim.



Thanks!



If you like my blog, please :

[Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



By the way, if you have any questions or there is something wrong here, please contact me on Email:

haokinus@gmail.com 


