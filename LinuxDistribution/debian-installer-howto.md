# Debian发行版基础系列三:debian-installer 原理分析(尘封归档篇)

## 安装器的组成

* kernel  用于引导安装介质的kernel;
* initrd  用于辅助内核初始化安装器环境的initrd, 由一系列udeb包组成.需要特别介绍下udeb包，udeb和deb格式相同扩展名不同，重要区别就是udeb仅仅用于构建debian-installer;

## 安装器工作流程

当可引导安装介质(安装光盘，安装U盘)启动后，可以选择进入文本向导模式或图形向导模式，然后完成如下操作

* 加载 kernel，initrd，加载额外的Udeb包,完成安装界面初始化环境;
* 如果启动参数包含preseed文件，则会按照preseed文件内定义的规则自动执行,如果没有对应的规则,则返回交互界面;
* 交互界面会引导用户完成键盘，时区，主机名，网络，用户密码，分区等设置，存储在当前安全器环境中;
* 完成分区格式化等操作后，该磁盘会被挂载到 /target，安装器会调用debootstrap在 /target 完成核心系统的构建;
* 安装器通过执行 chroot 操作进入以 /target 为根的系统中，完成软件源的更新，执行 tasksel，弹出可选软件菜单;
* 安装可选的软件包, 完成上述操作后,最后配置 grub, 将之前保存的全部设置应用，完成安装过程. 

## 上游参考文档

(http://d-i.alioth.debian.org/doc/internals/index.html)


## 重要记录：

* ext2/3/4分区格式化参数： 修改 e2fsprogs 源码包 misc/mke2fs.conf.in 
* 修改安装器默认背景logo： https://github.com/SixKnot/rootskel-gtk
* 调整安装器自动挂载磁盘： https://github.com/SixKnot/mountmedia
* 添加U盘安装支持功能:     https://github.com/SixKnot/cdrom-detect
* 修改默认分区格式:        git@bj.git.sndu.cn:Debian9-Server-Packages/partman-base.git
* 修改分区提示信息:        git@bj.git.sndu.cn:Debian9-Server-Packages/partman-ext3.git 
* 选择内核软件包：         git@@bj.git.sndu.cn:Debian9-Server-Packages/base-installer.git
