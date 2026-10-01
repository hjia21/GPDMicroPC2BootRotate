# GPDMicroPC2BootRotate
# 在安装了limine启动器的GPD Micro PC 2上旋转屏幕方向
# 只适用于Limine启动器

环境：CachyOS+Limine+原生竖屏的显示器<br>
增加内核参数，使竖屏显示的界面改为横屏，方便操作<br>
在limine.conf中增加如下内容：<br>
1：interface_rotation:90<br>
2：在cmdline中增加 fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up<br>
--定义：${addedconf}=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up<br>
--使用：${addedconf}<br>

# 第二种方法：
参考https://wiki.archlinux.org.cn/title/Limine<br>
首先查看/proc/cmdline的内核参数，确认原始信息<br>
在/etc/kernel/cmdline中增加上述横屏内容<br>

search file location<br>
sudo find /boot -maxdepth 4 -type f -name limine.conf -print

注意： /etc/default/limine 具有最高优先级，并覆盖所有嵌入配置。因此，在附加内核参数时，建议使用 +=。示例：<br>
```KERNEL_CMDLINE[default]+=rw root=UUID=... ```<br>
```KERNEL_CMDLINE[default]+=quiet splash initrd=/amd-ucode.img```<br>
