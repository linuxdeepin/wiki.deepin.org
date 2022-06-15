---
title: deepin常用资源整理（V3.2）
description: 
published: true
date: 2022-06-15T03:20:20.628Z
tags: 
editor: markdown
dateCreated: 2022-06-15T02:54:30.594Z
---

# deepin常用资源整理（V3.2）

在这里我整理了一些有帮助的资源，帮助大家更快捷地找到想看到的内容。
感谢所有提供了资源的网友。若你知道更多对他人有用的资源，或觉得本文需要改进，欢迎在评论中补充或是参与协作。
部分资料来源于互联网，仅供学习交流使用，版权归原作者所有，若涉及侵权请联系我删除；解决方案仅供参考，不一定有效，请以实际操作为准。

## 提示
1. 点击“大纲”中的标题可跳转到对应的位置。（移动版页面在左下角打开大纲）
2. 若要从 GitHub/Gitee 下载软件之类的成品，请访问项目地址，在网页的右侧找到“Releases”或“发行版”版块，点击下载最新版本，或跳转到Releases页面查看所有版本。
3. 该页面的资源分类有些不合理，但我暂时没有时间进行调整。可以借助浏览器的“在页面上查找”功能寻找你想要的资源，输入的关键词尽量简单精确。

## 一、综合

### 1.1 入门

1. 统信UOS家庭版｜新手安装锦囊：https://bbs.chinauos.com/post/8738
2. 我的deepin变形记：https://bbs.deepin.org/zh/post/228568
3. 写给deepin小白的入门教程：https://bbs.deepin.org/zh/post/209755
4. 深度系统浅度入门指南：https://bbs.deepin.org/zh/post/206130
5. 终于基本能用了，分享一下心得：https://bbs.deepin.org/zh/post/217375  


### 1.2 知识汇总
1. 助力UOS总站：论坛页面  腾讯文档页面
2. 统信UOS家庭版帮助文档：https://home.uniontech.com/help/zh_CN/Home/21.3/dde.html
3. Deepin Community Wiki：https://deepin.wiki/index.php?title= 
4. 深度易经：https://github.com/bubifengyun/deepin-bible （作者）  金山文档转存链接
5. deepin折腾笔记：https://bbs.deepin.org/zh/post/191781  金山文档转存链接(v6.6) 
6. 自己摸索的一些解决方案汇总 ：https://bbs.deepin.org/zh/post/209636  

### 1.3 博客等
1. 青菜芋子的博客：https://loafing.cn/tags/Deepin/
2. shenmo的世界 | Linux：https://shenmo7192.gitee.io/tags/linux
3. ManateeLazyCat的博客：https://manateelazycat.github.io

## 二、系统
### 2.1 系统安装

安装deepin 20.4之后的版本，安装器能自动识别已有EFI分区，无需再另外手动创建。
1. 安装教程（官方）：https://www.deepin.org/zh/installation/ 
2. swin10下的Deepin双系统安装小白教程：https://bbs.deepin.org/zh/post197659  金山文档转存链接
3. 如何安装deepin-windows双系统：https://bbs.deepin.org/zh/post/222739
4. 系统安装教程：https://bbs.deepin.org/zh/post/226286
5. 双系统安装及手动分区方法：https://bbs.deepin.org/zh/post/226463
6. 11代Intel新硬件安装深度：https://bbs.deepin.org/zh/post/220443
7. 不知道怎么分区的看过来：https://bbs.deepin.org/zh/post/194267
8. 关于装系统出现代码花屏解决方法，grub引导：https://bbs.deepin.org/zh/post/196084
9. 判断BIOS的启动模式和磁盘分区格式：https://bbs.deepin.org/zh/post/225766
10. 自定义安装后如何给分区分配卷标/盘符：https://bbs.deepin.org/zh/post/207279
11. 用ventoy运行deepin不用安装到硬盘的方法：https://bbs.deepin.org/zh/post/223203 （不推荐，这样做要等很长时间才能进入桌面，且系统密码未知）
12. Linux逻辑卷管理（LVM）系统折腾者的利器-分分钟再加一个Linux：https://bbs.deepin.org/zh/post/227941
13. 把 Deepin Linux 安装到“带区卷软阵列+逻辑卷（RAID0+LVM）”上：https://bbs.deepin.org/zh/post/227880
14. 无需命令行，双系统加密安装deepin的解决方案：https://bbs.deepin.org/zh/post/215392
15. 通过PXE批量部署安装Deepin V20【视频教程】：https://bbs.deepin.org/zh/post/207368
16. 我的Deepin初始化脚本：https://bbs.deepin.org/post/228930
17. 使用VMware workstation 安装和管理deepin：https://bbs.deepin.org/zh/post/232573
18. 使用kvm安装和管理deepin：https://bbs.deepin.org/zh/post/232573
19. 自制 deepin Live CD——1.1.0——支持本地安装启动：https://bbs.deepin.org/zh/post/236521
20. 【建议】使用Btrfs分区方案：https://bbs.deepin.org/zh/post/238188

附  在U盘上安装deepin
建议在USB 3.0或以上的U盘中安装deepin，启动时使用电脑的USB 3.0接口，否则会很卡。U盘存储空间至少为20GB。
[grub实现]U盘引导多个linux镜像安装,同时支持BIOS和UEFI模式：https://my.oschina.net/abcfy2/blog/491140
关于 Linux/Deepin to go 的一些心得：https://bbs.deepin.org/zh/post/224084
简单6步，把deepin装进口袋：https://bbs.deepin.org/zh/post/224438
Deepin装在 vhd/vdi 中使用：https://bbs.deepin.org/zh/post/209674
2.2 系统启动和引导
启动显示BusyBox … built-in shell(ash) ：https://www.cnblogs.com/lbhqq/p/6964746.html
Grub进入命令行：https://bbs.deepin.org/zh/post/210805
华为、荣耀笔记本双系统无法正常引导Windows解决办法：https://bbs.deepin.org/zh/post/205701
grub修复：https://bbs.deepin.org/zh/post/222216 （这里的修复只适用于部分情况）
无法开机，提示You are in emergency mode ... Cannot open access：https://bbs.deepin.org/zh/post/237419
2.3 系统内核
双内核使用说明：https://bbs.deepin.org/zh/post/216952
操作系统的内核到底是什么：https://bbs.deepin.org/zh/post/229142
deepin20.5+Linux内核5.18稳定版：https://bbs.deepin.org/zh/post/237619
如何编译安装Linux内核：https://www.cnblogs.com/harrypotterjackson/p/11846222.html
2.4 硬件和设备
显卡笔记本安装N卡闭源驱动教程：https://bbs.deepin.org/zh/post/215066
Deepin双显卡之bumblebee(大黄蜂)、Prime及手动切换方案：https://bbs.deepin.org/zh/post/210053
NVIDIA独立显卡的安装方法：https://bbs.deepin.org/zh/post/223856
一键安装NVIDIA显卡驱动：https://bbs.deepin.org/zh/post/227866
UOS/DEEPIN安装nvidia最新闭源驱动的教程：https://bbs.deepin.org/zh/post/232923
deepin安装N卡驱动教程：https://bbs.deepin.org/zh/post/238766
uos和deepin安装较新amd显卡驱动完整版：https://bbs.deepin.org/zh/post/237734
屏幕闪烁问题的解决办法：https://bbs.deepin.org/zh/post/224687
如何解决系统安装之后没有声音的情况：https://bbs.deepin.org/zh/post/195889
20.3打印成功经验分享：https://bbs.deepin.org/zh/post/228900
关于使用虚拟机解决驱动问题的可能性探讨：https://bbs.deepin.org/zh/post/228005
linux下外设辅助软件推荐：https://bbs.deepin.org/zh/post/226246
安装兄弟Brother打印机扫描驱动及配置文件的方法：https://bbs.deepin.org/zh/post/232569
惠普打印扫描一体机开启扫描功能：https://bbs.deepin.org/zh/post/238968
2.5 问题解决
deepin和windows双系统时间不同步的解决方法：https://bbs.deepin.org/zh/post/220213  最好的方法是分别设置Windows和Linux的时间与网络同步
用snapd安装软件后出现多个磁盘：https://bbs.deepin.org/zh/post/203517 （解决方法在7楼）
终于干掉snapd分区了：https://bbs.deepin.org/zh/post/213673
出现I/O或者blk报错的简易解决方法：https://bbs.deepin.org/zh/post/224416
dde-control-center的快捷键设置无法拉起deepin-screen-recorder：https://bbs.deepin.org/zh/post/227910
禁止kwin的自动关闭3D窗管的方法：https://bbs.deepin.org/zh/post/232810
还在为没有公钥导致的无法更新而烦恼吗？这里有解决方案：https://bbs.deepin.org/zh/post/237312
deepin忘记密码怎么办？--Live系统一步修改用户密码：https://bbs.deepin.org/zh/post/238135  （注意：任何人都可以重设密码，因此请勿单纯依赖系统密码来保护数据安全）
2.6 系统使用
2.6.1 桌面环境
在Linux中创建应用图标：https://bbs.deepin.org/zh/post/225323
安装gnome：https://bbs.deepin.org/zh/post/207834（见回复）
通过外部应用实现的热区：omd-requ：https://bbs.deepin.org/zh/post/226623
在Linux下配置人脸识别：https://bbs.deepin.org/zh/post/224848
深度Linux Deepin设置分辨率为1920x1080：https://www.jianshu.com/p/89a1bf1905d8
手把手教你用deepin20——多任务视图与窗口技巧：https://bbs.deepin.org/post/233111
better-dde: 让 DDE 更美好：https://bbs.deepin.org/zh/post/237746
2.6.2 应用处理
删除卸载残留的应用配置：https://bbs.deepin.org/zh/post/227702
在dde File Manager的较上端加入菜单项（类似深度压缩）：https://bbs.deepin.org/zh/post/229467
deb软件包安装卸载失败修复教程：https://bbs.deepin.org/zh/post/217421
deepin中Typora无法设置为默认程序的解决办法：https://blog.csdn.net/Charley_Leo/article/details/107091222
deepin更新提示缺少release文件：https://bbs.deepin.org/post/238809
2.6.3 输入法
创作自己的输入法皮肤(适用于旧版fcitx)：https://bbs.deepin.org/zh/post/210018
安装及使用Rime输入法--中州韵输入法：https://bbs.deepin.org/zh/post/207410
手动编绎fcitx5教程：https://bbs.deepin.org/zh/post/208959
Deepin 20.3上使用Fcitx5的方法：https://bbs.deepin.org/post/224852
deepin正式版/uos家庭版安装fcitx5：https://bbs.deepin.org/zh/post/223758
fcitx5输入法使用技巧&简约皮肤分享：https://bbs.deepin.org/zh/post/223743
deepin深度简约 fcitx5主题：https://bbs.deepin.org/zh/post/228832
中州韵98五笔助手：https://bbs.deepin.org/zh/post/231400
输入法不跟随光标：https://bbs.deepin.org/zh/post/231849
默认输入法配置：https://bbs.deepin.org/zh/post/232652
【deepin个性化配置(2)】安装和配置Fcitx5输入法：https://bbs.deepin.org/zh/post/233230
2.6.4 性能
交换空间：https://wiki.archlinux.org/title/Swap_(简体中文) （适用于deepin，安装系统后可按“交换文件”部分设置swap）
对小内存用户的使用建议 防卡设置：https://bbs.deepin.org/zh/post/199563 （文件所在位置为/usr/lib/sysctl.d/deepin.conf ，要以管理员身份打开）
为什么空闲时CPU睿频起飞：https://bbs.deepin.org/zh/post/222430
解决Deepin下CPU不能自主降频问题：https://bbs.deepin.org/zh/post/194744
deepin桌面卡死处理：https://bbs.deepin.org/zh/post/225151
启动wine qq时偶发性使桌面崩溃：https://bbs.deepin.org/zh/post/207380
性能与功耗之间的权衡与调整第三版：https://bbs.deepin.org/zh/post/223793
nvidia显卡firefox硬解在线视频：https://bbs.deepin.org/zh/post/233052
deepin如何给根目录扩容：https://bbs.deepin.org/zh/post/237402
2.6.5 命令与终端
deepin-terminal的新改造：https://bbs.deepin.org/zh/post/225366
终端deepin-terminal自定义命令和分屏杂谈：https://bbs.deepin.org/zh/post/237641
使用过的一些命令分享：https://bbs.deepin.org/zh/post/223660
以root权限运行命令配置无需输入密码（sudo和pkexec）：https://bbs.deepin.org/zh/post/229374
apt和apt-get的区别：https://bbs.deepin.org/zh/post/208702
查看系统安装的日期与时间：https://bbs.deepin.org/zh/post/229311
手把手教你装zsh，所有github链接都换成了gitee，包你安装顺利：https://bbs.deepin.org/zh/post/237774
使用fish代替bash，真好用：https://bbs.deepin.org/zh/post/238189
2.6.6 文件处理
不安装任何软件实现局域网快速共享文件：https://bbs.deepin.org/zh/post/209250
如何设置共享文件，windows访问deepin共享文件方法：https://bbs.deepin.org/zh/post/195192
deepin下如何访问Windows共享资料：https://bbs.deepin.org/zh/post/208246
Linux桌面环境与Win10之间共享文件夹的互相访问：https://www.jianshu.com/p/f872fe1d02dc
回收站无法清空的解决方法：https://bbs.deepin.org/zh/post/205839
开机自动挂载webdav：https://bbs.deepin.org/zh/post/229044
FTP服务搭建（vsftpd配置使用）：https://blog.csdn.net/babyfengfjx/article/details/122837362
使用docker拉取CloudDrive镜像实现把网盘挂载到系统：https://bbs.deepin.org/zh/post/237552
格式化硬盘后文件管理器显示62.3G占用：https://bbs.deepin.org/post/238570
2.6.7 其它
优雅地创作和分享技术博客：https://bbs.deepin.org/post/228934
在deepin上使用LaTeX：https://bbs.deepin.org/zh/post/229734
一句命令行安装live系统 解决手动分区无备份还原系统等问题：https://bbs.deepin.org/zh/post/215165
Windows ssh 客户端 PuTTY 正向与反向流量转发（转）：https://bbs.deepin.org/zh/post/230148
deepin下ssh常用网络功能：https://bbs.deepin.org/zh/post/231835
桌面固定网络驱动器的教程视频-适用群晖、威联通NAS/共享文件夹：https://bbs.deepin.org/zh/post/231878
助力轻松修改你的系统用户名：https://bbs.deepin.org/zh/post/232575
深度文件管理器改造小记：https://bbs.deepin.org/zh/post/237345
找了很久的 while 无限循环终于解决了输入判断的问题：https://bbs.deepin.org/zh/post/236712
自定义控制中心的关于本机：https://bbs.deepin.org/zh/post/237500
如何设置锁屏时间为3min?：https://bbs.deepin.org/zh/post/237606
控制中心 VPN 网关只能输入 IP 地址很反人类？试试这个：https://bbs.deepin.org/zh/post/238342
2.7 系统美化
2.7.1 壁纸
壁纸图片见下面的“5.3 壁纸”。
壁纸与屏保--让你的deepin更好用：https://bbs.deepin.org/zh/post/225531
修改锁屏壁纸：https://bbs.deepin.org/zh/post/224150
让Deepin吃上macOS的动态壁纸：https://bbs.deepin.org/zh/post/206933
deepin上超好用的动态壁纸软件-幻梦动态壁纸：https://bbs.deepin.org/zh/post/220785
幻梦动态壁纸,DIY系统版：https://bbs.deepin.org/zh/post/228082
发现deepin登陆壁纸寻找逻辑：https://bbs.deepin.org/zh/post/227912
觉得启动器背景太丑？试试这个：https://bbs.deepin.org/zh/post/237678
教你怎样自己制作暗色壁纸：https://bbs.deepin.org/zh/post/238967
2.7.2 程序窗口
一行命令美化标题栏大额头，可设置任意高度和任意颜色：https://bbs.deepin.org/zh/post/197036
定制版最大化最小化关闭按钮：https://bbs.deepin.org/zh/post/210071
将菜单显示在标题栏上：https://bbs.deepin.org/zh/post/224774
第三方应用圆角适配，移植于cutefish：https://bbs.deepin.org/zh/post/226223
分享一种缩小应用标题栏（额头）高度的方法：https://bbs.deepin.org/zh/post/234783
2.7.3 Dock、顶栏
dde-top-panel 顶栏程序+全局菜单 (V20)：https://bbs.deepin.org/zh/post/195128
定制的dde-dock分享：https://bbs.deepin.org/zh/post/224228
Linux桌面最轻量的Dock之Plank：https://bbs.deepin.org/zh/post/215170
Deepin 上的实时网速推荐 lfxNet（重构 lfxSpeed ）：https://bbs.deepin.org/zh/post/213210
2.7.4 字体
如何在deepinOS上加入方正字体：https://bbs.deepin.org/zh/post/228540
超1000款UOS字体包：链接: https://cloud.189.cn/t/uyeuQjU3IBZz 密码: dr0l
华为鸿蒙中文字体DEB包：https://bbs.deepin.org/zh/post/221591
更换系统默认中文字体的技巧：https://bbs.deepin.org/zh/post/237625
2.7.5 其他
【Mac布局美化】美化无止境，只要肯折腾：https://bbs.deepin.org/zh/post/196796
Bloom Origin 图标提取分享：https://bbs.deepin.org/zh/post/223230
图标主题的继承关系：https://bbs.deepin.org/zh/post/203946
deepin美化系列教程（五）：https://bbs.deepin.org/zh/post/227803
2.8 系统有关介绍
你好，deepin：https://bbs.deepin.org/zh/post/223462
深度桌面环境介绍：https://bbs.deepin.org/zh/post/213341
分享那些可能被你忽略的deepin自带工具：https://bbs.deepin.org/zh/post/227223
20.2.3新功能展示——OCR文字识别：https://bbs.deepin.org/zh/post/225260
万众7待，焕新出发，deepin新版商店为你而来：https://bbs.deepin.org/zh/post/222677
全新的图标主题，就静静地欣赏，细细的品味：https://bbs.deepin.org/zh/post/223494
20.3新版本特性介绍（一）全局搜索：https://bbs.deepin.org/zh/post/228676
20.3新版本特性-文件管理器：https://bbs.deepin.org/zh/post/230030
20.4新版本特性①藏宝箱：https://bbs.deepin.org/zh/post/231326
20.4新版本特性②安装器：https://bbs.deepin.org/zh/post/231570
【视频】怀一腔孤勇，筑梦前行：https://bbs.deepin.org/zh/post/231100
20.4深度使用体验：https://bbs.deepin.org/zh/post/231912
deepin 20.5——人脸解锁背后的那些事：https://bbs.deepin.org/zh/post/234359
20.6 deepin文字识别突然变得好用了？OCR升级的秘密在这里：https://bbs.deepin.org/zh/post/238149
三、应用软件
3.1 应用分享和使用技巧
你可以从星火应用商店（一款由社区爱好者维护，致力于丰富Linux生态的第三方应用商店）获取一些最新版本的Linux应用和开箱即用的wine应用。官网：https://www.spark-app.store（向星火商店捐赠）
deepin软件推荐（很多）：https://bbs.deepin.org/zh/post/237514
3.1.1 系统工具
Oh my dde：论坛页面  Gitee页面
iOS14风格小部件正式发布啦：https://bbs.deepin.org/zh/post/213377
Linux桌面小部件第一个阶段性版本来了：https://bbs.deepin.org/zh/post/231461
AppImage安装器 大更新 全新版本：https://bbs.deepin.org/zh/post/230017
deepin下的超级效率工具—超级标签栏：https://bbs.deepin.org/zh/post/220860
云旗OS助手，安装系统的新方法，免启动盘！：https://bbs.deepin.org/zh/post/203108 （不支持deepin，支持UOS）
deepin下使用Ventoy安装Windows：https://bbs.deepin.org/zh/post/209123
超卓文件管理器deb一键安装包分享：https://bbs.deepin.org/zh/post/222897
用DTK写的定时器：https://bbs.deepin.org/zh/post/223465
R计算器手动打包：https://bbs.deepin.org/zh/post/191341
MPaste：剪贴板管理工具：https://bbs.deepin.org/zh/post/220914
UOS 21.1提取软件包分享：深度安全中心&远程协助&系统诊断工具：https://bbs.deepin.org/zh/post/228885
统信UOS助手移植分享：https://bbs.deepin.org/zh/post/230801
App 4 Deepin(国际开发者为deepin开发的一套实用软件)：https://app4deepin.com/#home
OBS教程：3分钟学会直播推流与视频录制：https://bbs.deepin.org/zh/post/224816
screenkey：在屏幕上实时显示键盘操作：https://bbs.deepin.org/zh/post/227849
一个好用又好看的UEFI启动管理器rEFInd：https://bbs.deepin.org/zh/post/221068
Pensela：一款跨平台屏幕注释工具：https://bbs.deepin.org/zh/post/227701
用apt-fast加速软件包下载：https://bbs.deepin.org/zh/post/230874
Clash for Windows Linux版分享：https://bbs.deepin.org/zh/post/229552
做了个小应用：Tips 生成器 增添新手使用乐趣：https://bbs.deepin.org/zh/post/228818
apt 软件包信息查看器：论坛页面  Gitee页面
infomation-tips 系统状态提示器：https://bbs.deepin.org/zh/post/230635
desktop 图标文件生成器：https://bbs.deepin.org/zh/post/231528
一个类似uTools的快速启动器Albert：https://bbs.deepin.org/post/142027
自己写的小工具：folder-merger文件夹合并：https://bbs.deepin.org/zh/post/232545
开发了一款自定义触摸板手势的管理工具，有需要的朋友可以试试：https://bbs.deepin.org/zh/post/232824
Steam++ 加速你的github 和 steam游戏商店：https://bbs.deepin.org/zh/post/233231
deepin下载器接管Edge：https://bbs.deepin.org/post/236400
Deepin Maye：https://bbs.deepin.org/post/236438
WingCleaner 一个个人开发简单实用的脚本：论坛页面   gitee页面
基于dtk和rdesktop的rdp远程桌面工具1.2.0-1发布：https://bbs.deepin.org/zh/post/237866
深度取色器：https://bbs.deepin.org/zh/post/237906
基于Deepin下的Anaconda3安装与使用：https://bbs.deepin.org/zh/post/238563
截屏+OCR+搜索+贴图+以图搜图 ----eSearch应用：https://bbs.deepin.org/zh/post/238703
船新的Dock插件，助你认识网瘾，欢迎尝鲜/捉虫：https://bbs.deepin.org/zh/post/238791
3.1.2 网络应用
天翼云网盘（wine）下载及打包教程：https://bbs.deepin.org/zh/post/221558
一条命令让火狐浏览器可自动更新到最新版：https://bbs.deepin.org/zh/post/224603
如何让火狐国际版拥有中国版的功能：https://bbs.deepin.org/zh/post/228249
火狐浏览器国际版的简单设置：https://bbs.deepin.org/zh/post/237756
优雅地使用坚果云：https://bbs.deepin.org/zh/post/225170
解决megasync不能打开的问题：https://bbs.deepin.org/zh/post/206774
写了一个简单的图床管理软件，欢迎各位试用：https://bbs.deepin.org/zh/post/233372
3.1.3 办公学习
为知笔记deepin版闪亮登场：https://bbs.deepin.org/zh/post/209845
为知笔记最新版编译，有一丝丝的deepin特殊适配：https://bbs.deepin.org/zh/post/222180
有道云笔记linux版beta-1.1.3：https://bbs.deepin.org/zh/post/221134
为知笔记重构版（官方重构）无依赖打包分享：https://bbs.deepin.org/zh/post/227217
Microsoft Office PowerPoint 2007(wine)：https://bbs.deepin.org/zh/post/224229
专业的云端知识库——语雀：https://bbs.deepin.org/zh/post/221917
专业的素材管理应用——Eagle：https://bbs.deepin.org/zh/post/221954
完美解决腾讯会议在deepin上运行问题：https://bbs.deepin.org/zh/post/209769
安卓版腾讯会议…居然还挺好用？[附食用教程]：https://bbs.deepin.org/zh/post/238329
WPS for linux字体显示问题：https://blog.csdn.net/qq_36191272/article/details/105596225
使用 Dtk 开发了一个 MarkDown 编辑器，欢迎大家试用：https://bbs.deepin.org/zh/post/228829
一款词典工具：https://bbs.deepin.org/zh/post/227166
Pixso上架deepin应用商店，设计应用生态强势补充：https://bbs.deepin.org/zh/post/235058
3.1.4 社交沟通
Icalingua：第三方QQ客户端：https://bbs.deepin.org/zh/post/226550
解决QQ(wine)因字体卡死&宋体发虚太难看的一种方法：https://bbs.deepin.org/post/213530
Wechat（微信） Linux升级版：freechat-spark 可过验证：https://bbs.deepin.org/zh/post/226549
星火微信Linux2.1.2-2已更新：https://bbs.deepin.org/zh/post/231200
deepin foxmail通信录导入：https://bbs.deepin.org/post/233037
3.1.5 音乐视频
自动从网络上匹配歌词的Deepin Music：https://bbs.deepin.org/zh/post/221829
功能完善、社区活跃的超级播放器——KODI：https://bbs.deepin.org/zh/post/224939
神奇的全网视频下载工具 you-get：https://bbs.deepin.org/zh/post/221314
网易云音乐调整缩放比例的方法：https://bbs.deepin.org/zh/post/225377
使用Deepin深度系统制作音乐：https://bbs.deepin.org/zh/post/223386
开源的第三方SoundCloud客户端 SoundNote：https://bbs.deepin.org/zh/post/237530
bilimini——跨平台的好用的 B 站“桌面端”：https://bbs.deepin.org/zh/post/225784
b站辅助插件bilibili-evolved现已支持b站视频导出至mpv播放：https://bbs.deepin.org/zh/post/232228
高颜值的第三方网易云音乐播放器（强烈推荐）：https://github.com/qier222/YesPlayMusic/releases
Kdenlive22.04.0-2修复Deb包，支持字幕自动生成功能：https://bbs.deepin.org/zh/post/237153
wine版剪映基本能用了，字幕和画面都出来了：https://bbs.deepin.org/zh/post/238330
Wine7.9安装剪映全过程指南：https://bbs.deepin.org/zh/post/238301
打包剪映专业版（wine），并教你解决视频预览窗口显示黑屏的问题：https://bbs.deepin.org/zh/post/238449
3.1.6 游戏娱乐
Wine游戏收集贴-持续不定期更新-不定期更新链接：https://bbs.deepin.org/zh/post/217731
使用 PlayOnLinux 和 winetricks 完美运行《白色相簿2》：https://bbs.deepin.org/zh/post/214585
云原神 1.1.0版本直装包：https://bbs.deepin.org/zh/post/225788
linux下玩24点(题目生成+计算)：https://bbs.deepin.org/zh/post/217418
用 mame 命令玩 MAME 街机游戏：https://bbs.deepin.org/zh/post/208391
植物大战僵尸中文版打包分享：https://bbs.deepin.org/zh/post/195096
谁说deepin不能玩游戏：https://bbs.deepin.org/zh/post/235387
一个动漫游戏启动器-2.3.2-wine版本：自带wine和dxvk测试：https://bbs.deepin.org/post/236466
3.1.7 多设备协同
安卓手机后台管理工具黑阈激活器（DTK版本）：https://bbs.deepin.org/zh/post/208066
Macast —— 一个跨平台的DLNA投屏接收器：https://bbs.deepin.org/zh/post/225095
Deskreen投屏，Linux同wifi下投屏的解决方案：https://bbs.deepin.org/zh/post/230214
vivo互传（wine）：https://bbs.deepin.org/zh/post/221939
deepin下安装Citrix Receiver连接云桌面：https://bbs.deepin.org/zh/post/232020
3.1.8 其他应用
分享几个脚本工具：https://bbs.deepin.org/zh/post/221680
几款国产软件的设置：https://bbs.deepin.org/zh/post/222707
deepin20.2.3 安装win版招商证券  V7.09的方法：https://bbs.deepin.org/zh/post/226419
星火商店柚子重构版尝鲜下载：https://bbs.deepin.org/zh/post/228515
deepin如何寻找、安装非商店软件(Linux通用)：https://bbs.deepin.org/post/157341
3.2 运行其他平台应用、虚拟机
3.2.1 Uengine
Uengine用来在deepin/UOS上运行安卓应用，应用商店中的手机应用就使用了Uengine。同类应用还有Anbox、xDroid等。
uengine 使用总结：https://uos.osystem.club/102.html
Deepin20.2.2运行安卓和自行安装软件：https://bbs.deepin.org/zh/post/222222
大家快来安装安卓应用：https://bbs.deepin.org/zh/post/222286
UEngine 运行器：论坛页面 (1.6.1)  Gitee页面
APK打包器beta：https://bbs.deepin.org/zh/post/222729
成功Root Deepin的Android子系统（Uengine）：https://bbs.deepin.org/zh/post/228520
关于安装未知来源的apk，被禁止安装处理方法：https://bbs.deepin.org/zh/post/237401
3.2.2 Wine
Wine用来在Linux等系统上运行Windows应用。应用商店中的Wine应用使用了deepin-wine（deepin基于Wine进行了优化的版本）。
Deepin-Wine适配知识库：https://docs.qq.com/mind/DWFBpbmpjd0RtV2Z0
deepin-wine5 折腾过程兼新手教程：https://bbs.deepin.org/zh/post/200942
Wine助手初体验，一款跨平台应用安装兼容工具：https://bbs.deepin.org/zh/post/223564
wine 运行器1.3.1：论坛页面   Gitee页面
deepin-wine5 应用打包器：https://bbs.deepin.org/zh/post/214306
wine自动打包程序：https://bbs.deepin.org/zh/post/221842
Deepin用wine安装的Windows软件目录在哪：https://bbs.deepin.org/zh/post/227183
用wine安装官方最新版钉钉：https://bbs.deepin.org/zh/post/189460 (这是旧版系统的教程，但仍可以学习借鉴)
wine程序添加快捷键：https://bbs.deepin.org/zh/post/219312
安装wine-gecko 和wine-mono的步骤：https://bbs.deepin.org/zh/post/208082
CrossOver 17 测试版，解除时间限制：https://bbs.deepin.org/zh/post/231306
wine工具盘点——linux上运行windows软件必备神器：https://bbs.deepin.org/zh/post/237398
Deepin Wine6的run_v4脚本探索——启动方式：https://bbs.deepin.org/zh/post/237953
安装WineHQ：https://bbs.deepin.org/zh/post/238337
WineHQ使用时出现中文乱码：https://bbs.deepin.org/zh/post/238339
wine使用教程3-打包deepin-wine6-stable应用的方法：https://bbs.deepin.org/zh/post/238778
3.2.3 虚拟机
VMware 虚拟机3种安装方法： https://bbs.deepin.org/zh/post/221632
开箱即用的虚拟机软件——GNOME Boxes，应用商店可下载！：https://bbs.deepin.org/zh/post/220484
在kde或者gnome实现类 "linux subsystem for windows"体验：https://bbs.deepin.org/zh/post/228448  (可能不适用于deepin)
【deepin安装系列(1)】使用VMware workstation 安装和管理deepin：https://bbs.deepin.org/post/232566
3.3 开发程序
解包/打包deb教程 一周年重置版：https://bbs.deepin.org/zh/post/227931
deb打包教程：https://bbs.deepin.org/zh/post/194188
超级简单打包软件分享：https://bbs.deepin.org/zh/post/194219
统信软件社区开发培训——打包：https://bbs.deepin.org/zh/post/201183
这个打包deb的操作特简单，uos/Deepin打包操作方式，供大家参考：https://bbs.deepin.org/zh/post/225931
UOS/Deepin 中配置 DTK 开发环境：https://bbs.deepin.org/zh/post/209405
DTK程序简单的开发教程（简易的浏览器）：https://bbs.deepin.org/zh/post/207250
一个Dtk程序开发的实例，自己做一个deepin风格的程序：https://bbs.deepin.org/zh/post/198578
C++太繁琐？快来一起给QtQuick（QML）适配dtk风格的控件！：https://bbs.deepin.org/zh/post/222435
参考当年王勇做的DTK列表搞了个新的：https://bbs.deepin.org/zh/post/229550
使用 apt-build 从 deepin 仓库源码构建安装软件包：https://bbs.deepin.org/zh/post/223853
Oracle %ROWTYPE 在MySQL中的移植：https://bbs.deepin.org/zh/post/224122
Deepin Learning on Deepin —— 基础环境架设：https://bbs.deepin.org/zh/post/225041
pyside6开发环境搭建——基于pycharm：https://bbs.deepin.org/zh/post/226994
从源码编译安装GnuCash-4.8：https://bbs.deepin.org/zh/post/228255
最近在跟QtQuick教程，想搞Qt的一起来耍呀：https://bbs.deepin.org/zh/post/232135
微信开发者工具 Linux版 v1.05.2203070 免wine：https://bbs.deepin.org/post/232284
deepin，做一个开发者友好的发行版：https://bbs.deepin.org/zh/post/234593
使用go语言重新实现一款软件包管理器：https://bbs.deepin.org/zh/post/233959
关于preinst等维护者脚本被唤起时可能会获得的参数/状态提示总结：https://bbs.deepin.org/zh/post/237895
spark-dwine-helper文档：https://shenmo7192.gitee.io/categories/spark-dwine-helper%E6%96%87%E6%A1%A3/
四、社区与论坛
4.1 论坛使用
利用百度替代论坛搜索功能：https://bbs.deepin.org/zh/post/221982
（UOS）社区问题及建议反馈须知：http://bbs.chinauos.com/post/5005
使用 markdown 发论坛：https://bbs.deepin.org/zh/post/225059
社区黑暗模式的常用界面已经适配完成，大家可以体验：https://bbs.deepin.org/zh/post/216104
使用油猴将论坛中的图片查看功能全面改造：https://bbs.deepin.org/zh/post/229849
TG交流群组建立,欢迎大家加入：https://bbs.deepin.org/zh/post/230231
如何优雅地在论坛插入b站视频：https://bbs.deepin.org/zh/post/232626
4.2 社区活动（部分）
访谈依云和来自深度社区的竹子和马强：https://bbs.deepin.org/zh/post/220802
深度中国社区大佬面对面第一期：Arch中文社区依云：https://bbs.deepin.org/zh/post/221930
deepin社区AMA「深度用户面对面」直播活动精彩回顾：https://bbs.deepin.org/zh/post/221738
深度中国社区大佬面对面第三期——openSUSE中国社区山木老师：https://bbs.deepin.org/zh/post/226072
DDUC2021，我们不见不散：https://bbs.deepin.org/zh/post/228899
DDUC活动礼品设计灵感：https://bbs.deepin.org/zh/post/228868
DDUC2021——开源你我，始终如一：https://bbs.deepin.org/zh/post/228582
deepin开源社区中心星火行动启动：https://bbs.deepin.org/zh/post/230086
深度开源社区版主轮值活动：https://bbs.deepin.org/zh/post/233938
deepin高校开源社区中心招募啦！就等你来：https://bbs.deepin.org/zh/post/232405
deepin荣获2021年度“科创中国”开源创新榜优秀开源产品：https://bbs.deepin.org/zh/post/232217
深度社区全新规划：打造中国主导的桌面系统根社区：https://bbs.deepin.org/zh/post/237175
五、更多
5.1 测评感受
deepin生产环境下的使用体会：https://bbs.deepin.org/zh/post/222880
Deepin 20.2.2软件深度体验：https://bbs.deepin.org/zh/post/223513
深度向左，优麒麟向右-- Ubuntu Kylin 20.04 LTS Pro 体验测评：https://bbs.deepin.org/zh/post/220515
DEEPIN20.3升级和体验：https://bbs.deepin.org/post/228774
【deepin 20.3】使用心得和未来改进的建议：https://bbs.deepin.org/post/228812
首次体验deepin是什么感觉？ https://bbs.deepin.org/zh/post/229071
强大的深度截图：https://bbs.deepin.org/zh/post/229378
深度操作系统deepin体验分享：（一）  （二）  （三）
5.2 其他系统的资源
UOS各版本系统说明：http://bbs.chinauos.com/post/6514
在其他发行版使用Deepin-Wine的终级方案：https://zhuanlan.zhihu.com/p/379196410
嫁接Deepin到一个兼容你设备的Linux系统：https://bbs.deepin.org/zh/post/206117
 在ubuntu上完美使用deepin-wine5：https://bbs.deepin.org/zh/post/204884
关于 Deepin 桌面移植到 Gentoo Linux 发行版：https://bbs.deepin.org/zh/post/221216
WSL Deepin：https://bbs.deepin.org/zh/post/206395
Deepin鼠标指针：https://bbs.deepin.org/zh/post/199527
从deepin迁移到其他Linux
CutefishOS 0.5 安装搜狗输入法教程：https://bbs.cutefishos.com/d/283-cutefishos-05 （Cutefish OS网站关闭，此资源过时）
移植deepin-contacts到其他Debian系发行版：https://bbs.deepin.org/zh/post/229606
用 Nix 制作自定义的 live 镜像：https://bbs.deepin.org/zh/post/236303
阅读其他系统的Wiki
deepin 20是Debian Linux的一个分支。尽管其他的Linux与deepin有所不同，它们的Wiki仍有值得学习的知识。
Debian Wiki：https://wiki.debian.org/zh_CN/FrontPage?action=show&redirect=%E9%A6%96%E9%A1%B5
openSUSE Wiki：https://zh.opensuse.org/%E9%A6%96%E9%A1%B5
ArchWiki：https://wiki.archlinux.org/title/Main_page_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
5.3 壁纸
壁纸汇总：https://gfdgd-xi.github.io/deepin.background.github.io/ （感谢@gfdgd xi）
deepin壁纸（官方）：https://bbs.deepin.org/zh/post/209087
Zorin OS 壁纸：https://bbs.deepin.org/zh/post/219669
两张失散已久的壁纸：https://bbs.deepin.org/zh/post/223085
从微博搬过来的两张壁纸：https://bbs.deepin.org/zh/post/215923
一个果里果气的桌面——CuteFish Desktop：https://bbs.deepin.org/zh/post/223226
deepin 20.4& Cutefish OS 0.8壁纸：https://bbs.deepin.org/zh/post/231497
deepin壁纸大赛优质壁纸分享：https://bbs.deepin.org/zh/post/236372
5.4 PPT SHOW
此部分作品由社区用户@PossibleVing 设计。
1.第一期：新论坛的 UI ？我觉得这样很 OK：https://bbs.deepin.org/zh/post/191168
2.第二期：deepin V20 社区概念版正式发布！（？）：https://bbs.deepin.org/zh/post/191489
3.第三期：最新版启动器尝鲜体验！（不是）：https://bbs.deepin.org/zh/post/197545
4.第四期：这次是 deepin-greeter 和 dde-dock ~：https://bbs.deepin.org/zh/post/197602
5.第五期：腾讯不可能做出这样的 Linux QQ，除非……：https://bbs.deepin.org/zh/post/197633
6.第六期：DDE，FILE，MANAGER！：https://bbs.deepin.org/zh/post/197723
7.第七期：星星之火，可以燎原！：https://bbs.deepin.org/zh/post/197824
8.第八期：星火应用商店更新好快啊！：https://bbs.deepin.org/zh/post/198986
9.第九期：PPT 自制 deepin 壁纸（和星火壁纸）分享~：https://bbs.deepin.org/zh/post/199297
10.第十期：多屏协同就看它了~：https://bbs.deepin.org/zh/post/199838
11.第十一期：又双叒叕是星火商店官网……：https://bbs.deepin.org/zh/post/200999
12.第十二期：这次……真的是个 PPT ！：https://bbs.deepin.org/zh/post/203159
5.5 其他
网页版deepin：https://bbs.deepin.org/zh/post/219792
盘点 23 款国产桌面发行版：https://bbs.deepin.org/zh/post/231369
回去体验了一下15.7：https://bbs.deepin.org/zh/post/237403


