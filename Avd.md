## 这里是一些关于安卓开发的相关问题
[来自论坛](https://bbs.deepin.org/forum.php?mod=viewthread&tid=138244&extra=)
## 无法启动AVD
- 解决方法：
    
         apt install lib64stdc++6
         cd ~/Android/Sdk/emulator/lib64/libstdc++
         mv libstdc++.so.6 libstdc++.so.6.bak
         ln -s /usr/lib64/libstdc++.so.6 ~/Android/Sdk/emulator/lib64/libstdc++                
               
## AVD没有声音（旧版系统）

- 解决方法：
1. 在深度商店里下载PulseAudio
2. 启动AVD
3. 打开PulseAudio
4. 把qemu设备的静音取消

## AVD没有声音（新版系统不需要下载PulseAudio）

- 解决方法：
1. 启动AVD
2. 单击音量图标
3. 把AVD应用的静音取消