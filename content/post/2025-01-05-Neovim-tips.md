---
title:       "Neovim Tips"
subtitle:    ""
description: "Notes on Neovim and LazyVim"
date:        "2025-01-05T10:46:42Z"
author:      "Hao Xu"
image:       ""
tags:        ["Vim", "NeoVim", "LazyVim"]
categories:  ["Tech" ]
draft:       false
---

Leader Key: \<space\>

全部退出nvim：\<leader\>qq

# 普通模式

重复上一步命令：.

## 光标移动

显示行号：:set number

普通模式左下上右移动：h、j(jump)、k(climb)、l(left)

向左下上右移动5个单位：5h、5j、5k、5l

跳转到第一行：gg

跳转到第N行：Ngg or NG or :N

跳转到最后一行：G



### 单词移动
移动至下一个单词开头：w

移动至下N个单词开头：Nw

移动至上一个单词开头：b

移动至上N个单词开头：Nb

移动至上一个单词结尾：ge

移动至上N个单词结尾：Nge

移动至下一个单词结尾：e

移动至下N个单词结尾：Ne


### 行内移动
移动至行首：|

移动至此行第一个字符：^或0（数字）

移动至行尾：$

移动至当前行下第N行行尾：N$


### TODO移动
上一个TODO：[t

下一个TODO：]t


移动至当前屏第一行：

移动至当前屏最后一行：


## 标记和跳转

【a-z只能用于文件内标记与跳转，A-Z同时标记文件可用于不同文件间跳转】

标记某当前行：m\[A-Za-z\]

跳转到标记行：(单引号)’\[A-Za-z\]

跳转到标记行时光标所在位置：(反引号)`\[A-Za-z\]

删除标记：delmarks \[A-Za-z\]

删除所有标记：delmarks!

删除后不再使用标记：:wshada!


## 翻转

向下翻一页：Ctrl+F

向上翻一页：Ctrl+B

向下翻半页：Ctrl+D（向下）

向上翻半页：Ctrl+U（向上）

向下翻一行，保持当前光标不动：Ctrl+E

向上翻一行，保持当前光标不动：Ctrl+Y

当前行移到顶部：zt/z+Enter

当前行移到底部：zb

当前行移到中间：zz


## 当前行移动


## 撤销与重做

撤消上一个操作：u

撤销上一个撤销：Ctrl+r


## 选择与搜索

全选：\<C-a\>

选择：见可视模式

向下搜索：/关键字

向上搜索：?关键字

搜索回车确定，然后进行选择

搜索光标所在的单词：*


搜索窗口待办清单：\<leader\>st

搜索窗口Tudo/Fix/Fixme：\<leader\>sT

分窗Tudo/Fix/Fixme：\<leader\>xt

分窗Tudo/Fix/Fixme：\<leader\>xT

前一个TODO：[t

下一个TODO：]t


取消搜索高亮：\<leader\>nh

下一个搜索结果：n

上一个搜索结果：N


搜索字符X：fX

下一个搜索结果：f

上一个搜索结果：F


## 删除

【删除内容会复制到剪切板】

删除行：dd

删除单词/光标之后的单词剩余部分：dw

删除以光标开始的N个字：Ndw

删除光标当前字符：x

光标之后的该行部分：d$


## 复制粘贴

复制光标所在行：yy

复制光标下第N行：Nyy


## 缩进

向右增加缩进：Shift+>

向左减少缩进：Shift+<


## Neo-tree

打开/关闭Neo-tree (当前文件夹所在目录)：\<leader\>e

打开/关闭Neo-tree (启动nvim时的root dir)：\<leader\>E

新建文件：a

新建文件夹：A或a文件名以/结尾

重命名：r

删除文件/文件夹：d

显示隐藏文件（.gitignore中的文件会隐藏）：H

从Neo-tree和文件窗口间移动：Ctrl-W+h/l


## Neogen

生成注释：<leader>cn


## Buffer

前一个Buffer：[b

后一个Buffer：]b

删除当前Buffer：\<leader\>bd


## 分割窗口及其移动与大小调整

分割窗口：

水平分割：\<leader\>- or ss

垂直分割：\<leader\>| or sv


删除分割的窗口：\<leaader\>x or \<leader\>wd

回到之前的窗口：\<leader\>n or \<leader\>ww or \<C-\\\>


转到别的窗口：

左：sh

下：sj

上：sk

右：sl


<!--调整窗口大小：-->
<!--左：<C-h>-->
<!--下：<C-j>-->
<!--上：<C-k>-->
<!--右：<C-l>-->



## 诊断

取消诊断：\<leader\>ud

# 插入模式

## 进入与退出插入模式

在光标位置插入：i

在光标后插入：a

在行首插入：I

在行尾插入：A

在光标下另起一行：o

在光标上另起一行：O


退出插入模式：jk or Esc


## 自动补全

选择上一个：\<C-p\>

选择下一个：\<C-n\>

打开补全：\<C-space\>已被Mac切换输入法快捷键占用

关闭补全：\<C-e\>


格式化：\<leader\>cf

# 可视模式

进入可视模式：v

# Reference

https://www.lazyvim.org/keymaps

