---
title: Deepin应用管理
description: 
published: true
date: 2022-10-25T01:49:18.623Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:32:34.710Z
---

# ⚙️网络应用
<details>
<summary><font color="#0000FF">展开查看</font></summary>

# 应用快捷{.tabset}
## 🪟浏览器
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://www.firefox.org/favicon.ico">Firefox
- [<img height="16" width="16" align="center"  src="http://www.firefox.org/favicon.ico">Firefox*火狐浏览器是一个安全高效的浏览器*](http://www.firefox.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
火狐浏览器是一个安全高效的浏览器，它具有速度快、隐私保护、丰富的插件资源、不同设备之间同步数据、分页浏览、个性化定制等特性。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install firefox-dde(深度桌面版)
```
```
sudo apt-get install firefox       
```
```
sudo apt-get install firefox-esr(延长支持版)
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove firefox-dde(深度桌面版)
```
```
sudo apt-get remove firefox      
```
```
sudo apt-get remove firefox-esr(延长支持版)
```
**<font color="#0000FF">仓库地址：</font>**
[深度桌面版](https://packages.deepin.com/deepin/pool/main/f/firefox-dde/) (深度桌面版)
[未知版本](https://packages.deepin.com/deepin/pool/main/f/firefox/)
[延长支持版](https://packages.deepin.com/deepin/pool/main/f/firefox-esr)(延长支持版)

**<font color="#0000FF">常见问题</font>**
**<font color="#1E90FF">一、如何安装Flash插件</font>**
```
sudo apt-get install libflashplugin
```
**<font color="#1E90FF">二、如何安装中文语言包</font>**
```
sudo apt-get install firefox-l10n-zh-cn
```
**<font color="#1E90FF">三、手动更新flashplayer插件</font>**
参见：[点击查看](https://bbs.deepin.org/forum.php?mod=viewthread&tid=143255&extra=)

**<font color="#1E90FF">四、更新flashplayer插件</font>**
下载地址：[点击下载](https://get2.adobe.com/cn/flashplayer/otherversions/ )
```
flash_player_npapi_linux.x86_64.tar.gz 
```
**<font color="#1E90FF">tar.gz 插件包安装说明:</font>**
解压 tar.gz 插件包到合适的本地文件夹，你会看到：
```
/libflashplayer.so 
/usr/ 
```
在终端中进入上述本地文件夹，使用下列命令
```
sudo cp libflashplayer.so /usr/lib/mozilla/plugin
sudo cp -r usr/* /usr
```

**<font color="#0000FF">五、Firefox 使用PPAPI版 flashplayer</font>**
https://get2.adobe.com/cn/flashplayer/otherversions/  下载   flash_player_ppapi_linux.x86_64.tar.gz 

**<font color="#1E90FF">tar.gz 插件包安装说明:</font>**
解压 tar.gz 插件包到合适的本地文件夹，你会看到：
```
/libpepflashplayer.so
```
在终端中进入上述本地文件夹，使用下列命令
```
sudo mkdir /usr/lib/adobe-flashplugin
sudo cp libpepflashplayer.so /usr/lib/adobe-flashplugin
```

推荐 freshplayerplugin -- 使 linux 下 firefox 能够使用 ppapi flash
https://www.v2ex.com/t/153629

因为mozilla 没打算支持ppapi的flash，于是有牛人做了一个ppapi to npapi 的转换器，于是firefox就可以在linux下直接使用最新版的flash啦！

项目地址 https://github.com/i-rinat/freshplayerplugin

Firefox所有扩展突然被禁用
老版的firefox由于 AMO（Firefox 扩展中心）中间签名证书过期，可能导致 Firefox 认为这些扩展是未签名的，所以会被禁用.更新到最新版本就能恢复.

**<font color="#0000FF">相关链接</font>**
1.[【已修复】Firefox所有扩展突然被禁用](https://mozilla.com.cn/thread-413298-1-1.html)
2.[火狐官网](https://www.firefox.com.cn/)
3.[火狐官方论坛](http://mozilla.com.cn/forum.php)


#### <img height="16" width="16" align="center"  src="https://www.google.com/favicon.ico">Chrome
- [<img height="16" width="16" align="center"  src="https://www.google.com/favicon.ico">Chrome*谷歌浏览器是一个由Google公司开发的网页浏览器*](https://www.google.com/chrome)
{.links-list}

**<font color="#0000FF">简介</font>**
谷歌浏览器是一个由Google公司开发的网页浏览器，具有稳定、快速、安全、简洁、插件扩展等特点，其中包括稳定版(Stable)、开发版(Dev)、测试版(Beta)以及其他版本。Stable主要是为追求稳定的普通用户使用，一般更新最慢。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install chrome-dde（stable）
sudo apt-get install google-chrome-beta（beta）
sudo apt-get install google-chrome-unstable（unstable）
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove chrome-dde
sudo apt-get remove google-chrome-beta（beta）
sudo apt-get remove google-chrome-unstable（unstable）
```
**<font color="#0000FF">仓库地址：</font>**
[http://packages.deepin.com/deepin/pool/main/c/chrome-dde/](http://packages.deepin.com/deepin/pool/main/c/chrome-dde/)
[http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-beta](http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-beta)
[http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-unstable](http://packages.deepin.com/deepin/pool/non-free/g/google-chrome-unstable)

**<font color="#0000FF">常见问题</font>**
```
sudo apt-get install libflashplugin-pepper
```

参见：https://bbs.deepin.org/forum.php?mod=viewthread&tid=143255&extra=

**<font color="#1E90FF">如何去掉密钥环提示</font>**

密钥环是linux系统用于安全保存程序私密数据的模块，可以用于加密保存密码、证书、密钥等安全数据。chrome的密钥环用于保存本地访问站点密码或缓存从google服务器同步下来的访问站点的密码。
Deepin系统的chrome会默认会把密码放在登录密钥环里，之所以会提示解锁登录密钥环是因为你的登陆密钥环被锁定了，只要把你的登陆密钥环解锁就可以了。

**<font color="#1E90FF"> 安装seahorse</font>**
```
sudo apt-get install seahorse
```
**<font color="#1E90FF"> 运行seahorse</font>**
```
seahorse
```
解锁密码或修改密码环密码
密码->登录(Login)->右键解锁

另外一种情况
如果是chrome的安全数据没有存放于登录密钥环，那么有可能是创建了一个新叫“默认密钥环”的密钥环来存储。如想不每次打开电脑都输入的话就应该把它的密码设为空或者在登录密钥环上创建一个项目指向“默认密钥环”。

密码->默认密钥环->修改密码

**<font color="#0000FF">相关链接</font>**

#### <img height="16" width="16" align="center"  src="https://www.maxthon.cn/favicon.ico">傲游云浏览器
- [<img height="16" width="16" align="center"  src="https://www.maxthon.cn/favicon.ico">傲游云浏览器*傲游云浏览器是一款基于Chromium开发的云浏览器*](http://www.maxthon.cn/)
{.links-list}

**<font color="#0000FF">简介</font>**
傲游云浏览器是一款基于Chromium开发的云浏览器，它集成了傲游账户、收藏和智能填表同步、快速访问、鼠标手势、超级拖拽、自动更新等功能，同时具有数据同步、插件系统功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install maxthon-browser-stable
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove maxthon-browser-stable
```
**<font color="#0000FF">仓库地址：</font>**
[http://packages.deepin.com/deepin/pool/non-free/m/maxthon-browser-stable/](http://packages.deepin.com/deepin/pool/non-free/m/maxthon-browser-stable/)

**<font color="#0000FF">常见问题</font>**

**<font color="#0000FF">相关链接</font>**


#### <img height="16" width="16" align="center"  src="https://www.opera.com/favicon.ico">Opera
- [<img height="16" width="16" align="center"  src="https://www.opera.com/favicon.ico">Opera*是一款网络浏览器*](http://www.opera.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Opera是一款网络浏览器，它具有网络同步，密码管理、会话管理、鼠标手势、键盘快捷键、内置搜索引擎、智能弹出式广告拦截、网址的过滤和主题皮肤、插件扩展等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install opera-stable (稳定版)
sudo apt-get install opera-developer (开发者版本)
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove opera-stable (稳定版)
sudo apt-get remove opera-developer (开发者版本)
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/o/opera-stable/
http://packages.deepin.com/deepin/pool/non-free/o/opera-developer/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://astian.org/wp-content/uploads/2021/12/Asset-1-1.png">Midori
- [<img height="16" width="16" align="center"  src="https://astian.org/wp-content/uploads/2021/12/Asset-1-1.png">Midori*是一个轻量级的网页浏览器*](http://www.midori-browser.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
Midori是一个轻量级的网页浏览器，它全面整合GTK+2和GTK+3，还使用和Safari一样的WebKit引擎，具有分页浏览、会话管理、书签收藏、搜索、用户脚本和样式支持、扩展等功能。

**<font color="#0000FF">安装</font>**
sudo apt-get install midori

**<font color="#0000FF">卸载</font>**
sudo apt-get remove midori

**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/m/midori/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：midori</font>**

#### <img height="16" width="16" align="center"  src="https://vivaldi.com/favicon.ico">Vivaldi
- [<img height="16" width="16" align="center"  src="https://vivaldi.com/favicon.ico">Vivaldi*是一款极速浏览器*](https://vivaldi.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Vivaldi是一款极速浏览器，支持英语、日语、法语、俄语等近10种语言。Vivaldi浏览器的布局与Opera浏览器非常相似，拥有不少Opera的功能，例如快速拨号界面、滑鼠手势等。另外，Vivaldi允许用户在屏幕截图上添加笔记。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install vivaldi-stable
sudo apt-get install vivaldi-beta
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove vivaldi-stable
sudo apt-get remove vivaldi-beta
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/v/vivaldi-stable/
http://packages.deepin.com/deepin/pool/main/v/vivaldi-beta/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### Yandex<img height="16" width="16" align="center"  src="https://yastatic.net/s3/home-static/_/a5/a557b72322add07a6b41fc8f71cfffc8.png">
- [<img height="16" width="16" align="center"  src="https://yastatic.net/s3/home-static/_/a5/a557b72322add07a6b41fc8f71cfffc8.png">Yandex*是一款免费的浏览器*](https://www.yandex.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Yandex是一款免费的浏览器，来自自俄罗斯最大的搜索引擎Yandex，在国外拥有众多的用户，具有界面简洁、浏览速度快、集成快速搜索、涡轮加速等功能，在网速缓慢的情况下，加快网页的加载速度。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install yandex-browser-beta
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove yandex-browser-beta
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/y/yandex-browser-beta/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## 🖥️远程桌面客户端
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.teamviewer.com/favicon.ico">TeamViewer
- [<img height="16" width="16" align="center"  src="https://www.teamviewer.com/favicon.ico">TeamViewer*是一个用于远程控制、桌面共享和文件传输的简单且快速的解决方案。*](https://www.teamviewer.com)
{.links-list}

**<font color="#0000FF">简介</font>**
TeamViewer是一个用于远程控制、桌面共享和文件传输的简单且快速的解决方案。它具有即时远程控制、远程维护、远程访问、家庭办公、在线会议/演示等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install teamviewer
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove teamviewer
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/t/teamviewer/

**<font color="#0000FF">常见问题</font>**
请尽量使用深度商店提供的TeamViewer。由于系统自带的软件包安装程序存在逻辑问题，双击从官网下载的TeamViewer的deb安装包安装将可能导致深度桌面环境被卸载。安装下载的deb包，请尽量使用命令：
```
sudo dpkg -i xxx.deb
```
并注意相应提示。

**<font color="#0000FF">相关链接</font>**

**<font color="#1E90FF">维基百科：</font>**
https://zh.wikipedia.org/wiki/TeamViewer

#### <img height="16" width="16" align="center"  src="http://www.remmina.org/favicon-16x16.png">Remmina
- [<img height="16" width="16" align="center"  src="http://www.remmina.org/favicon-16x16.png">Remmina*是一个远程桌面客户端*](http://www.remmina.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
Remmina是一个远程桌面客户端，它提供了RDP、VNC、XDMCP、SSH等远程连接协议的支持。其优点在于界面清爽，方便易用。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install remmina
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove remmina
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/r/remmina/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://ugetdm.com/wp-content/uploads/2018/01/cropped-google-plus-avatar-32x32.jpg">UGet
- [<img height="16" width="16" align="center"  src="https://ugetdm.com/wp-content/uploads/2018/01/cropped-google-plus-avatar-32x32.jpg">UGet*是一个下载管理器*](http://ugetdm.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Uget是一个下载管理器，它支持断点续传、监视剪贴板、Firefox插件、分类/批量下载，还支持从HTML中导入下载地址、命令行下载等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install uget
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove uget
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/u/uget/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接：</font>**
[UGet官网](https://ugetdm.com/)

**<font color="#1E90FF">维基百科：</font>**


#### <img height="16" width="16" align="center"  src="http://filezilla-project.org/favicon.ico">FileZilla
- [<img height="16" width="16" align="center"  src="http://filezilla-project.org/favicon.ico">FileZilla*是一个快速可靠的、跨平台的FTP、FTPS和SFTP客户端*](http://filezilla-project.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
FileZilla是一个快速可靠的、跨平台的FTP、FTPS和SFTP客户端。它具有断点续传、超时侦测、SSL加密、多国语言、多标签界面、多协议支持、远程查找文件、站点管理和传输队列管理等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install filezilla
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove filezilla
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/f/filezilla/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**
https://zh.wikipedia.org/wiki/FileZilla

#### <img height="16" width="16" align="center"  src="https://transmissionbt.com/images/gearshift.png">Transmission
- [<img height="16" width="16" align="center"  src="https://transmissionbt.com/images/gearshift.png">Transmission*是一个BitTorrent客户端软件*](https://transmissionbt.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Transmission是一个BitTorrent客户端软件，它支持速度限制、制作种子、远程控制、磁力链接、数据加密、损坏修复、数据来源交换等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install transmission
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove transmission
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/t/transmission/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接:</font>**
官方网站：https://transmissionbt.com/
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://www.qbittorrent.org/favicon.ico">qBittorrent
- [<img height="16" width="16" align="center"  src="https://www.qbittorrent.org/favicon.ico">qBittorrent*是一个轻量级BitTorrent客户端*](https://www.qbittorrent.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
qBittorrent是一个轻量级BitTorrent客户端，它支持文件上传/下载、支持DHT网络、数据交换、文件选择性下载、预览媒体文件、支持Unicode、支持代理连接、远程控制等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install qbittorrent
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove qbittorrent
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/q/qbittorrent/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="http://flareget.com/favicon.ico">FlareGet
- [<img height="16" width="16" align="center"  src="http://flareget.com/favicon.ico">FlareGet*是一个跨平台的下载管理器和加速器*](http://flareget.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
FlareGet是一个跨平台的下载管理器和加速器，它支持多个线程同时下载、多种浏览器集成、抓取网页上视频等功能，同时还支持云存储服务。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install flareget
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove flareget
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/f/flareget/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://github.com/fluidicon.png">gFTP
- [<img height="16" width="16" align="center"  src="https://github.com/fluidicon.png">gFTP*是一个FTP客户端工具*](https://www.gftp.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
gFTP是一个FTP客户端工具，它支持多个线程同时下载、断点续传、支持FTP、HTTP和SSH协议、支持FTP和HTTP代理，还可以下载整个目录、支持文件队列、缓存、拖拽等操作。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install gftp
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove gftp
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/g/gftp/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="http://www.crossftp.com/favicon.ico">CrossFTP
- [<img height="16" width="16" align="center"  src="http://www.crossftp.com/favicon.ico">CrossFTP*是一款FTP客戶端工具*](http://www.crossftp.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
CrossFTP是一款FTP客戶端工具，它支持多标签管理、Unicode/中文编码、站点管理/加密、文件远程备份、文件本地和Web搜索、自动重连、命令控制等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install crossftp
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove crossftp
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/c/crossftp/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">Xtreme Download Manager
- [Xtreme Download Manager*是一个P2P文件下载软件*](https://sourceforge.net/projects/xdman/)
{.links-list}

**<font color="#0000FF">简介</font>**
Xtreme Download Manager是一个P2P文件下载软件，它具有带宽控制系统、上传下载传输速度控制、下载管理智能化、积分系统、强力分享能力、支持DLP、支持多线程和队列管理、自动调节路由器设置等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install xdm
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove xdm
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/x/xdm/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://anydesk.com/favicon.ico">AnyDesk-无详情
- [AnyDesk*是一款远程桌面控制应用*](https://anydesk.com/)
{.links-list}

## 📧邮件客户端
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">Evolution
- [<img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">Evolution*是一款电子邮件和日程安排工具*](http://wiki.gnome.org/Apps/Evolution)
{.links-list}

**<font color="#0000FF">简介</font>**
Evolution是一款电子邮件和日程安排工具，为用户提供了一整套高效的个人和工作组信息管理方案，多年来一直深受Linux用户的好评。通过它您可以阅读和发送E-Mail，管理个人联系簿，在线创建和确认群组会议等。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install evolution
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove evolution
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/e/evolution/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="http://www.mozilla.org/favicon.ico">雷鸟邮件
- [<img height="16" width="16" align="center"  src="http://www.mozilla.org/favicon.ico">雷鸟邮件*是一个邮件客户端*](http://www.mozilla.org/zh-CN/thunderbird/)
{.links-list}

**<font color="#0000FF">简介</font>**
雷鸟邮件是一个邮件客户端，支持IMAP 、POP邮件协议以及HTML邮件格式，可以整合多个网络邮箱于一体，让您在本地随时都能接收或者发送邮件。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install thunderbird
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove thunderbird
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/t/thunderbird/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://www.nylas.com/wp-content/uploads/new_cropped-Nylas_favicon-270x270-1-150x150.png">Nylas N1
- [<img height="16" width="16" align="center"  src="https://www.nylas.com/wp-content/uploads/new_cropped-Nylas_favicon-270x270-1-150x150.png">Nylas N1*是一个开源的邮件客户端*](https://www.nylas.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Nylas N1是一个开源的邮件客户端，它支持插件框架，可以扩展创建强大的新功能，它兼容上百个邮件服务提供商，提供良好的程序外观并具有离线功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install nylas
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove nylas
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/n/nylas/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://owncloud.org/favicon.ico">ownCloud
- [<img height="16" width="16" align="center"  src="https://owncloud.org/favicon.ico">ownCloud*是一款用来创建私有云服务的工具*](https://owncloud.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
**<font color="#0000FF">安装</font>**
```
sudo apt-get install owncloud
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove owncloud
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/o/owncloud/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Geary
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Geary*是一款桌面电子邮件客户端程序*](https://wiki.gnome.org/Apps/Geary)
{.links-list}

**<font color="#0000FF">简介</font>**
Geary是一款桌面电子邮件客户端程序，它支持基本的查看和撰写、预览、回复等电子邮件基本功能，同时还支持IMAP协议，可以使用Google, Yahoo和Microsoft等其他在线邮箱服务。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install geary
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove geary
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/g/geary/
**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**
</details>

  
# 💬社交沟通
<details>
<summary><font color="#0000FF">展开查看</font></summary>

# 应用快捷{.tabset}
## <img height="16" width="16" align="center"  src="https://im.qq.com/favicon.ico">QQ
- [<img height="16" width="16" align="center"  src="https://im.qq.com/favicon.ico">QQ*是腾讯开发的一款基于Internet的即时通信软件*](https://im.qq.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
QQ是腾讯开发的一款基于Internet的即时通信软件。支持在线聊天、视频电话、点对点断点续传文件、网络硬盘等多种功能。QQ作为一种方便、高效的聊天工具，是中国目前使用最广泛的即时通信软件。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install deepin.com.qq.im
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove deepin.com.qq.im
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.qq.im

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://im.qq.com/favicon.ico">QQ轻聊
- [<img height="16" width="16" align="center"  src="https://im.qq.com/favicon.ico">QQ轻聊版*是中国主流聊天工具QQ的精简版本*](http://im.qq.com/lightqq/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://bearychat.com/favicon.ico">BearyChat
- [<img height="16" width="16" align="center"  src="https://bearychat.com/favicon.ico">BearyChat*是一款为工作场景设计的团队沟通工具*](https://bearychat.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
BearyChat是一款为工作场景设计的团队沟通工具，它支持自主创建公开或私密讨论组、历史消息全局搜索、文件存储管理、重要信息收藏、第三方服务集成等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install bearychat
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove bearychat
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/b/bearychat/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="http://www.akey.me/favicon.ico">安司密信
- [<img height="16" width="16" align="center"  src="http://www.akey.me/favicon.ico">安司密信*是一款社交应用*](http://www.akey.me/)
{.links-list}

**<font color="#0000FF">简介</font>**
安司密信是一款社交应用，支持即时信息发送以及网络通话功能，所有数据都经过安司密盾的高强度加密，以便有效抵御外部黑客和第三方组织的恶意攻击。开启安司密信安全模式后，所有文字、语音、图片、视频信息都会经过高强度加密之后发送给好友。支持绝密会话、阅后即焚、远程销毁等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install akeychat
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove akeychat
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/a/akeychat/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://telegram.org/favicon.ico">Telegram
- [<img height="16" width="16" align="center"  src="https://telegram.org/favicon.ico">Telegram*是一个聊天应用软件*](https://telegram.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://skype.gmw.cn/resource/assets/favicon/favicon.ico">Skype
- [<img height="16" width="16" align="center"  src="https://skype.gmw.cn/resource/assets/favicon/favicon.ico">Skype*是一款即时通讯软件*](http://skype.gmw.cn/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://bqq.gtimg.com/bqq/v5/images/qyqq.png">企业QQ
- [<img height="16" width="16" align="center"  src="https://bqq.gtimg.com/bqq/v5/images/qyqq.png">企业QQ*是一个面向中小企业用户的即使通信产品*](http://b.qq.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://rtx.tencent.com//rtx/images/index/qywx_plugin.png">腾讯通
- [<img height="16" width="16" align="center"  src="http://rtx.tencent.com//rtx/images/index/qywx_plugin.png">腾讯通 *是一个企业级的实时通信平台*](http://rtx.tencent.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
腾讯通是一个企业级的实时通信平台，它通过丰富的沟通方式为企业内办公、企业商务提供服务，支持文本会话、语音/视频交流、手机短信、文件传输、IP电话、网络会议、以及应用程序共享、电子白板等远程协作功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install apps.com.qq.rtxclient
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove apps.com.qq.rtxclient
```
**<font color="#0000FF">仓库地址</font>**
http://packages.deepin.com/deepin/pool/non-free/a/apps.com.qq.rtxclient/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="http://wangwang.taobao.com/favicon.ico">阿里旺旺
- [<img height="16" width="16" align="center"  src="http://wangwang.taobao.com/favicon.ico">阿里旺旺*是一款专为淘宝会员量身定做的个人交易沟通软件*](http://wangwang.taobao.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
阿里旺旺是一款专为淘宝会员量身定做的个人交易沟通软件，它能方便进行买家和卖家的实时沟通，提供文字聊天、语音聊天、视频聊天、文件传输、发送离线文件等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install apps.com.taobao.aliclient.wangwang
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove apps.com.taobao.aliclient.wangwang
```
**<font color="#0000FF">仓库地址</font>**
http://packages.deepin.com/deepin/pool/non-free/a/apps.com.taobao.aliclient.wangwang/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://alimarket.taobao.com/favicon.ico">千牛工作台
- [<img height="16" width="16" align="center"  src="https://alimarket.taobao.com/favicon.ico">千牛工作台 *是阿里巴巴官方出品的卖家一站式店铺管理工具*](https://alimarket.taobao.com/markets/qnww/pc)
{.links-list}

**<font color="#0000FF">简介</font>**
千牛工作台是阿里巴巴官方出品的卖家一站式店铺管理工具，卖家可以通过它发布经营资讯消息、商业伙伴关系，借此提升卖家的经营效率，促进彼此间的合作共赢，让卖家可以更加便捷和高效的管理店铺，让生意游刃有余。

**<font color="#0000FF">安装</font>**
sudo apt-get install apps.com.taobao.aliclient.qianniu

**<font color="#0000FF">卸载</font>**
sudo apt-get remove apps.com.taobao.aliclient.qianniu

**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/a/apps.com.taobao.aliclient.qianniu/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://www.pidgin.im/favicon/favicon.ico">Pidgin
- [<img height="16" width="16" align="center"  src="https://www.pidgin.im/favicon/favicon.ico">Pidgin*是一款即时通讯软件*](http://www.pidgin.im/)
{.links-list}

**<font color="#0000FF">简介</font>**
Pidgin是一款即时通讯软件，它允许您同时使用多个IM账号登录，包括QQ、MSN、Yahoo！、IRC等。Pidgin支持很多常见的网络功能，包括文件传输、离开状态、输入信息提示以及MSN窗口关闭提示。

**<font color="#0000FF">安装</font>**
简介
腾讯通是一个企业级的实时通信平台，它通过丰富的沟通方式为企业内办公、企业商务提供服务，支持文本会话、语音/视频交流、手机短信、文件传输、IP电话、网络会议、以及应用程序共享、电子白板等远程协作功能。

安装
```
sudo apt-get install apps.com.qq.rtxclient
```
卸载
```
sudo apt-get remove apps.com.qq.rtxclient
```
仓库地址
http://packages.deepin.com/deepin/pool/non-free/a/apps.com.qq.rtxclient/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## ![xchatlogo.png ](/图片存储/xchatlogo.png =16x16)Xchat
- [![xchatlogo.png ](/图片存储/xchatlogo.png =16x16)Xchat*是一款跨平台的IRC通讯协议软件*](http://xchat.org/)
{.links-list}

**<font color="#0000FF">简介</font>**
Xchat是一款跨平台的IRC通讯协议软件，有着良好的用户界面，具备常用的聊天功能，利用它您可以登录到任何的IRC服务器与别人交流。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install xchat
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove xchat
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/x/xchat/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://hexchat.github.io/favicon.ico">HexChat
- [<img height="16" width="16" align="center"  src="https://hexchat.github.io/favicon.ico">HexChat*是基于XChat的一款聊天工具*](https://hexchat.github.io/)
{.links-list}

**<font color="#0000FF">简介</font>**
HexChat是基于XChat的一款聊天工具，支持多种网络连接模式，可以自动连接、自动加入频道和自动生成ID，支持拼写检查和DCC文件传输等。

**<font color="#0000FF">安装</font>**
sudo apt-get install hexchat

**<font color="#0000FF">卸载</font>**
sudo apt-get remove hexchat

**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/h/hexchat/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://wac-cdn.atlassian.com/assets/img/favicons/atlassian/favicon.png">HipChat
- [<img height="16" width="16" align="center"  src="https://wac-cdn.atlassian.com/assets/img/favicons/atlassian/favicon.png">HipChat*是一款专为团队内部群聊设计的聊天工具*](http://www.hipchat.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
HipChat是一款专为团队内部群聊设计的聊天工具，您可以创建群组或一对一聊天，同时还提供视频聊天、屏幕共享等功能。HipChat还整合了团队文件管理和分享以及拖拽完成保存等相关操作。

**<font color="#0000FF">安装</font>**
sudo apt-get install hipchat

**<font color="#0000FF">卸载</font>**
sudo apt-get remove hipchat

**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/non-free/h/hipchat/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

</details>

# 🎵音乐欣赏
# 应用快捷{.tabset}
## <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度音乐
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度音乐*深度科技重新打造的一款专注于本地音乐播放的应用程序*](https://www.deepin.org/original/deepin-music/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://p3.music.126.net/tBTNafgjNnTL1KlZMt7lVA==/18885211718935735.jpg">网易云音乐
- [<img height="16" width="16" align="center"  src="http://p3.music.126.net/tBTNafgjNnTL1KlZMt7lVA==/18885211718935735.jpg">网易云音乐*是一款专注于发现与分享的音乐产品*](http://music.163.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
网易云音乐是一款专注于发现与分享的音乐产品，依托专业音乐人、DJ、好友推荐及社交功能，在线音乐服务主打歌单、社交、大牌推荐和音乐指纹，以歌单、DJ节目、社交、地理位置为核心要素，主打发现和分享。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install netease-cloud-music
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove netease-cloud-music
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/n/netease-cloud-music/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接：</font>**
[deepin官网介绍](https://www.deepin.org/cooperative/netease-cloud-music/)
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="https://www.soundnodeapp.com/wp-content/uploads/2020/06/favicon.png">Soundnode App
- [<img height="16" width="16" align="center"  src="https://www.soundnodeapp.com/wp-content/uploads/2020/06/favicon.png">Soundnode App*是一款音乐播放器应用*](http://www.soundnodeapp.com/)
{.links-list}

**<font color="#0000FF">简介</font>**
Soundnode App是一款音乐播放器应用，除了简洁的播放界面和常用的播放功能之外，还具有搜索本地新歌曲、通过各种方式播放歌曲、将歌曲并保存到你喜欢的播放列表、关注/取消关注的用户等功能。

**<font color="#0000FF">安装</font>**
```
sudo apt-get install soundnode
```
**<font color="#0000FF">卸载</font>**
```
sudo apt-get remove soundnode
```
**<font color="#0000FF">仓库地址：</font>**
http://packages.deepin.com/deepin/pool/main/s/soundnode/

**<font color="#0000FF">常见问题</font>**
**<font color="#0000FF">相关链接</font>**
**<font color="#1E90FF">维基百科：</font>**

## <img height="16" width="16" align="center"  src="http://kreogist.github.io/images/2.png">Kreogist Mu
- [<img height="16" width="16" align="center"  src="http://kreogist.github.io/images/2.png">Kreogist Mu*是一个音乐管理中心*](http://kreogist.github.io/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.clementine-player.org/favicon.ico">Clementine
- [<img height="16" width="16" align="center"  src="https://www.clementine-player.org/favicon.ico">Clementine*是一个音乐播放器和媒体库管理器*](https://www.clementine-player.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.spotify.com/favicon.ico">Spotify
- [<img height="16" width="16" align="center"  src="https://www.spotify.com/favicon.ico">Spotify*是一种专有的P2P音乐流媒体服务*](https://www.spotify.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://audacious-media-player.org/favicon.ico">Audacious
- [<img height="16" width="16" align="center"  src="http://audacious-media-player.org/favicon.ico">Audacious*是一款音乐播放器*](http://audacious-media-player.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Rhythmbox
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Rhythmbox*是一个音乐播放和管理应用*](https://wiki.gnome.org/Apps/Rhythmbox)
{.links-list}

## <img height="16" width="16" align="center"  src="https://amarok.kde.org/favicon.ico">Amarok
- [<img height="16" width="16" align="center"  src="https://amarok.kde.org/favicon.ico">Amarok*是一款音乐播放器*](https://amarok.kde.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.tomahawk-player.org/favicon.ico">Tomahawk
- [<img height="16" width="16" align="center"  src="https://www.tomahawk-player.org/favicon.ico">Tomahawk*是一款网络音乐播放器*](https://www.tomahawk-player.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://musescore.org/favicon.ico">Musescore
- [<img height="16" width="16" align="center"  src="https://musescore.org/favicon.ico">Musescore*是一套作曲写乐谱工具*](https://musescore.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://muse-sequencer.org/wp-content/uploads/2021/02/cropped-LogoMakr-0Ai7db-32x32.png">MusE
- [<img height="16" width="16" align="center"  src="https://muse-sequencer.org/wp-content/uploads/2021/02/cropped-LogoMakr-0Ai7db-32x32.png">MusE*是一个MIDI/音频的音序器*](http://muse-sequencer.org/)
{.links-list}

# 🎥视频播放
# 应用快捷{.tabset}
## 🎥视频播放一
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度影院
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度影院*是深度科技打造的一款专注于本地视频播放的应用程序*](https://www.deepin.org/original/deepin-movie/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://smplayer.org/favicon.ico">SMplayer
- [<img height="16" width="16" align="center"  src="http://smplayer.org/favicon.ico">SMplayer*是一款跨平台的视频播放工具*](http://smplayer.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.videolan.org/favicon.ico">VLC
- [<img height="16" width="16" align="center"  src="http://www.videolan.org/favicon.ico">VLC*是一款自由、开源的跨平台多媒体播放器及框架*](http://www.videolan.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.openshot.org/favicon.ico">OpenShot
- [<img height="16" width="16" align="center"  src="http://www.openshot.org/favicon.ico">OpenShot*是一个视频编辑软件*](http://www.openshot.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://bakamplayer.u8sand.net/favicon.ico">Baka MPlayer
- [<img height="16" width="16" align="center"  src="http://bakamplayer.u8sand.net/favicon.ico">Baka MPlayer*是一个多媒体播放器*](http://bakamplayer.u8sand.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.kohaupt-online.de/favicon.ico">Vokoscreen
- [<img height="16" width="16" align="center"  src="http://www.kohaupt-online.de/favicon.ico">Vokoscreen*是一款有效的强大屏幕录制工具*](http://www.kohaupt-online.de/hp/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.maartenbaert.be/favicon.ico">SimpleScreenRecorder
- [<img height="16" width="16" align="center"  src="http://www.maartenbaert.be/favicon.ico">SimpleScreenRecorder*是一款屏幕录制软件*](http://www.maartenbaert.be/simplescreenrecorder/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Kazam
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Kazam*是一款简易的桌面屏幕录制工具*](https://launchpad.net/kazam/)
{.links-list}

## 🎥视频播放二
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.shotcut.org/assets/img/favicon.ico">Shotcut
- [<img height="16" width="16" align="center"  src="https://www.shotcut.org/assets/img/favicon.ico">Shotcut*是一款跨平台的视频编辑器软件*](https://www.shotcut.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://obsproject.com/favicon.ico">Open Broadcaster Software
- [<img height="16" width="16" align="center"  src="https://obsproject.com/favicon.ico">Open Broadcaster Software*是一款视频录制和直播的应用*](https://obsproject.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.lwks.com/favicon.ico">Lightworks
- [<img height="16" width="16" align="center"  src="https://www.lwks.com/favicon.ico">Lightworks*是一款非线性视频编辑器*](https://www.lwks.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://handbrake.fr/favicon.ico">HandBrake
- [<img height="16" width="16" align="center"  src="https://handbrake.fr/favicon.ico">HandBrake*是一款视频转码工具*](https://handbrake.fr/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://otsaloma.io/gaupol/favicon.ico?v=202204030412">Gaupol
- [<img height="16" width="16" align="center"  src="https://otsaloma.io/gaupol/favicon.ico?v=202204030412">Gaupol*是一个字幕编辑软件*](http://otsaloma.io/gaupol/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://recordmydesktop.sourceforge.net/favicon.ico">recordMyDesktop
- [<img height="16" width="16" align="center"  src="http://recordmydesktop.sourceforge.net/favicon.ico">recordMyDesktop*是一个桌面视频录制软件*](http://recordmydesktop.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://arctime.cn/images/favicon.ico?crc=388733599">ArcTime
- [<img height="16" width="16" align="center"  src="http://arctime.cn/images/favicon.ico?crc=388733599">ArcTime*是一款可视化字幕编辑器*](http://arctime.cn/index.html)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://natron.inria.fr/favicon.ico">Natron
- [<img height="16" width="16" align="center"  src="https://natron.inria.fr/favicon.ico">Natron*是一个跨平台的视频合成软件*](https://natron.inria.fr/)
{.links-list}

# 🧩图形图像
# 应用快捷{.tabset}
## 🧩图形图像一
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度看图
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度看图*是深度科技精心打造的一款图片查看和管理应用*](https://www.deepin.org/original/deepin-image-viewer/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度截图
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度截图*是深度科技开发的深度操作系统下自带的截图工具*](https://www.deepin.org/original/deepin-screenshot/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.edrawsoft.cn/favicon.ico">亿图图示
- [<img height="16" width="16" align="center"  src="http://www.edrawsoft.cn/favicon.ico">亿图图示*是一款综合图形图表制作应用*](http://www.edrawsoft.cn/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://krita.org/favicon.ico">Krita
- [<img height="16" width="16" align="center"  src="https://krita.org/favicon.ico">Krita*是一个位图形编辑软件*](https://krita.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://inkscape.org/favicon.ico">Inkscape
- [<img height="16" width="16" align="center"  src="https://inkscape.org/favicon.ico">Inkscape*是一款矢量绘图软件*](https://inkscape.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.bluehost.com/favicon.ico">LightZone-网站和内容不一致
- [<img height="16" width="16" align="center"  src="https://www.bluehost.com/favicon.ico">LightZone*是一款数码图象编辑工具*](http://lightzoneproject.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.blender.org/favicon.ico">Blender
- [<img height="16" width="16" align="center"  src="https://www.blender.org/favicon.ico">Blender*是一款三维动画制作软件*](https://www.blender.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.bricsys.com/favicon.ico">BricsCAD
- [<img height="16" width="16" align="center"  src="https://www.bricsys.com/favicon.ico">BricsCAD*是一个功能强大的CAD平台*](https://www.bricsys.com)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnConvert
- [<img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnConvert*是一款多功能图像批处理工具*](http://www.xnview.com/en/xnconvert/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnView MP
- [<img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnView MP*是一款非常棒的图像查看工具*](http://www.xnview.com/en/xnviewmp/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnSketch
- [<img height="16" width="16" align="center"  src="http://www.xnview.com/favicon.ico">XnSketch*是一款图片处理应用*](http://www.xnview.com/en/xnsketch/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://shutter-project.org/favicon.ico">Shutter
- [<img height="16" width="16" align="center"  src="http://shutter-project.org/favicon.ico">Shutter*是一款广受欢迎的截屏软件*](http://shutter-project.org/)
{.links-list}

## 🧩图形图像二
### {.tabset}
#### ![mypaint.ico](/图片存储/mypaint.ico =16x16)MyPaint
- [![mypaint.ico](/图片存储/mypaint.ico =16x16)MyPaint*是一个图像绘画工具*](http://mypaint.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.polarr.co/favicon.ico">泼辣修图
- [<img height="16" width="16" align="center"  src="https://www.polarr.co/favicon.ico">泼辣修图 *（Polarr）是一个智能图片处理工具*](https://www.polarr.co/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Peek
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Peek*是一款屏幕录制工具*](https://github.com/phw/peek)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.synfig.org/favicon.ico">Synfig studio
- [<img height="16" width="16" align="center"  src="http://www.synfig.org/favicon.ico">Synfig studio*是一套功能强大的2D矢量动画制作软件*](http://www.synfig.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.rawtherapee.com/favicon.ico">RawTherapee
- [<img height="16" width="16" align="center"  src="http://www.rawtherapee.com/favicon.ico">RawTherapee*是一款RAW格式转换软件*](http://www.rawtherapee.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">图像查看器
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">图像查看器 *（EyeOfGnome）是一款图像查看器软件*](https://wiki.gnome.org/Apps/EyeOfGnome)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">HotShots
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">HotShots*是一款易于使用的屏幕捕获工具*](https://sourceforge.net/projects/hotshots/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Gwenview
- [<img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Gwenview*是一个图片浏览和管理软件*](https://www.kde.org/applications/graphics/gwenview/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.digikam.org/favicon.ico">digiKam
- [<img height="16" width="16" align="center"  src="https://www.digikam.org/favicon.ico">digiKam*是一款跨平台的数字照片管理软件*](https://www.digikam.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Shotwell
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Shotwell*是一款轻量级的图片管理软件*](https://wiki.gnome.org/Apps/Shotwell)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.yworks.com/favicon.ico">yEd Graph Editor
- [<img height="16" width="16" align="center"  src="http://www.yworks.com/favicon.ico">yEd Graph Editor*是一款流程图绘制工具*](http://www.yworks.com/)
{.links-list}

# 🎮娱乐游戏
# 应用快捷{.tabset}
## <img height="16" width="16" align="center"  src="http://store.steampowered.com/favicon.ico">Steam
- [<img height="16" width="16" align="center"  src="http://store.steampowered.com/favicon.ico">Steam*是一款方便迅速的综合性游戏平台*](https://store.steampowered.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.warsow.net/images/warsow-logo-256x256.png">Warsow
- [<img height="16" width="16" align="center"  src="https://www.warsow.net/images/warsow-logo-256x256.png">Warsow*是一款第一人称射击游戏*](https://www.warsow.net/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://www.desura.com/favicon.ico">Desura
- [<img height="16" width="16" align="center"  src="http://www.desura.com/favicon.ico">Desura*是一个正版游戏网站*](http://www.desura.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://www.snes9x.com/favicon.ico">Snes9x
- [<img height="16" width="16" align="center"  src="http://www.snes9x.com/favicon.ico">Snes9x*是一款超级任天堂SFC模拟器*](http://www.snes9x.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://jushiip.itch.io/favicon.ico">Flail Rider
- [<img height="16" width="16" align="center"  src="http://jushiip.itch.io/favicon.ico">Flail Rider*是一款快节奏的桌面游戏*](http://jushiip.itch.io/flail-rider)
{.links-list}

## <img height="16" width="16" align="center"  src="http://mamedev.org/favicon.ico">MAME
- [<img height="16" width="16" align="center"  src="http://mamedev.org/favicon.ico">MAME*是一个模拟器，也是国内玩家最熟悉和最常使用的街机模拟器之一*](http://mamedev.org/)
{.links-list}

## ![supertuxproject.png](/图片存储/supertuxproject.png =16x16)SuperTux
- [![supertuxproject.png](/图片存储/supertuxproject.png =16x16)SuperTux*是一款类似超级马里奥兄弟的游戏*](http://supertuxproject.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://assault.cubers.net/docs/images/favicon.ico">AssaultCube
- [<img height="16" width="16" align="center"  src="http://assault.cubers.net/docs/images/favicon.ico">AssaultCube*是一款第一视角射击游戏*](http://assault.cubers.net/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://www.ppsspp.org/favicon.ico">PPSSPP
- [<img height="16" width="16" align="center"  src="http://www.ppsspp.org/favicon.ico">PPSSPP*是一款非常出色的PSP模拟器*](http://www.ppsspp.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://stuntrally.tuxfamily.org/uploads/images/favicon.png">Stunt Rally
- [<img height="16" width="16" align="center"  src="http://stuntrally.tuxfamily.org/uploads/images/favicon.png">Stunt Rally*是一款赛车游戏*](http://stuntrally.tuxfamily.org/)
{.links-list}

# 💼办公学习
# 应用快捷{.tabset}
## 💼办公学习一
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://static.epy.wpscdn.cn/favicon.ico">WPS Office
- [<img height="16" width="16" align="center"  src="https://static.epy.wpscdn.cn/favicon.ico">WPS Office*是由金山软件股份有限公司自主研发的一款办公软件套件*](http://linux.wps.cn/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.libreoffice.org/favicon.ico">LibreOffice
- [<img height="16" width="16" align="center"  src="https://www.libreoffice.org/favicon.ico">LibreOffice*是一款功能强大的办公套件*](https://www.libreoffice.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.yozosoft.com/favicon.ico">永中Office
- [<img height="16" width="16" align="center"  src="http://www.yozosoft.com/favicon.ico">永中Office*是一款功能强大的办公软件*](http://www.yozosoft.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度云打印
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度云打印*是由深度科技开发的一种新型打印解决方案*](https://www.deepin.org/original/deepin-cloud-print/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度云扫描
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度云扫描*是由深度科技开发的一种新型扫描技术*](https://www.deepin.org/original/deepin-cloud-scan/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.xmind.net/favicon.ico">XMind
- [<img height="16" width="16" align="center"  src="http://www.xmind.net/favicon.ico">XMind*是一款全球领先的思维导图软件*](http://www.xmind.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.wiz.cn/favicon.ico">为知笔记
- [<img height="16" width="16" align="center"  src="http://www.wiz.cn/favicon.ico">为知笔记*为知笔记是一款云服务笔记软件*](http://www.wiz.cn/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Simplenote
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Simplenote*是一款纯文本笔记记录应用*](https://github.com/Automattic/simplenote-electron)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">Notepadqq
- [<img height="16" width="16" align="center"  src="/favicon.ico">Notepadqq*是一套纯文字编辑器，与Notepad++非常相似。*](http://notepadqq.altervista.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.geogebra.org/favicon.ico">GeoGebra
- [<img height="16" width="16" align="center"  src="https://www.geogebra.org/favicon.ico">GeoGebra*是一个动态数学应用*](https://www.geogebra.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://leanote.com/favicon.ico">Leanote
- [<img height="16" width="16" align="center"  src="https://leanote.com/favicon.ico">Leanote*是一款在线的云笔记应用*](https://leanote.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://remarkableapp.github.io/images/remarkable.png">Remarkable
- [<img height="16" width="16" align="center"  src="https://remarkableapp.github.io/images/remarkable.png">Remarkable*是一款Markdown编辑器*](https://remarkableapp.github.io/)
{.links-list}

## 💼办公学习二
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://www.lyx.org/favicon.ico">LyX
- [<img height="16" width="16" align="center"  src="http://www.lyx.org/favicon.ico">LyX*是一款利用LaTeX来排版的文件编辑软件*](http://www.lyx.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.insilmaril.de/vym/flags/flag-url-16x16.png">VYM
- [<img height="16" width="16" align="center"  src="https://www.insilmaril.de/vym/flags/flag-url-16x16.png">VYM*是一款思维导图软件*](http://www.insilmaril.de/vym/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://gottcode.org/favicon.ico">FocusWriter
- [<img height="16" width="16" align="center"  src="http://gottcode.org/favicon.ico">FocusWriter*是一款写作软件*](http://gottcode.org/focuswriter/)
{.links-list}

#### ![cyberelk.ico](/图片存储/cyberelk.ico =16x16)打印机
- [![cyberelk.ico](/图片存储/cyberelk.ico =16x16)打印机*是一款用于配置系统打印机服务器的工具*](http://cyberelk.net/tim/software/system-config-printer/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://code.google.com/favicon.ico">ChmSee
- [<img height="16" width="16" align="center"  src="http://code.google.com/favicon.ico">ChmSee*是一个在Linux下阅读CHM格式帮助文件的软件*](http://code.google.com/p/chmsee)
{.links-list}

#### ![gwyddion.ico](/图片存储/gwyddion.ico)Gwyddion
- [![gwyddion.ico](/图片存储/gwyddion.ico)Gwyddion*是一款模块化扫描探针显微镜数据可视化和分析工具*](http://gwyddion.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">pyRenamer
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">pyRenamer*是一款文件批量改名工具*](https://launchpad.net/pyrenamer)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.scilab.org/themes/bs4esi/favicon/favicon-16x16.png">Scilab
- [<img height="16" width="16" align="center"  src="http://www.scilab.org/themes/bs4esi/favicon/favicon-16x16.png">Scilab*是一个为工程和科学应用量身定做的数值计算软件*](http://www.scilab.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.texstudio.org/favicon.ico">TeXstudio
- [<img height="16" width="16" align="center"  src="http://www.texstudio.org/favicon.ico">TeXstudio*是一个编写LaTeX文档的集成开发环境*](http://www.texstudio.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.xm1math.net/favicon.png">Texmaker
- [<img height="16" width="16" align="center"  src="http://www.xm1math.net/favicon.png">Texmaker*是一个LaTeX的编辑环境*](http://www.xm1math.net/texmaker/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.texmacs.org/favicon.ico">GNU TeXmacs
- [<img height="16" width="16" align="center"  src="http://www.texmacs.org/favicon.ico">GNU TeXmacs*是一个优秀的科学文档排版软件*](http://www.texmacs.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">KWrite
- [<img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">KWrite*是一款程序员非常喜欢的文字编辑器*](https://www.kde.org/applications/utilities/kwrite/)
{.links-list}

## 💼办公学习三
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://smallpdf.com/favicon.ico">Smallpdf
- [<img height="16" width="16" align="center"  src="https://smallpdf.com/favicon.ico">Smallpdf*是一款在线PDF处理工具*](https://smallpdf.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.abisource.com/favicon.png">AbiWord
- [<img height="16" width="16" align="center"  src="https://www.abisource.com/favicon.png">AbiWord*是一款功能完备的高效文字处理软件*](https://www.abisource.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.ganttproject.biz/img/logo.png">GanttProject
- [<img height="16" width="16" align="center" type="image/png" src="https://www.ganttproject.biz/img/logo.png">GanttProject*是一款项目计划和管理图绘制应用*](http://www.ganttproject.biz/)
{.links-list}

#### <img height="32" width="32" align="center"  src="https://scantailor.org/wp-content/plugins/thrive-visual-editor/editor/css/images/logo_placeholder_dark.svg">Scan Tailor
- [<img height="16" width="16" align="center"  src="https://scantailor.org/wp-content/plugins/thrive-visual-editor/editor/css/images/logo_placeholder_dark.svg">Scan Tailor*是一个用于扫描件的后期处理软件*](http://scantailor.org/)
{.links-list}

#### ![gnumeric.png](/图片存储/gnumeric.png =16x16)Gnumeric
- [![gnumeric.png](/图片存储/gnumeric.png =16x16)Gnumeric*是一款电子表格处理软件*](http://www.gnumeric.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.tagspaces.org/img/favicon.png">TagSpaces
- [<img height="16" width="16" align="center"  src="https://www.tagspaces.org/img/favicon.png">TagSpaces*是一款文档管理工具*](http://tagspaces.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.qownnotes.org/favicon.png">QOwnNotes
- [<img height="16" width="16" align="center"  src="https://www.qownnotes.org/favicon.png">QOwnNotes*是一款支持Markdown和ownCloud同步的文本编辑器*](http://www.qownnotes.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">CuteMarkEd
- [<img height="16" width="16" align="center"  src="/favicon.ico">CuteMarkEd*是一款Markdown编辑器*](http://cloose.github.io/CuteMarkEd/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://moeditor.org/favicon.ico">Moeditor
- [<img height="16" width="16" align="center"  src="https://moeditor.org/favicon.ico">Moeditor*是一款markdown编辑器*](https://moeditor.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wordmarkapp.com/images/favicon-16x16.png">WordMark
- [<img height="16" width="16" align="center"  src="https://wordmarkapp.com/images/favicon-16x16.png">WordMark*是一款MarkDown编辑器*](http://wordmarkapp.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://pad.haroopress.com/assets/images/logo-small.png">Haroopad
- [<img height="16" width="16" align="center"  src="https://pad.haroopress.com/assets/images/logo-small.png">Haroopad*是一款Linux平台下的markdown编辑器*](https://pad.haroopress.com)
{.links-list}

## 💼办公学习四
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.zybuluo.com/static/img/favicon.png">Cmd Markdown
- [<img height="16" width="16" align="center"  src="https://www.zybuluo.com/static/img/favicon.png">Cmd Markdown*是一款MarkDown编辑器*](https://www.zybuluo.com/cmd/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">MarkMyWords
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">MarkMyWords*是一款markdown编辑器*](https://github.com/voldyman/MarkMyWords)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Marp
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Marp*是一款markdown编辑器*](https://github.com/yhatt/marp)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://librecad.org/favicon.ico">LibreCAD
- [<img height="16" width="16" align="center"  src="http://librecad.org/favicon.ico">LibreCAD*是一款2D的CAD绘图工具*](http://librecad.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.freecadweb.org/images/favicon.ico">FreeCAD
- [<img height="16" width="16" align="center"  src="https://www.freecadweb.org/images/favicon.ico">FreeCAD*是一款通用的三维CAD建模软件*](http://www.freecadweb.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.3ds.com/favicon.ico">DraftSight
- [<img height="16" width="16" align="center"  src="http://www.3ds.com/favicon.ico">DraftSight*是一款专业的2D CAD制图软件*](http://www.3ds.com/products-services/draftsight/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.sweethome3d.com/favicon.ico">Sweet Home
- [<img height="16" width="16" align="center"  src="http://www.sweethome3d.com/favicon.ico">Sweet Home 3D*是一款家装辅助设计软件*](http://www.sweethome3d.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.qcad.org/favicon.ico">QCad
- [<img height="16" width="16" align="center"  src="http://www.qcad.org/favicon.ico">QCad*是一款2D计算机辅助画图软件*](http://www.qcad.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.gnucash.org/favicon.ico">GnuCash
- [<img height="16" width="16" align="center"  src="http://www.gnucash.org/favicon.ico">GnuCash*是一款适用于个人或小型企业的财务软件*](http://www.gnucash.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.stellarium.org/favicon.ico">Stellarium
- [<img height="16" width="16" align="center"  src="http://www.stellarium.org/favicon.ico">Stellarium*是一款虚拟星象仪的计算机软件*](http://www.stellarium.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://qelectrotech.org/favicon.ico">QElectroTech
- [<img height="16" width="16" align="center"  src="http://qelectrotech.org/favicon.ico">QElectroTech*是一款电路图绘制软件*](http://qelectrotech.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.scribus.net/favicon.ico">Scribus
- [<img height="16" width="16" align="center"  src="https://www.scribus.net/favicon.ico">Scribus*是一款电子杂志制作软件*](https://www.scribus.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.roomarranger.com/favicon.ico">Room Arranger
- [<img height="16" width="16" align="center"  src="http://www.roomarranger.com/favicon.ico">Room Arranger*是一款实时的模拟房屋设计布局的软件*](http://www.roomarranger.com/)
{.links-list}

# 📜翻译阅读
# 应用快捷{.tabset}
## <img height="16" width="16" align="center"  src="https://www.foxitsoftware.cn/favicon.ico">福昕阅读器
- [<img height="16" width="16" align="center"  src="https://www.foxitsoftware.cn/favicon.ico">福昕阅读器 *（Foxit Reader）是一款PDF文档阅读器*](https://www.foxitsoftware.cn/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://cidian.youdao.com/favicon.ico">有道词典
- [<img height="16" width="16" align="center"  src="http://cidian.youdao.com/favicon.ico">有道词典*是中国第一个基于Linux下的互联网商业翻译软*](http://cidian.youdao.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">文档查看器
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">文档查看器*Evince（文档查看器）是一个支持多种格式的文件浏览器*](https://wiki.gnome.org/Apps/Evince)
{.links-list}

## <img height="16" width="16" align="center"  src="https://code-industry.net/wp-content/uploads/2019/01/mpe_c.png">Master PDF Editor
- [<img height="16" width="16" align="center"  src="https://code-industry.net/wp-content/uploads/2019/01/mpe_c.png">Master PDF Editor*是一款功能强大的PDF和XPS文档编辑工具*](http://code-industry.net/masterpdfeditor/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://calibre-ebook.com/favicon.ico">Calibre
- [<img height="16" width="16" align="center"  src="http://calibre-ebook.com/favicon.ico">Calibre*是一个“一站式”的电子书管理软件*](http://calibre-ebook.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://goldendict.org/icon.png">GoldenDict
- [<img height="16" width="16" align="center"  src="http://goldendict.org/icon.png">GoldenDict*是一款词典软件，支持多种词典文件格式*](http://goldendict.org/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://launchpad.net/qpdfview/favicon.ico">qpdfview
- [<img height="16" width="16" align="center"  src="https://launchpad.net/qpdfview/favicon.ico">qpdfview*是一个小巧的PDF阅读器*](https://launchpad.net/qpdfview)
{.links-list}

## <img height="16" width="16" align="center"  src="http://okular.kde.org/favicon.ico">Okular
- [<img height="16" width="16" align="center"  src="http://okular.kde.org/favicon.ico">Okular*是一个PDF文档阅读软件*](http://okular.kde.org/)
{.links-list}

## ![comix.png](/图片存储/comix.png)Comix
- [![comix.png](/图片存储/comix.png)Comix*是一款优秀的连环画电子阅读器*](http://comix.sourceforge.net/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.mendeley.com/favicon.ico">Mendeley
- [<img height="16" width="16" align="center"  src="https://www.mendeley.com/favicon.ico">Mendeley*是一款文献管理软件*](https://www.mendeley.com/)
{.links-list}

## <img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">PdfMod
- [<img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">PdfMod*是一个PDF文档编辑应用*](http://wiki.gnome.org/Apps/PdfMod)
{.links-list}

## <img height="16" width="16" align="center"  src="http://a.fsdn.com/con/img/sandiego/logo-180x180.png">PDF-Shuffler
- [<img height="16" width="16" align="center"  src="http://a.fsdn.com/con/img/sandiego/logo-180x180.png">PDF-Shuffler*是一个PDF合并及分割工具*](http://pdfshuffler.sourceforge.net/)
{.links-list}

# 🔅编程开发
# 应用快捷{.tabset}
## 🔅编程开发一
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://developer.android.com/favicon.ico">Android Studio
- [<img height="16" width="16" align="center"  src="https://developer.android.com/favicon.ico">Android Studio*是一个基于IntelliJ IDEA的Android集成开发环境*](https://developer.android.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://code.visualstudio.com/favicon.ico">Visual Studio Code
- [<img height="16" width="16" align="center"  src="https://code.visualstudio.com/favicon.ico">Visual Studio Code*是一款轻量级代码编辑器*](https://code.visualstudio.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.sublimetext.com/favicon.ico">Sublime
- [<img height="16" width="16" align="center"  src="http://www.sublimetext.com/favicon.ico">Sublime*是一个代码编辑器*](http://www.sublimetext.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.atom.io/favicon.ico">Atom
- [<img height="16" width="16" align="center"  src="http://www.atom.io/favicon.ico">Atom*是一个在线文本编辑器*](http://www.atom.io/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.genymotion.com/favicon.ico">Genymotion
- [<img height="16" width="16" align="center"  src="https://www.genymotion.com/favicon.ico">Genymotion*是一款专业的虚拟模拟器*](https://www.genymotion.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.vim.org/images/vim_shortcut.ico">GVim
- [<img height="16" width="16" align="center"  src="http://www.vim.org/images/vim_shortcut.ico">GVim*是从vi发展而来的一个文本编辑器*](http://www.vim.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.ultraedit.com/favicon.ico">UltraEdit
- [<img height="16" width="16" align="center"  src="http://www.ultraedit.com/favicon.ico">UltraEdit*是一个文本编辑器*](http://www.ultraedit.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.eclipse.org/favicon.ico">Eclipse IDE
- [<img height="16" width="16" align="center"  src="http://www.eclipse.org/favicon.ico">Eclipse IDE*是一个开放源代码的、基于Java的可扩展开发平台*](http://www.eclipse.org)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.jetbrains.com/favicon.ico">JetBrains IDE
- [<img height="16" width="16" align="center"  src="https://www.jetbrains.com/favicon.ico">JetBrains IDE*公司是最好的Java IDE的创造者*](https://www.jetbrains.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.activestate.com/favicon.ico">Komodo IDE
- [<img height="16" width="16" align="center"  src="http://www.activestate.com/favicon.ico">Komodo IDE*是一款支持多种动态编程语言的集成开发环境*](http://www.activestate.com/komodo-ide)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.navicat.com/images/Navicat_16_Premium_win_256x256.ico">Navicat
- [<img height="16" width="16" align="center"  src="https://www.navicat.com/images/Navicat_16_Premium_win_256x256.ico">Navicat*是一个的数据库管理工具*](https://www.navicat.com/products/navicat-for-mysql)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.qt.io/favicon.ico">Qt Creator
- [<img height="16" width="16" align="center"  src="https://www.qt.io/favicon.ico">Qt Creator*是跨平台的轻量级集成开发环境*](https://www.qt.io)
{.links-list}

## 🔅编程开发二
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://labs.mysql.com/common/themes/sakila/favicon.ico">MySQL Workbench
- [<img height="16" width="16" align="center"  src="https://labs.mysql.com/common/themes/sakila/favicon.ico">MySQL Workbench*是一款专为MySQL设计的ER/数据库建模工具*](https://www.mysql.com/products/workbench/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sqlitestudio.pl/img/sqlitestudio.png">SQLiteStudio
- [<img height="16" width="16" align="center"  src="https://sqlitestudio.pl/img/sqlitestudio.png">SQLiteStudio*是一款可以帮助用户管理sqlite数据库的工具*](https://sqlitestudio.pl/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://dbeaver.io/wp-content/uploads/2016/07/beaver_icon_32x32.png">DBeaver
- [<img height="16" width="16" align="center"  src="https://dbeaver.io/wp-content/uploads/2016/07/beaver_icon_32x32.png">DBeaver*是一个通用的数据库管理工具和SQL客户端*](http://dbeaver.jkiss.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.wireshark.org/favicon.ico">Wireshark
- [<img height="16" width="16" align="center"  src="https://www.wireshark.org/favicon.ico">Wireshark*是一个网络封包分析软件*](https://www.wireshark.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.codeblocks.org/favicon.ico">Code::Blocks
- [<img height="16" width="16" align="center"  src="http://www.codeblocks.org/favicon.ico">Code::Blocks*是一个开源、免费、跨平台的C++ IDE*](http://www.codeblocks.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.genuitec.com/favicon.ico">MyEclipse
- [<img height="16" width="16" align="center"  src="https://www.genuitec.com/favicon.ico">MyEclipse*是一款软件开发工具，是对EclipseIDE的扩展*](https://www.genuitec.com/products/myeclipse/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartGit
- [<img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartGit*是一个Git版本控制系统的图形化客户端程序*](http://www.syntevo.com/smartgit/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.smartsvn.com/favicon.ico">SmartSVN
- [<img height="16" width="16" align="center"  src="http://www.smartsvn.com/favicon.ico">SmartSVN*是一个图形化的SVN客户端*](http://www.smartsvn.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartCVS
- [<img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartCVS*是一款用于CVS的版本控制应用*](http://www.syntevo.com/smartcvs/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartSynchronize
- [<img height="16" width="16" align="center"  src="http://www.syntevo.com/favicon.ico">SmartSynchronize*是一款检查文件与目录比较的工具*](http://www.syntevo.com/smartsynchronize/)
{.links-list}

####  🌟GNU Emacs
- [ 🌟GNU Emacs*是一个可扩展、定制的文本编辑器*](http://gnuemacs.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">Deepin Emacs
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">Deepin Emacs*是一个可自编程和扩展的文本编辑器*](https://www.deepin.org/original/deepin-emacs/)
{.links-list}

## 🔅编程开发三
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://brackets.io/favicon.ico">Brackets
- [<img height="16" width="16" align="center"  src="http://brackets.io/favicon.ico">Brackets*是一个HTML/CSS/JavaScrip前端WEB集成开发环境*](http://brackets.io/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://codelite.org/favicon.ico">CodeLite
- [<img height="16" width="16" align="center"  src="http://codelite.org/favicon.ico">CodeLite*是一个C/C++编程语言的跨平台IDE*](http://codelite.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://bluefish.openoffice.nl/favicon.png">Bluefish
- [<img height="16" width="16" align="center"  src="http://bluefish.openoffice.nl/favicon.png">Bluefish*是一款为熟练的Web设计员和程序员而设的编辑器*](http://bluefish.openoffice.nl/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://eric-ide.python-projects.org/favicon.ico">Eric
- [<img height="16" width="16" align="center"  src="http://eric-ide.python-projects.org/favicon.ico">Eric*是一个全功能Python和Ruby的编辑器和IDE*](http://eric-ide.python-projects.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">LiteIDE
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">LiteIDE*是一款开源、跨平台的轻量级Go语言集成开发环境*](https://sourceforge.net/projects/liteide/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.monodevelop.com/favicon.ico">MonoDevelop
- [<img height="16" width="16" align="center"  src="http://www.monodevelop.com/favicon.ico">MonoDevelop*是个跨平台的集成开发环境*](http://www.monodevelop.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.geany.org/favicon.ico">Geany
- [<img height="16" width="16" align="center"  src="http://www.geany.org/favicon.ico">Geany*是一个跨平台的开源集成开发环境*](http://www.geany.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://netbeans.org/favicon.ico">NetBeans
- [<img height="16" width="16" align="center"  src="https://netbeans.org/favicon.ico">NetBeans*是一个集成开发环境*](https://netbeans.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.arduino.cc/favicon.ico">Arduino
- [<img height="16" width="16" align="center"  src="https://www.arduino.cc/favicon.ico">Arduino*是一个软硬件平台*](https://www.arduino.cc/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://glade.gnome.org/images/favicon.png">Glade
- [<img height="16" width="16" align="center"  src="https://glade.gnome.org/images/favicon.png">Glade*是一个相当不错的图形界面设计工具*](https://glade.gnome.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpadlibrarian.net/155745622/gambas-logo64.png">Gambas
- [<img height="16" width="16" align="center"  src="https://launchpadlibrarian.net/155745622/gambas-logo64.png">Gambas*是一个开发Visual Basic的集成开发环境*](http://gambas.sourceforge.net/)
{.links-list}

## 🔅编程开发四
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://1v5ymx3zt3y73fq5gy23rtnc-wpengine.netdna-ssl.com/wp-content/uploads/2021/03/android-chrome-144x144-1.png">GitKraken
- [<img height="16" width="16" align="center"  src="https://1v5ymx3zt3y73fq5gy23rtnc-wpengine.netdna-ssl.com/wp-content/uploads/2021/03/android-chrome-144x144-1.png">GitKraken*是一款Git客户端*](https://www.gitkraken.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://anjuta.org/favicon.ico">Anjuta
- [<img height="16" width="16" align="center"  src="http://anjuta.org/favicon.png">Anjuta*是一个C、C++的集成开发环境*](http://anjuta.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.scintilla.org/favicon.ico">SciTE
- [<img height="16" width="16" align="center"  src="http://www.scintilla.org/favicon.ico">SciTE*是一款免费开源的编辑器*](http://www.scintilla.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Yakuake
- [<img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Yakuake*是一款下拉式终端仿真器*](https://www.kde.org/applications/system/yakuake/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://kate-editor.org/favicon.ico">Kate
- [<img height="16" width="16" align="center"  src="http://kate-editor.org/favicon.ico">Kate*是一个开源的、先进的文本编辑器*](http://kate-editor.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.lazarus-ide.org/favicon.ico">Lazarus
- [<img height="16" width="16" align="center"  src="http://www.lazarus-ide.org/favicon.ico">Lazarus*是一个Pascal集成开发环境*](http://www.lazarus-ide.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.intel.com/etc.clientlibs/settings/wcm/designs/intel/default/resources/favicon-32x32.png">Intel XDK
- [<img height="16" width="16" align="center"  src="https://www.intel.com/etc.clientlibs/settings/wcm/designs/intel/default/resources/favicon-32x32.png">Intel XDK*是一款HTML5跨平台集成开发工具*](https://software.intel.com/en-us/intel-xdk)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">GkDebconf
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">GkDebconf*基本上是一个图形化的前端*](https://sourceforge.net/projects/gkdebconf/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Okteta
- [<img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">Okteta*是一款十六进制编辑器*](https://www.kde.org/applications/utilities/okteta/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://wxmedit.github.io/favicon.ico">wxMEdit
- [<img height="16" width="16" align="center"  src="http://wxmedit.github.io/favicon.ico">wxMEdit*是一个跨平台的文本/十六进制编辑器*](http://wxmedit.github.io/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://poedit.net/favicon.ico">Poedit
- [<img height="16" width="16" align="center"  src="http://poedit.net/favicon.ico">Poedit*是一款.Po文件编辑器*](http://poedit.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">D-Feet
- [<img height="16" width="16" align="center"  src="http://wiki.gnome.org/favicon.ico">D-Feet*是一个易于使用D-bus调试器*](http://wiki.gnome.org/action/show/Apps/DFeet)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">PuTTY
- [<img height="16" width="16" align="center"  src="/favicon.ico">PuTTY*是一套远程登陆工具*](www.putty.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">PyRoom
- [<img height="16" width="16" align="center"  src="/favicon.ico">PyRoom*是一款文本编辑器*](http://pyroom.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://racket-lang.org/favicon.ico">Racket
- [<img height="16" width="16" align="center"  src="http://racket-lang.org/favicon.ico">Racket*是一种计算机程序设计语言*](http://racket-lang.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.gnu.org/favicon.ico">GNU Octave
- [<img height="16" width="16" align="center"  src="https://www.gnu.org/favicon.ico">GNU Octave*是一种高级编程语言*](https://www.gnu.org/software/octave/)
{.links-list}

# ⚜️系统管理
# 应用快捷{.tabset}
## ⚜️系统管理一
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度文件管理器
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度文件管理器*是深度科技开发的一款功能强大、简单易用的文件管理工具*](https://www.deepin.org/original/dde-file-manager/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度终端
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度终端*是深度科技精心打造的一款终端模拟器*](https://www.deepin.org/original/deepin-terminal/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度启动盘制作工具
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">深度启动盘制作工具*是深度科技团队开发的一款系统启动盘制作工具*](https://www.deepin.org/original/deepin-boot-maker/)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">归档管理器
- [<img height="16" width="16" align="center"  src="/favicon.ico">归档管理器*是一个归档管理器*](http://fileroller.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://pinyin.sogou.com/favicon.ico">搜狗输入法
- [<img height="16" width="16" align="center"  src="http://pinyin.sogou.com/favicon.ico">搜狗输入法*是一款流行的中文输入法*](http://pinyin.sogou.com/linux/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">系统监视器
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">系统监视器 *（gnome-system-monitor）是一个开源的应用程序*](https://github.com/GNOME/gnome-system-monitor)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">字体查看器
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">字体查看器*是一个查看字体的应用程序*](https://github.com/GNOME/gnome-font-viewer)
{.links-list}

## ⚜️系统管理二
### {.tabset}
#### <img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">Deepin OpenSymbol
- [<img height="16" width="16" align="center"  src="https://www.deepin.org/favicon.ico">Deepin OpenSymbol*是深度科技团队在Wingdings系列字体内容上，重新绘制一系列符号字体*](https://www.deepin.org/original/deepin-opensymbol/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Nautilus
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Nautilus*是一款文件管理器*](https://wiki.gnome.org/Apps/Nautilus)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Boxes
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Boxes*是一个桌面虚拟化管理工具*](https://wiki.gnome.org/Apps/Boxes)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.virtualbox.org/favicon.ico">VirtualBox
- [<img height="16" width="16" align="center"  src="https://www.virtualbox.org/favicon.ico">VirtualBox*是一款开源的虚拟机软件*](https://www.virtualbox.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.vmware.com/favicon.ico">VMware Workstation
- [<img height="16" width="16" align="center"  src="http://www.vmware.com/favicon.ico">VMware Workstation*是一款桌面虚拟计算机软件*](http://www.vmware.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Disks
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Disks*是一款磁盘管理工具*](https://launchpad.net/gnome-disk-utility)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://gparted.org/images/gparted-16.png">GParted
- [<img height="16" width="16" align="center"  src="https://gparted.org/images/gparted-16.png">GParted*是一款Linux下的功能非常强大的分区工具*](http://gparted.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.bleachbit.org/sites/default/files/zen_classic_logo_0.png">BleachBit
- [<img height="16" width="16" align="center"  src="https://www.bleachbit.org/sites/default/files/zen_classic_logo_0.png">BleachBit*是一款开源的系统清理工具*](http://bleachbit.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Gnome Terminal
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Gnome Terminal*是一个终端仿真器*](https://wiki.gnome.org/Apps/Terminal)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">WinUSB
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">WinUSB*是一个创建Windows可引导U盘工具*](https://sourceforge.net/projects/winusb/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://unetbootin.github.io/favicon.ico">UNetbootin
- [<img height="16" width="16" align="center"  src="http://unetbootin.github.io/favicon.ico">UNetbootin*是一个跨平台制作启动盘的工具*](http://unetbootin.github.io/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">CPU-G
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">CPU-G*是一款CPU检测软件*](https://sourceforge.net/projects/cpug/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Hardinfo
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Hardinfo*是一个系统信息查看软件*](https://github.com/lpereira/hardinfo)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Brasero
- [<img height="16" width="16" align="center"  src="https://wiki.gnome.org/favicon.ico">Brasero*是一款CD/DVD刻录软件*](https://wiki.gnome.org/Apps/Brasero)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.kde.org/aether/media/180x180.png">K3b
- [<img height="16" width="16" align="center"  src="https://www.kde.org/aether/media/180x180.png">K3b*是一款全功能的CD/DVD烧录软件*](http://www.k3b.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">I-Nex
- [<img height="16" width="16" align="center"  src="https://sourceforge.net/favicon.ico">I-Nex*是一款系统信息查询工具*](https://sourceforge.net/projects/i-nex/)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">Psensor
- [<img height="16" width="16" align="center"  src="/favicon.ico">Psensor*是一款监控硬件温度的工具*](http://wpitchoune.net/psensor/)
{.links-list}

#### ![littlesvr.ico](/图片存储/littlesvr.ico =16x16)ISO Master
- [![littlesvr.ico](/图片存储/littlesvr.ico =16x16)ISO Master*是一个光盘镜像文件编辑器*](http://littlesvr.ca/isomaster/)
{.links-list}

## ⚜️系统管理三
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://gufw.org/img/favicon.ico">Gufw
- [<img height="16" width="16" align="center"  src="http://gufw.org/img/favicon.ico">Gufw*是一款防火墙UFW的图形化管理工具*](http://gufw.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Terminator
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Terminator*是一款跨平台的终端工具*](https://launchpad.net/terminator/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.glx-dock.org/templates/bluecloud/images/icon.ico">Cairo-Dock
- [<img height="16" width="16" align="center"  src="https://www.glx-dock.org/templates/bluecloud/images/icon.ico">Cairo-Dock*是一个Dock类软件*](http://www.glx-dock.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Gnome Pie
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Gnome Pie*是一款炫酷的程序启动器*](https://github.com/Simmesimme/Gnome-Pie)
{.links-list}

#### ![guake.png](/图片存储/guake.png =16x16)Guake
- [![guake.png](/图片存储/guake.png =16x16)Guake*是一个下拉式的终端程序*](http://guake-project.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">VNC-Client
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">VNC-Client*是一款远程连接应用*](https://launchpad.net/evnc)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://justgetflux.com/favicon.ico">f.lux
- [<img height="16" width="16" align="center"  src="https://justgetflux.com/favicon.ico">f.lux*是一款自动调整屏幕色温亮度的应用*](https://justgetflux.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Redshift
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Redshift*是一款显示器色温自动调整应用*](https://github.com/jonls/redshift)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Conky Manager
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Conky Manager*是一款简单的图形化工具*](https://launchpad.net/conky-manager)
{.links-list}

#### <img height="16" width="16" align="center"  src="/favicon.ico">Gconf Editor
- [<img height="16" width="16" align="center"  src="/favicon.ico">Gconf Editor*是一个配置编辑软件*](https://download.gnome.org/sources/gconf-editor/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Tickeys
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Tickeys*是一款打字机声效模拟软件*](https://github.com/BillBillBillBill/Tickeys-linux)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://meldmerge.org/favicon.ico">Meld
- [<img height="16" width="16" align="center"  src="http://meldmerge.org/favicon.ico">Meld*是一款可视化的文件及目录对比/合并工具*](http://meldmerge.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.scootersoftware.com/favicon.ico">Beyond Compare
- [<img height="16" width="16" align="center"  src="http://www.scootersoftware.com/favicon.ico">Beyond Compare*是一款文件/文件夹对比工具*](http://www.scootersoftware.com)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.marzocca.net/favicon.ico">Baobab
- [<img height="16" width="16" align="center"  src="http://www.marzocca.net/favicon.png
  ">Baobab*是一款分析磁盘使用情况的图形工具*](http://www.marzocca.net/linux/baobab/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.nero.com/favicon.ico">Nero
- [<img height="16" width="16" align="center"  src="http://www.nero.com/favicon.ico">Nero*是一款多媒体刻录和编辑应用*](http://www.nero.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Catfish
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Catfish*是一个文件搜索工具*](https://launchpad.net/catfish-search)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Synapse
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Synapse*是一款使用Vala语言编写的启动器软件*](https://launchpad.net/synapse-project)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://konsole.kde.org/assets/aether/media/16x16.svg">Konsole
- [<img height="16" width="16" align="center"  src="https://konsole.kde.org/assets/aether/media/16x16.svg">Konsole*是一个终端模拟器*](https://konsole.kde.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.peazip.org/favicon.ico">PeaZip
- [<img height="16" width="16" align="center"  src="http://www.peazip.org/favicon.ico">PeaZip*是一款解压缩软件*](http://www.peazip.org/)
{.links-list}

## ⚜️系统管理四
### {.tabset}
#### <img height="16" width="16" align="center"  src="http://b1.org/favicon.ico">B1 Free Archiver
- [<img height="16" width="16" align="center"  src="http://b1.org/favicon.ico">B1 Free Archiver*是一款压缩/解压缩应用*](http://b1.org/)
{.links-list}

#### ![xarchiver.ico](/图片存储/xarchiver.ico =16x16)Xarchiver
- [![xarchiver.ico](/图片存储/xarchiver.ico =16x16)Xarchiver*是一款解压/压缩应用*](http://xarchiver.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://kshutdown.sourceforge.io/kshutdown.png">KShutdown
- [<img height="16" width="16" align="center"  src="https://kshutdown.sourceforge.io/kshutdown.png">KShutdown*是一个定时关机工具*](http://kshutdown.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://keepass.info/favicon.ico">KeePass2
- [<img height="16" width="16" align="center"  src="http://keepass.info/favicon.ico">KeePass2*是一个密码管理工具*](http://keepass.info/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.keepassx.org/images/favicon.ico">KeePassX
- [<img height="16" width="16" align="center"  src="https://www.keepassx.org/images/favicon.ico">KeePassX*是一个密码管理工具*](https://www.keepassx.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://assets-global.website-files.com/5cb7274cc3cc0c1bc1771305/5f7c37eb6a292e3352372975_favicon%2032.png">Synergy
- [<img height="16" width="16" align="center"  src="https://assets-global.website-files.com/5cb7274cc3cc0c1bc1771305/5f7c37eb6a292e3352372975_favicon%2032.png">Synergy*是一款键盘鼠标共享软件*](http://synergy-project.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/fluidicon.png">GtkHash
- [<img height="16" width="16" align="center"  src="https://github.com/fluidicon.png">GtkHash*是一个用来计算消息摘要和checksum的工具*](http://gtkhash.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.fastmail.com/favicon.ico?domain=messagingengine.com">dupeGuru
- [<img height="16" width="16" align="center"  src="https://www.fastmail.com/favicon.ico?domain=messagingengine.com">dupeGuru*是一款重复文件清理应用*](https://www.hardcoded.net/dupeguru/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">ANGRYsearch
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">ANGRYsearch*是一款文件快速搜索工具*](https://github.com/DoTheEvo/ANGRYsearch)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/hluk/CopyQ/raw/master/src/images/icon.ico">CopyQ
- [<img height="16" width="16" align="center"  src="https://github.com/hluk/CopyQ/raw/master/src/images/icon.ico">CopyQ*是一款剪贴板软件，适用于大量数据的复制粘贴*](http://hluk.github.io/CopyQ/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Easystroke
- [<img height="16" width="16" align="center"  src="https://github.com/favicon.ico">Easystroke*是一个手势识别应用*](https://github.com/thjaeger/easystroke)
{.links-list}

#### <img height="16" width="16" align="center"  src="http://www.giuspen.com/favicon.ico">CherryTree
- [<img height="16" width="16" align="center"  src="http://www.giuspen.com/favicon.ico">CherryTree*是一个支持无限层级分类的笔记软件*](http://www.giuspen.com/cherrytree/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://sites.google.com/favicon.ico">FF Multi Converter
- [<img height="16" width="16" align="center"  src="https://sites.google.com/favicon.ico">FF Multi Converter*是一个多功能的格式转换工具*](https://sites.google.com/site/ffmulticonverter/)
{.links-list}

#### ![luckybackup.ico](/图片存储/luckybackup.ico)LuckyBackup
- [![luckybackup.ico](/图片存储/luckybackup.ico)LuckyBackup*是一个文件备份和同步工具*](http://luckybackup.sourceforge.net/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Referencer
- [<img height="16" width="16" align="center"  src="https://launchpad.net/favicon.ico">Referencer*是一款文献管理工具*](https://launchpad.net/referencer)
{.links-list}

#### ![media.png](/图片存储/media.png =16x16)Crossover
- [![media.png](/图片存储/media.png =16x16)Crossover*一款可以在Linux中运行Windows软件和游戏的工具*](www.codeweavers.com)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.playonlinux.com/favicon.ico">PlayOnLinux
- [<img height="16" width="16" align="center"  src="https://www.playonlinux.com/favicon.ico">PlayOnLinux*是一款Wine的前端程序*](https://www.playonlinux.com/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://www.winehq.org/favicon.ico">Wine
- [<img height="16" width="16" align="center"  src="https://www.winehq.org/favicon.ico">Wine*是一个能够在多种作系统（Linux，macOS 及 BSD 等）上运行 Windows 应用的兼容层*](https://www.winehq.org/)
{.links-list}

#### <img height="16" width="16" align="center"  src="https://birdfont.org/favicon.ico">BirdFont
- [<img height="16" width="16" align="center"  src="https://birdfont.org/favicon.ico">BirdFont*是一个免费的字体编辑器*](https://birdfont.org/)
{.links-list}


# 🔨其他应用
# 应用快捷{.tabset}
## <img height="16" width="16" align="center"  src="http://boinc.berkeley.edu/logo/favicon.gif">BOINC
- [<img height="16" width="16" align="center"  src="http://boinc.berkeley.edu/logo/favicon.gif">BOINC*是一款用来贡献计算资源的应用*](http://boinc.berkeley.edu/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">KRuler
- [<img height="16" width="16" align="center"  src="https://www.kde.org/favicon.ico">KRuler*是一款制定屏幕分辨率规则和颜色测量的工具*](https://www.kde.org/applications/graphics/kruler/)
{.links-list}

## <img height="16" width="16" align="center"  src="/favicon.ico">Dr.Geo
- [<img height="16" width="16" align="center"  src="/favicon.ico">Dr.Geo*是一款交互式的几何形状分布的应用*](http://www.drgeo.eu/)
{.links-list}

## <img height="16" width="16" align="center"  src="https://wxmaxima-developers.github.io/wxmaxima/wxmaxima.svg">wxMaxima
- [<img height="16" width="16" align="center"  src="https://wxmaxima-developers.github.io/wxmaxima/wxmaxima.svg">wxMaxima*是一个基于wxWidgets的计算机代数系统应用*](http://andrejv.github.io/wxmaxima/)
{.links-list}


