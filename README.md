# GPDMicroPC2BootRotate
# 在安装了limine启动器的GPD Micro PC 2上旋转屏幕方向
# 只适用于Limine启动器

环境：CachyOS+Limine+原生竖屏的显示器<br>
增加内核参数，使竖屏显示的界面改为横屏，方便操作<br>
# 第一种方法，这种更新后会掉设置：
在limine.conf中增加如下内容：<br>
1.在文件开始部分加入：interface_rotation:90<br>
2：在cmdline行中增加 fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up,可以用定义的方式：<br>
--定义，这个也放到开始位置：${addedconf}=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up<br>
--使用，在对应的cmdline行后面加上：${addedconf}<br>

# 第二种方法：<br>
参考https://wiki.archlinux.org.cn/title/Limine 第6.1.1配置<br>
首先查看/proc/cmdline的内核参数，确认原始信息<br>
在/etc/kernel/cmdline中增加上述横屏内容：<br>
+=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up<br>

备注<br>
cmdline相关的位置与设置：<br>
1.如果不存在 /etc/limine-entry-tool.conf，请将其复制到 /etc/default/limine<br>
2./proc/cmdline 为当前的mcdline<br>
3./etc/kernel/cmdline可能开始没有<br>

查找文件位置<br>
sudo find /boot -maxdepth 4 -type f -name limine.conf -print<br>

注意： /etc/default/limine 具有最高优先级，并覆盖所有嵌入配置。因此，在附加内核参数时，建议使用 +=。示例：<br>
```KERNEL_CMDLINE[default]+=rw root=UUID=... ```<br>
```KERNEL_CMDLINE[default]+=quiet splash initrd=/amd-ucode.img```<br>

# 中文输入法安装<br>
```sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-configtool fcitx5-gtk fcitx5-qt```<br>
编辑：/etc/environment<br>
增加```XMODIFIERS=@im=fcitx``` for XWayland application<br>
其他的按照：https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#KDE_Plasma的说法，不要动<br>
