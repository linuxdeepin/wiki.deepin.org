# Debian发行版基础系列四:debian-cd 工作原理分析(尘封归档篇)

debian-cd 是debian的官方的制作ISO的工具。

## 安装光盘的组成

```
autorun.inf         # windows启动引导支持(可以移除) 
setup.exe           # windows启动引导支持(可以移除) 
win32-loader.ini    # windows启动引导支持(可以移除)
g2ldr.mbr           # windows启动引导支持(可以移除)
g2ldr               # windows启动引导支持(可以移除)

debian              # 链接文件，指向当前目录
css                 # ISO启动引导主题文件 
pics                # ISO启动引导背景图片

efi/                # EFI引导文件
boot/               # GRUB引导文件 
isolinux/           # ISO启动项菜单 
install             # 空目录
install.amd/        # vmlinuz, initrd.gz 文件 
dists/              # ISO内置仓库索引
pool/               # ISO内置仓库数据目录
firmware            # firmware驱动软件包
preseed.cfg         # 自动应答规则文件
custom.postinstall  # 自动应答规则文件对应执行的脚本
md5sum.txt          # ISO文件MD5校验值清单
.disk/              # ISO标志信息
```

## debian-cd 工作流程

* 初始化工作环境，建立相应目录，创建的目录示例如下:

    ```
    * mkdir -pv deepin-cd/build/15/{apt,archive-keyring,kui} #用于存放制作ISO需要的全部文件，软件包等等
    * mkdir -pv deepin-cd/output/15/                         #用于存放直至完成的ISO文件，和输出结果
    ```
    
* 依据定义的task来完成的包列表的分析，从本地仓库获取软件包，在示例中build目录 (deepin-cd/build/15/kui/..) 下创建新的仓库，并检查新建仓库的内的全部软件包是否存在依赖缺失(由于debian-cd这部分实现过于复杂，臃肿，难以维护，目前功能已经被isobuilder取代，直接使用isobuilder创建的本地仓库)
* 执行/tools/make_disc_trees.pl 创建构建ISO需要的目录树，包括启动引导文件等，实际执行动作类似如下示例：

    ```
    /data/codes/project/isobuilder/deepin-cd/tools/make_disc_trees.pl                                                              \
    /data/codes/project/isobuilder/deepin-cd /data/isobuilder/mirror /data/codes/project/isobuilder/deepin-cd/build/15 kui "amd64" \
    "xorriso" "-as mkisofs -r -checksum_algorithm_iso md5,sha1,sha256,sha512 "
    ......
    tools/boot/kui/boot-amd64 1 /data/isobuilder/deepin-cd/build/15/kui/CD1
    ```

* 生成ISO内文件MD5值清单,参考命令如下

    ```
    find . -type f | grep -v -e ^\./\.disk -e ^\./dists | xargs md5sum >> md5sum.txt
    ```

* 最后调用xorriso生成ISO文件，将结果保存在`deepin-cd/output/15/`目录,实际执行动作类似如下示例：

    ```
    xorriso -as mkisofs -r -checksum_algorithm_iso md5,sha1,sha256,sha512 -V 'Deepin Server 15 amd64 1'           \
        -J -isohybrid-mbr syslinux/usr/lib/ISOLINUX/isohdpfx.bin                                              \
        -J -joliet-long -cache-inodes                                                                         \
        -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot                                           \
        -boot-load-size 4 -boot-info-table -eltorito-alt-boot                                                 \
        -e boot/grub/efi.img -no-emul-boot -isohybrid-gpt-basdat -isohybrid-apm-hfsplus boot1 CD1             \
        -o /data/codes/project/isobuilder/deepin-cd/output/15/deepin-server-15-general-amd64-DVD-20160801.iso \
    ```

## 其他

debian-cd 关键代码如下:

* tools/apt-selection        对APT工具进行封装，使用独立的配置目录和配置文件来获取软件包
* tools/boot/kui/boot-amd64  生成启动项配置
* tools/make_disc_trees.pl   生成ISO目录结构树和准备相应的文件 

## 基于grub2封装可引导ISO

```
xorriso -as mkisofs -graft-points --modification-date=2017010903552500 -b boot/grub/i386-pc/eltorito.img -no-emul-boot -boot-load-size 4 -boot-info-table --grub2-boot-info --grub2-mbr /usr/lib/grub/i386-pc/boot_hybrid.img --efi-boot boot/grub/efi.img -efi-boot-part --efi-boot-image --protective-msdos-label -o test.iso --sort-weight 0 / --sort-weight 1 /boot ISO/.
```

## 上游参考文档
* <http://lukeluo.blogspot.sg/2013/06/grub-how-to-2-make-boot-able-iso-with.html>
