note: 1. 建议优先使用商店里的深度显卡驱动管理器安装NVIDIA驱动。
            2. 安装黑屏后,只能recovery模式或单用户模式下卸载驱动

##简介
本经验由深度论坛用户(leonardo520 )分享

[原文地址](https://bbs.deepin.org/forum.php?mod=viewthread&tid=132312)

##正文

声明：这个教程是我归纳整理一位热心的网友教授我的方法，非常感谢这位大哥的热心帮助，解决了困扰我很久的问题。本着开源精神把此方法分享出来，希望能帮助到和我一样被独立显卡驱动所困扰的同学。欢迎大家交流，一同进步。


###安装独立显卡驱动
注解：这里介绍的方法是从官方仓库中找到编译好的驱动来安装的。

第一步：安装驱动

        sudo apt-get install bumblebee-nvidia nvidia-driver nvidia-settings

注解：nvidia-driver对应仓库里最新的nvidia驱动程序。现在软件仓库里面只有367,340,304三个版本，分别对应 nvidia-driver, nvidia-legacy-340xx-driver, nvidia-legacy-304xx-driver。

目前linux下有三种optimus的实现：

- l nouveau-only: PRIME GPU offloading using nouveau
- l nvidia-only: nvidia's more recent implementation, also packaged as nvidia-prime in Ubuntu
- l nouveau or nvidia: bumblebee

ubuntu采用的是第二种，debian只打包了第三种bumblebee。ubuntu的nvidia-prime如果要切换显卡，必须要重启X session，因为在X启动的时候nvidia的驱动模块就已经加载了，也就是说独显是一直在工作的。而debian采用bumblebee，开机加载的是intel的驱动，程序默认也都是用intel的集显，如果需要用独显要用optirun运行程序，这样能做到最大程度的提高电池续航能力。目前debian的nvidia-driver，nvidia-legacy-driver都是默认bumblebee解决双显卡，X启动时，驱动是intel，glx是mesa的glx，但是有些硬件可能会出现驱动是intel，glx却是nvidia的情况，这就会导致opengl的程序跑不起来，需要手动执行sudo update-alternatives --config glx来选择。

两种实现其实各有利弊，debian当前也没有打包prime的打算打包方式不同，debian这边没有打包适配prime的驱动，加prime支持要改东西太多，所以就只用大黄蜂了。

第二步：检查驱动是否安装成功

        sudo apt-get install mesa-utils

注解：安装mesa-utils这个包，用来显示系统的glx相关信息。

        optirun glxinfo|grep NVIDIA

注解：用optirun调用独显输出系统的glxinfo来查看驱动是否安装成功。如果打开nvidia-settings时提示“You do not appear to be using the NVIDIA X driver”,在terminal中运行如下命令optirun -b none nvidia-settings -c :8

测试 Bumblebee 是否支持你的 Optimus 系统:

        optirun glxgears -info

如果在终端中看到一个关于你的 Nvidia 的提示，恭喜你，Bumblebee 和 Optimus 已经开始工作了。

如何使用bumblebeek开启独立显卡玩游戏：

l Primusrun 程序名 或者 optirun 程序名

例如：Primusrun firefox 或者optirun firefox

l Stream中如何使用bumblebee开启独显玩游戏

https://support.steampowered.com/kb_article.php?ref=6316-GJKC-7437

##相关链接
[安装最新NVIDIA驱动](https://linuxconfig.org/how-to-install-the-latest-nvidia-drivers-on-debian-9-stretch-linux#h2-1-firmware)