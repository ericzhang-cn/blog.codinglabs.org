2012年3月，我自购了一台13寸的Macbook Air，从那时开始至今近两年时间，我一直用它作为工作本。但是最近越来越觉得4G的内存和128G的SSD力不从心，苦于Air无法升级硬件，于是终于下决心拿出入职时公司给配的Dell E6410，自己买了内存和SSD，升级成了8G内存+370G混合硬盘（120G SSD做主盘，250G硬盘做从盘）。

硬件升级事小，关键是系统的迁移代价比较大。我在Dell本上装的是Ubuntu 13.10，由于我平时习惯使用Dropbox等云端服务，浏览器配置也都通过Google账号漫游，所以这部分迁移几乎没有成本，主要的成本在开发环境配置和常用软件迁移。虽然都是Unix系，但是Mac OSX下很多软件Linux下并没有。

花了大约一个周末，总算把我的Ubuntu配置的比较顺手了，目前也已经正式投入工作。其中的重头戏便是开发环境（主要是terminal和vim）的配置，另外就是一些常用工具。这篇文章记录了我一些主要的工作，算是给自己留一个文档，也希望能给打算从Mac迁移到Linux的同学做一个借鉴。

开发环境配置
============
之前在Mac下，我直接使用的是[maximum-awesome](https://github.com/square/maximum-awesome)，开发环境这块完全不用自己操心。可惜maximum-awesome只能在Mac下使用，并没有Linux版。于是需要自己做一些工作，以便让开发环境够顺手。

使用terminator作为终端
----------------------

### 安装terminator
Ubuntu自带的终端是gnome-terminal，虽然也还不错，但是不能支持屏幕分割、选择复制等功能让我很不爽，于是我换用terminator作为终端，terminator可以支持屏幕分割，并且默认快捷键和gnome-terminal无异，熟悉gnome-terminal的话可以快速上手。

Ubuntu下可以这样安装terminator：

```bash
sudo apt-get install terminator
```

### terminator常用快捷键

+ Ctrl-Shift-c 拷贝
+ Ctrl-Shift-v 粘贴
+ Ctrl-Shift-t 开新Tab窗口
+ Ctrl-Shift-o 上下拆分屏幕
+ Ctrl-Shift-e 左右拆分屏幕
+ Ctrl-Shift-w 关闭当前窗口
+ Ctrl-Shift-q 关闭整个终端

配置terminator使用solarized配色
-------------------------------

### 使用terminator-solarized
maximum-awesome所使用的[solarized](http://ethanschoonover.com/solarized)配色是相当不错的，所以自然希望继续使用。针对terminator的solarized配色已经有人专门做好了：[terminator-solarized](https://github.com/ghuntley/terminator-solarized)，只要按如下操作就可以使用：

```bash
mkdir -p ~/.config/terminator/
curl https://raw.github.com/ghuntley/terminator-solarized/master/config > ~/.config/terminator/config
```

然后重新打开terminator就已经是solarized配色了。

### 对terminator更多的配置
接下来，可以在terminator-solarized配置文件的基础上进行更多的配置，例如背景透明、启用选择复制等。

关于terminator的详细配置选项可以参考[terminator manpage](http://manpages.ubuntu.com/manpages/intrepid/man5/terminator_config.5.html)，下面贴出我的~/.config/terminator/config供参考：

```bash
[global_config]
	title_transmit_bg_color = "#d30102"
	focus = system
	suppress_multiple_term_dialog = True
[keybindings]
[profiles]
	[[default]]
		palette = "#073642:#dc322f:#859900:#b58900:#268bd2:#d33682:#2aa198:#eee8d5:#002b36:#cb4b16:#586e75:#657b83:#839496:#6c71c4:#93a1a1:#fdf6e3"
		copy_on_selection = True
		background_image = None
		background_darkness = 0.95
		background_type = transparent
		use_system_font = False
		cursor_color = "#eee8d5"
		foreground_color = "#839496"
		show_titlebar = False
		font = Monospace 11
		background_color = "#002b36"
	[[solarized-dark]]
		palette = "#073642:#dc322f:#859900:#b58900:#268bd2:#d33682:#2aa198:#eee8d5:#002b36:#cb4b16:#586e75:#657b83:#839496:#6c71c4:#93a1a1:#fdf6e3"
		background_color = "#002b36"
		background_image = None
		cursor_color = "#eee8d5"
		foreground_color = "#839496"
	[[solarized-light]]
		palette = "#073642:#dc322f:#859900:#b58900:#268bd2:#d33682:#2aa198:#eee8d5:#002b36:#cb4b16:#586e75:#657b83:#839496:#6c71c4:#93a1a1:#fdf6e3"
		background_color = "#fdf6e3"
		background_image = None
		cursor_color = "#002b36"
		foreground_color = "#657b83"
[layouts]
	[[default]]
		[[[child1]]]
			type = Terminal
			parent = window0
			profile = default
		[[[window0]]]
			type = Window
			parent = ""
[plugins]
```

### 配置dircolors
完成上述配置后，你会发现用ls命令查看目录和文件时是一片灰色。这是因为默认情况下solarized各种bright方案基本都是灰色，而系统默认显示目录和文件时多用bright色，此时需要配置dircolors才能显示出彩色的文件和目录。

[dircolors-solarized](https://github.com/seebi/dircolors-solarized)项目提供了适合于solarized的dircolors配色方案，只要选择合适的方案使用就可以了。例如我是用的solarized dark配色，所以可以选择适合这个配色的dircolors.ansi-dark：

```bash
curl https://raw.github.com/seebi/dircolors-solarized/master/dircolors.ansi-dark > ~/.dircolors
```

然后在~/.bashrc中加入如下配置：

```bash
# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
```

执行

```bash
source ~/.bashrc
```

后，再执行ls或ll就可以看到彩色的目录或文件了。

配置完的terminator效果如下：

<p class="picture"><img alt="" src="/uploads/pictures/getting-started-with-ubuntu/01.png"/></p>

配置VIM
-------
vim作为我日常使用频率最高的代码编辑器，自然要好好配置一番。之前的maximum-awesome自带了很多vim插件，这里我没有精力完全按照maximum-awesome的插件配置，只选取了日常比较常用的几个插件先配上，后面需要的话可以再加。

### 插件
我目前安装的插件有：

+ [NERDTree](https://github.com/scrooloose/nerdtree)：可以在单独的window中浏览目录和文件，方便打开的选取文件。
+ [taglist](https://github.com/vim-scripts/taglist.vim)：可以通过ctags生成的tag文件索引定位代码中的常量、函数、类等结构，阅读代码和写代码必备。
+ [powerline](https://github.com/Lokaltog/vim-powerline)：在底部显示一个非常漂亮的状态条，还可以通过不同的颜色提醒用户当前处于什么状态（如normal、insert或visual）。
+ [vim-colors-solarized](https://github.com/altercation/vim-colors-solarized)：vim的solarized配色插件。

如果所有插件都按vim标准方法安装，各种插件会非常分散，不便于管理，于是我选用[pathogen](https://github.com/tpope/vim-pathogen)安装和管理vim插件。pathogen允许将各个插件放在.vim/bundle/下各自的目录中，通过启动时自动加载所有插件。

### 自动配置工具
整个配置过程过于繁琐不再赘述，我已经将配置过程写成了一个自动配置脚本并放到了github：[https://github.com/ericzhang-cn/vim-conf](https://github.com/ericzhang-cn/vim-conf)，需要的朋友只要clone下来并运行init.sh脚本就可以自动完成整个配置：

```bash
git clone https://github.com/ericzhang-cn/vim-conf.git
cd vim-conf && ./init.sh
```

最终配置效果如下：

<p class="picture"><img alt="" src="/uploads/pictures/getting-started-with-ubuntu/02.png"/></p>

（更新：当前我已经换用[maximum-awesome-linux](https://github.com/ericzhang-cn/maximum-awesome-linux)，不再维护之前那个配置脚本）

### 快捷键
其中并没有对vim默认的快捷键做过多重设，只有两个：

+ ,-d：打开或关闭NERDTree
+ ,-t：打开或关闭taglist

（更新：换用maximum-awesome-linux后快捷键会不一样，具体请参考[README](https://github.com/ericzhang-cn/maximum-awesome-linux/blob/master/README.md)）

常用工具
========

浏览器
------
Ubuntu自带的是Firefox，我平常使用的是Chromium，这点在Ubuntu下没任何问题，可以直接安装：

```bash
sudo apt-get install chromium-browser
```

用Google账号登录后，书签、插件等会自动同步，非常方便。

搜狗输入法&谷歌输入法
---------------------
Mac下有搜狗输入法或百度输入法。不过目前搜狗也基于fcitx做了linux版的搜狗输入法。

Ubuntu自带的ibus直接卸掉，然后安装[fcitx](https://fcitx-im.org/wiki/Fcitx)：

```bash
sudo add-apt-repository ppa:fcitx-team/nightly && sudo apt-get update
sudo apt-get install fcitx-sogoupinyin
```

当然谷歌拼音也不错：

```bash
sudo apt-get install fcitx-googlepinyin
```

邮件
----
Mac下是使用Foxmail for Mac，linux下可以选择ThunderBird，用起来很顺手。

Dropbox
-------
Dropbox有官方linux客户端，可以到其官网下载安装。不过安装后也许会发现dropbox的icon没有出现在上面的indicator上，可以这样修复：

```bash
sudo apt-get install libappindicator1
dropbox stop && dropbox start
```

Evernote
--------
很不幸，我平常用来做笔记的evernote没有linux官方客户端，不过可以选择使用web版，或者安装第三方linux客户端everpad：

```bash
sudo add-apt-repository ppa:nvbn-rm/ppa
sudo apt-get update
sudo apt-get install everpad
```

当然功能和美观程度都没法和Mac下官方的客户端相比，不过日常使用还是足够了。

办公Office
----------
Ubuntu自带的LibreOffice可以很好的满足需求，试用了WPS for linux，无法正常打开Office 2010的文件，故而弃用之。LibreOffice打开Office 2010的文件没有问题。

PDF及论文管理
-------------
平常使用的[Mendeley](http://www.mendeley.com/)有官方linux客户端，可以到官网下载。

绘图及图像处理
--------------
平常工程文档做图一般用[yEd](http://www.yworks.com/en/products_yed_about.html)，是java开发的，所以在linux下可以直接使用。

图像处理可以用gimp：

```bash
sudo apt-get install gimp
```

影音播放
--------
平常很少离线看视频，如果需要的话，vlc应该够了：

```bash
sudo apt-get install vlc
```

截图工具
--------
截图工具非shutter莫属：

```bash
sudo apt-get install shutter
```

阿里旺旺
--------
因为工作关系，平常必须使用旺旺交流。Mac下有非常好用的官方版，linux下并没有官方旺旺，有个内部版本巨烂无比。不过之前参加活动获赠一个[CrossOver](http://www.codeweavers.com/products/)正版授权序列号。

目前CrossOver运行阿里旺旺2013非常流畅。

QQ
---
这个对我来说不是刚需。CrossOver可以运行TM2013，另外WebQQ或开VirtualBox在虚拟机中运行QQ都可以。

专业软件
--------
其它常用软件特别是如R或Octave等专业软件，本身就有linux版，所以可以按需安装。如Git等开发工具本来就是linux下的软件，当然更不在话下。

目前已经用Ubuntu工作了一段时间，总体来说从Mac转过来肯定需要一点适应，不过目前感觉没有遇到特别大的问题，毕竟同属Unix系，对码农来说大多数使用习惯几乎是无缝切换。

另外，重新回到OSS的感觉不错！最后上张图吧：

<p class="picture"><img alt="" src="/uploads/pictures/getting-started-with-ubuntu/03.png"/></p>
