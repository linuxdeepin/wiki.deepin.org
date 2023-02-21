---
title: live系统修改密码
description: 很多时候用户在使用系统过程中，真的会忘记自己的密码，这个时候想要能够修改密码的方式，这篇文章可能提供一种思路。
published: true
date: 2022-10-21T15:09:03.279Z
tags: live cd, 密码
editor: markdown
dateCreated: 2022-06-01T03:36:17.508Z
---

# Step One:进入live系统

![1.gif](https://storage.deepin.org/thread/202205201618318171_1.gif)
![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=YmU4ZjgxNzAxZWNmNjY2MTg2ZDI5YTU0NzVkNDkyNzFfdzJmVDczbGRtOUlmWGlQSmg5MHRCSmQ0TnB1Y1lZMWNfVG9rZW46Ym94Y25qa3Nkak92YjJ3c0ZjMUN1Q3VxbDhnXzE2NTMwMTQ4MzQ6MTY1MzAxODQzNF9WNA)**很多人可能还不知道如何进入deepin的live系统，可以看上面动图的操作，也是非常简单：**

1. 首先准备好一个装有deepin镜像的启动U盘（推荐使用ventoy）；
2. 直接走装镜像的路子,启动到系统安装界面；
3. 唯一区别是在grub安装界面的时候，不要选择任何选项，而是按一下键盘上的 **“E”** 按键（如果是非EFI启动，可能需要按TAB键）；
4. 按过之后就会出现下图的编辑界面，通过上下左右按键移动到下方红框标识处，删除 **“cd-installer”** 内容；

   ![2.png](https://storage.deepin.org/thread/20220520162047272_2.png)
5. 然后直接按键盘上F10按键，接下来就会直接进入live系统界面了。
6. 进入live系统后是如下界面的样子（下图是V20.6的镜像）：

> 特别提醒：在live系统下长时间也会自动锁屏了，如果你也遇到了锁屏发现没有密码无法进入系统，可能你需要重新来一次，此时可以直接通过ctrl+alt+F2 进入TTY，然后在TTY界面设置密码即可：``sudo passwd uos``,然后再切回来用设置的密码登录即可。

![](https://vjxs22vadq.feishu.cn/space/api/box/stream/download/asynccode/?code=YzA2MjdlZDQ0ZWNjZDM2MjY2NWMwMThmYjk3OTI5OTRfeEg3SXo2OFE3cTR4WlhTRzhkOTh3SmxxY0l3V0doUWJfVG9rZW46Ym94Y25TaHNCQ0kzalI5RjRYOWlFYTF2a1diXzE2NTMwMTYzMzY6MTY1MzAxOTkzNl9WNA)![3.png](https://storage.deepin.org/thread/202205201621386209_3.png)

# Step Two:切换chroot目录

1. 在live系统中打开文件管理器；
2. 找到根目录所在分区（如我这里的Roota）;
3. 进入目录后，右键点击空白处，打开终端；
4. 然后输入 ``sudo chroot ./``;
5. 回车后，我们就切换到原系统的根目录了。

![qiehuanchroot.gif](https://storage.deepin.org/thread/202206011317339861_qiehuanchroot.gif)

# Step Three:修改用户密码

1. 确认你要修改密码的用户名，比如我这里要修改 ‘babyfengfjx’ 用户的密码；
2. 在上面的终端里，接着执行：``passwd babyfengfjx``    -- 这里记得换成自己的用户名；
3. 按照提示设置新的密码即可。

![genghuanmima.gif](https://storage.deepin.org/thread/202206011319569555_genghuanmima.gif)

# Step Four:重启系统，使用新的密码登录

完成上述操作后，即可重启系统，使用刚设置的新密码进行登录了。