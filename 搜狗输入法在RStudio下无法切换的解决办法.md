##简介
本经验有深度官方人员分享。

##正文
	

解决办法为：
```dpkg-deb -R  fcitx-frontend-qt5-rstudio_1.0.5-1_amd64.deb    fcitx-frontend-qt5-rstudio (解压deb包)```

然后把解压出来的文件fcitx-frontend-qt5-rstudio/usr/lib/rstudio/　　目录下面的对应文件拷贝到系统/usr/lib/rstudio/目录下面

同理
```dpkg-deb -R  libfcitx-qt5-1-rstudio_1.0.5-1_amd64.deb  libfcitx-qt5-1-rstudio(解压deb包)```

把解压目录对应的libfcitx-qt5-1-rstudio/usr/lib/rstudio/  拷贝到/usr/lib/rstudio目录下面重新启动rstudio即可。

原理是如果dpkg安装会检测依赖关系，有依赖问题则无法安装，这个手动安装不用管依赖问题。