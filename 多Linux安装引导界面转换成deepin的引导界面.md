[[en:System_hang]]


##简介

当你的电脑安装了多个linux的时候，你想把引导界面换成deepin的Grub2引导界面的时候，在deepin系统中重装Grub2即可。

##处理方法

打开深度终端，执行以下命令（分条执行）：

sudo grub-install /dev/sda

sudo update-grub

执行完重启引导界面即更换为deepin的图形引导界面。

##参考链接

https://bbs.deepin.org/forum.php?mod=viewthread&tid=26640&extra=

由坛友“linphp”提供