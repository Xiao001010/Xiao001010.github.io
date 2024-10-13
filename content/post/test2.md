---
title:       "Test2"
subtitle:    ""
description: ""
date:        "2024-10-13T00:33:57+01:00"
author:      ""
image:       ""
tags:        ["Vim", "Test"]
categories:  ["Tech" ]
draft:       false
---
Description answer: 

If the description in front matter is not provided, it will display top lines above the \<!--more--\> tag. If the description is provided, it will display the description in front matter. 

Using the tag &lt;!--more--&gt; to separate the content. The content above the tag will be displayed in the list view. The content below the tag will be displayed in the detail view. 

<!--more-->
What if first line is not a header and with out description?

Test line 1

# Test line 2 with header 1

Test line 3

Test line 4

Test line 5

## vim graphical cheat sheet

Test line 6

Test line 7

Test line 8

Test with more below
<!--more-->

Test line 9

Test line 10

## Test line 11 with header 2
<!--![](//img/2018-02-09-vim-tips/vi-vim-cheat-sheet.svg)-->
<!--more-->
## Vim Jumps

* ^ — Move to start of line
* $ — Move to end of line
* b — Move back a word
* w — Move forward a word
* e — Move to the end of the next word
* Ctrl-o and Ctrl-i to go to the previous/next location you jumped to
* ``(two backticks) jump back to where you were
* gi go back to the last place you inserted a text and enter insert mode

## Vim Navigations

* { and } jump paragraph back and forth
* Ctrl-F/B move one screen back and forth
* Search the word under cursor, then n/p to jump to next/previous 


## Enable Vim mode in bash
vi ~/.inputrc
set editing-mode vi

## Enable system clipboard upport

See if system clipboard is supported:     
```
$ vim --version | grep clipboard
-clipboard       +iconv           +path_extra      -toolbar
+eval            +mouse_dec       +startuptime     -xterm_clipboard
```

Rinstall vim as vim-gnome:   
```
sudo apt-get install vim-gnome
```
Select what you want using the mouse - then type to copy to clipboard:  
```
"+y
```

To paste to vim from clipboard type:  
```
"+p
```
## Others
* Ex: open the current directory
* set number: show line number
