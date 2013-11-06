之前一直是在线看[Coursera](https://www.coursera.org/)上的课程视频。最近迫于租住的房子网速太差，加之Coursera访问经常不稳定，为了使得流畅学习的过程不被破坏，开始考虑将视频下载到本地观看。

因为之前一直没有在本地看视频的习惯，很少使用播放器，所以找个顺心的播放器就成了重中之重。经过一番折腾，最终选择了[MPlayer](http://www.mplayerhq.hu)，原因主要有如下几点。

+ 免费的。
+ MPlayer同时支持Mac和Linux。因为我公司用的是Mac，而住处用的是Linux，所以播放器能跨这两个操作系统很重要。
+ 通过命令行操作，简单快捷。这也是很吸引我的一点，我实在不习惯为了播放一个视频用鼠标在文件系统中一层一层的点啊点，而MPlayer只要一行命令就可以播放指定视频，并且可以方便的指定播放参数。播放过程中也是全快捷键操作，效率很高。
+ 能自动加载srt字幕。英文课程还是需要字幕支持的。
+ 可以快放并且保持语调不变。这点对我也很重要，我看视频一般是用1.5倍速播放，Coursera的html5播放器可以完美支持快放且保持音调不变，而我惊喜的发现MPlayer也只要通过简单的启动参数就可以实现相同的效果。

下面和大家分享一下用MPlayer看Coursera视频的心得。

# 安装
MPlayer存在于大多数Linux发行版和Mac的软件源里，因此可以很方便的安装。

Mac下建议用[Homebrew](http://brew.sh/)安装：

<pre class="prettyprint linenums">
brew install mplayer
</pre>

注意，如果你和我一样用的是OSX 10.9 Mavericks，那么直接安装会失败。这是因为brew的mplayer安装脚本不兼容10.9，这块我折腾了好久。最后在github上找到了[相关的补丁](https://github.com/i8degrees/homebrew/commit/d0ba78cf321bb7fa005284377e50e98d57bf13a7)。这个补丁目前还在brew的hotfix分支下，没有合并到master，我们可以手工应用补丁，将这个补丁文件覆盖掉我们本地brew下的mplayer.rb文件，再执行上面的命令就可以正常安装了。

Linux下可以用相应发行版的软件源安装。例如Ubuntu或Linux Mint（我使用的发行版），可以通过apt安装：

<pre class="prettyprint linenums">
sudo apt-get install mplayer
</pre>

# 使用
## 打开文件
安装完成后，直接在命令行用：

<pre class="prettyprint linenums">
mplayer 视频文件
</pre>

就可以打开相应的文件进行播放了，具体效果见下图：

<p class="picture"><img alt="" src="/uploads/pictures/using-mplayer-for-coursera/01.png"/></p>

## 基本操作
MPlayer的播放界面只有一个画面框，所有操作都是通过键盘完成。基本操作如下：

+ 左右箭头 - 快退或快进10秒
+ 上下箭头 - 快退或快进1分钟
+ PageUp和PageDown - 快进或快退10分钟
+ p或空格键 - 暂停/继续
+ q或ESC键 - 退出播放
+ /和\* - 减小或增大音量

其它还有一些常用操作可以通过以下命令查看：

<pre class="prettyprint linenums">
mplayer -h
</pre>

## 挂载字幕
如果有和视频文件同名的srt文件，MPlayer会自动挂载字幕，也可以通过-sub来指定字幕：

<pre class="prettyprint linenums">
mplayer -sub 字幕文件 视频文件
</pre>

## 加速播放
大多数课程视频讲解稍显啰嗦，因此可以通过加速播放节省学习时间。MPlayer通过-speed选项改变播放速度，例如：

<pre class="prettyprint linenums">
mplayer -speed 1.5 path/to/video/file
</pre>

可以将播放速度调整为原始的1.5倍。

## 保持正常音调
用上面的方法加速播放后，声音也会加速，频率变快，因此音调非常尖锐刺耳，这个问题可以通过ScaleTempo插件解决。这个插件默认包含在MPlayer中。只要通过下面命令：

<pre class="prettyprint linenums">
mplayer -af scaletempo -speed 1.5 path/to/video/file
</pre>

就可以加速播放的同时保持语调不变了。
