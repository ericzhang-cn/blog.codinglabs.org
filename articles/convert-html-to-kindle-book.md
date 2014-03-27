自从买了kindle后，总是想着如何最大效用发挥其效用。虽然多看上有很多书可以购买，网上也有很多免费的电子书，但是仍然有很多感兴趣的内容是以网页的形式存在的。例如[O’Reilly Atlas](http://chimera.labs.oreilly.com/books/)就提供了诸多电子书，但是只提供免费的在线阅读；另外还有很多资料或文档都只有网页形式。于是就希望通过某种方法讲这些在线资料转为epub或mobi格式，以便在kindle上阅读。这篇文章介绍了如何借助calibre并编写少量代码来达到这个目的。

# Calibre
## Calibre简介
[Calibre](http://calibre-ebook.com/)是一个免费的电子书管理工具，可以兼容Windows, OS X及Linux，令人欣喜的是，除了GUI外，calibre还提供了诸多命令行工具，其中的ebook-convert命令可以根据用户编写的recipes文件（实际是python代码）抓取指定页面内容并生成mobi等格式的电子书。通过编写recipes可以自定制抓取行为，以适应不同的网页结构。

## 安装Calibre
Calibre的下载地址是[http://calibre-ebook.com/download](http://calibre-ebook.com/download)，可以根据自己的操作系统下载相应的安装程序。

如果是Linux操作系统，还可以通过软件仓库安装：

Archlinux：

```bash
pacman -S calibre
```

Debian/Ubuntu：

```bash
apt-get install calibre
```

RedHat/Fedora/CentOS：

```bash
yum -y install calibre
```

注意，如果你使用OSX，需要单独安装[Command Line Tool](http://manual.calibre-ebook.com/cli/cli-index.html)。

# 抓取网页生成电子书
