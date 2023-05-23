---
title: Add-Neovim-Vscode
date: 2023-04-29 17:43:15
tags:
- vim
- neovim
- plugin
- vscode
---

起因是为了找可以支持 surround.vim 插件相似效果的 vscode + vim 方案， 结果发现了 vscode 插件中除了 vim 插件外，还有 neovim 插件，并且 neovim 插件具有比 vim 插件更加丰富的 vim 特性（因为直接调用 neovim）。于是马上尝试把 vim 插件换成了 neovim 插件。确实效果还不错，只是配置更加麻烦了，无法“开箱即用”。 下面做一个简要介绍。

## 安装插件并集成于 vscode

第一步是按照 neovim 插件，没啥可讲的。

然后需要把系统的 neovim 路径配置到 neovim 插件的设置中。由于我的 windows 系统中没有安装过 neovim，因此还要去按照 neovim

## 安装 neovim

参考安装文档 [https://github.com/neovim/neovim/wiki/installing-neovim](https://github.com/neovim/neovim/wiki/installing-neovim)，我使用了 winget 方案安装：

```powershell
>winget install Neovim.Neovim
```

> 意外地，和 linux 系统下安装一样方便

## 找到安装路径并配置

安装好后，windows系统下，可以直接在桌面新出现的快捷方式右键选择 go to file location 找到安装路径。把同一目录下的 nvim.exe 路径配置到 neovim 插件配置中即可。

重新打开 vscode，不出意外就可以使用 neovim 映射方案了。

## neovim 配置文件

vim的配置文件是 .vimrc， nvim 的配置文件是 init.vim。问题是 init.vim 的位置。

参考 [https://jdhao.github.io/2018/11/15/neovim_configuration_windows/](https://jdhao.github.io/2018/11/15/neovim_configuration_windows/)，可以在一个nvim中使用命令找到配置文件目录：

```neovim
:echo stdpath('config')
```

底行会提示对应的配置路径。我查看对应路径，发现并没有已经创建的目录，于是还参考了[简书](https://www.jianshu.com/p/88dd20795263)创建配置文件和目录。

## 配置系统剪切板为无名寄存器

为了使默认的复制粘贴使用系统 clipboard, 在配置文件中做了以下配置：

```neovim
set clipboard=unnamed
```

## 插件安装

回归最初的任务，是希望使用插件。为此先参考了[这个视频](https://www.bilibili.com/video/BV15x4y1A7ff/?spm_id_from=333.337.search-card.all.click&vd_source=de5ac27060641adcdbb08086ca63b8a2)

首先安装 vim-plugin，这并不是希望安装的插件本身，而是一个插件安装器。根据其 [github 仓库](https://github.com/junegunn/vim-plug)的提示，选择 windows/neovim 版本进行安装：

```powershell
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni "$(@($env:XDG_DATA_HOME, $env:LOCALAPPDATA)[$null -eq $env:XDG_DATA_HOME])/nvim-data/site/autoload/plug.vim" -Force
```

然后再 init.vim 中插入安装插件需要的格式。以插件 vim-airline 为例：

```neovim
call plug#begin() 

Plug 'https://github.com/vim-airline/vim-airline'

call plug#end()
```

重新打开，并没有发现有什么不同，因为还需要在底行输入命令安装插件：

```neovim
:PlugInstall
```

结果极为顺利地安装完成了，重新打开时变得极为炫酷。

如法炮制，一连安装了 nerdtree 和 一开始想要安装的 vim-surround:

```neovim
call plug#begin() 

Plug 'https://github.com/vim-airline/vim-airline'
Plug 'https://github.com/preservim/nerdtree'
Plug 'https://github.com/tpope/vim-surround'

call plug#end()
```

现在可以比较方便地使用`surround`了。

> 后来发现 vim 插件里其实已经继承了 vim-surround 插件，其实没必要专门用 neovim; neovim 集成 vscode 还有没法实时响应的不足，因此目前来看，还是不如直接使用 vim 插件。